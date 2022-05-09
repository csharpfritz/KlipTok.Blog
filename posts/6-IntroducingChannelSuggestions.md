---
title: Introducing Channel Suggestions
draft: true
publishdate: 2022-05-08T21:10:00.9764593-04:00
---

Our team that's been working on KlipTok has no experience with building machine learning algorithms.  With some advice from friends on the [ML.NET](https://dotnet.microsoft.com/apps/machinelearning-ai/ml-dotnet) project team, they got us pointed to a few demos that would allow us to start using the power of machine learning to make more interesting interactions with KlipTok.

ML.NET makes it easy for .NET developers to build, manage, and consume machine-learning models.  We believe can unlock some interesting insights from the KlipTok data using this framework.

In particular, we went through the ["Movie Recommendation" tutorial](https://docs.microsoft.com/dotnet/machine-learning/tutorials/movie-recommendation) and immediately identified some changes that we could implement in KlipTok.  In particular, let's use a similar technique to suggest channels based on the channels you already follow on Twitch.

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

I might have been able to create this as a C# `record` object