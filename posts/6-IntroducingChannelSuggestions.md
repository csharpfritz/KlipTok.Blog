---
title: Introducing Channel Suggestions
author: Jeff Fritz
draft: false
publishdate: 2022-05-10T21:11:00.9764593-04:00
---

Our team that's been working on KlipTok has no experience with building machine learning algorithms.  With some advice from friends on the [ML.NET](https://dotnet.microsoft.com/apps/machinelearning-ai/ml-dotnet) project team, they got us pointed to a few demos that would allow us to start using the power of machine learning to make more interesting interactions with KlipTok.

![The new suggested streamers panel on KlipTok, generated using ML.NET](img/601-MachineLearningGeneratedSuggestedStreamers.png)

ML.NET makes it easy for .NET developers to build, manage, and consume machine-learning models.  We believe can unlock some interesting insights from the KlipTok data using this framework.

In particular, we went through the ["Movie Recommendation" tutorial](https://docs.microsoft.com/dotnet/machine-learning/tutorials/movie-recommendation) and immediately identified some changes that we could implement in KlipTok.  In particular, let's use a similar technique to suggest channels based on the channels you already follow on Twitch.

Let me explain how we use ML.NET to recommend channels for KlipTok.

## Building a model to recommend channels

The demo in the "Movie Recommendation" tutorial analyzes ratings of movies and makes recommendations to you based on similar ratings of the same movie.  We can take this concept and simplify it to the shopping cart scenario:

> Hey, you just put **Product X** in your cart and lots of folks that buy **Product X** also buy **Product Y**.  Would you like to also add **Product Y** to your cart?

Fortunately, the tutorial has a link to some sample code that covers exactly this scenario, referred to as ["One Class Matrix Factorization"](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/MatrixFactorization_ProductRecommendation).

The data for this becomes very simple:  we just need two fields in our records.  One field contains the id of the user who is following the id of the channel in the second field.  I built a small class called `FollowerDataForAnalysis`

```csharp

	public class FollowerSummaryForAnalysis
	{

		public string UserId { get; set; }

		public string ChannelId { get; set; }

	}

```

I might have been able to create this as a C# `record` object, but I'm going to run with this class for now.

The ML.NET object that we use to build models and work with them is called the MLContext.  In my case, I created an `MLContext` and load these `FollowerSummaryForAnalysis` data points from my database.  I then configure the datapoints in the MLContext for analysis:

```csharp

  IEnumerable&lt;FollowerSummaryForAnalysis&gt; followersAnalysis = LoadFromDatabase();

  var trainingDataView = _Context.Data.LoadFromEnumerable(followersAnalysis);

  var estimator = _Context.Transforms.Conversion.MapValueToKey(outputColumnName: "userIdEncoded", inputColumnName: nameof(FollowerSummaryForAnalysis.UserId))
    .Append(_Context.Transforms.Conversion.MapValueToKey(outputColumnName: "channelIdEncoded", inputColumnName: nameof(FollowerSummaryForAnalysis.ChannelId)));

```

Next, I need to define some options for the model that will analyze the data.  I'm not too sure what these data points mean, but they were recommended by the tutorial.  I do know that I want it to repeat the training 3000 times in order to improve the data model.

```csharp

  var options = new MatrixFactorizationTrainer.Options
  {
      MatrixColumnIndexColumnName = "userIdEncoded",
      MatrixRowIndexColumnName = "channelIdEncoded",
      LabelColumnName = "Followed",
      LossFunction = MatrixFactorizationTrainer.LossFunctionType.SquareLossOneClass,
      Alpha = 0.01,
      Lambda = 0.025,
      NumberOfIterations = 3000,
      C = 0.00001,
      Quiet = true
  };

  var trainerEstimator = estimator.Append(_Context.Recommendation().Trainers.MatrixFactorization(options));

```

At this point, we can train the model with the `Fit` method on the `trainerEstimator` object:

```csharp

_Model = trainerEstimator.Fit(trainingDataView);

```

Finally, I save the model into a `Stream` object that persists my model into Azure Blob storage so that it can be used in Azure Functions for the KlipTok API.

```csharp

_Context.Model.Save(_Model, trainingDataView.Schema, destinationStream);

```

I currently have this model generation and save operation running once a day.

## Loading the model and serving up predictions

The model can be loaded into a `PredictionEngine` to predict a potential channel suggestion for a user.  In order to deliver that, we need to run predictions for a collection of candidate channels.  KlipTok will look at the 200 channels that most recently created clips and run those through the `PredictionEngine` for the logged in user and present a subset of 4 channels to suggest on the sidebar of KlipTok's user interface.

To enable the prediction engine to run faster, I have configured a `PredictionEnginePool` that will reuse resources and allow predictions to run significantly faster.  In testing on my local machine, it can run 200 predictions in less than 20ms. 

I added the following line to `Startup.cs` in my Azure Functions project to enable the pooling mechanism:

```csharp
  services.AddPredictionEnginePool&lt;FollowerSummaryForAnalysis,ChannelPrediction&gt;()
    .FromUri(
      uri: "https://myblobstorage.blob.core.windows.net/channelDiscoveryModel.zip",
      period: TimeSpan.FromMinutes(30)
    );
```

This tells the prediction engine to attempt to reload the model from disk every 30 minutes.

With the engine loaded, I can run predictions with this code:

```csharp
public IEnumerable<SuggestedStreamer> SuggestChannels(string userId, IEnumerable<ChannelProfile> candidateChannels, int maxCount)
{

  var outList = new List<SuggestedStreamer>();

  foreach (var channel in candidateChannels)
  {

    var input = new FollowerSummaryForAnalysis()
    {
      UserId = userId,
      ChannelId = channel.ChannelId
    };

    var score = _PredictionEngine?.Predict(input) ?? _PredictionEnginePool.Predict(input);
    outList.Add(new SuggestedStreamer
    {
      DisplayName = channel.DisplayName,
      ProfilePicUrl = channel.ProfilePicUrl,
      TwitchUserId = channel.ChannelId,
      Score = (decimal)Math.Round(score.Score, 5)
    });
  }
}
```

After this is fetched, we only transmit 20 channels to the browser.  The user interface will remove any channels that are muted and take a random subset of 4 channels to present in the sidebar.

## Summary

I was really happy with how easy it was to adapt a tutorial for ML.NET and get some relevant recommandations for the KlipTok application.  In the future, we'd like to use this technique to also recommend clips to visitors.