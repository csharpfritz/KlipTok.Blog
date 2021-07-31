---
title: Most Requested Feature Deployed - Live Now on Twitch
draft: false
publishdate: 2021-07-31T16:00
---

Over the last few months, the most requested feature on KlipTok on the [feedback site](https://feedback.kliptok.com) has been the addition of a "Live Now" indicator for the list of channels.

<div class="center">

[![Screenshot from feedback.kliptok.com - Request to add a 'Live' indicator next to channels](img/feedback-LiveIndicator.png)](https://feedback.kliptok.com/posts/23/live-now-icon-next-to-the-channels-name-in-the-list-of-streamers)

<span aria-label="caption" class="caption">Original feedback requesting the Live Indicator</span>

</div>

On Twitch, its a simple red circle.. the circle doesn't really tell you anything, and you need to hover over it in order to see what it actually means.  On KlipTok, we want to be more inclusive and support folks that have different web usage capabilities.  

Our 'Live Now' indicator is a red rounded square with the word 'LIVE' in the middle.  It's easy to spot and clear what it's referring to.  If you click on it, you'll be taken to their channel in a new tab of your browser.

You can even use the sidebar refresh buttons, the spinning arrows next to the titles in the sidebar, to refresh the list of current channels that KlipTok has for you and it will update the Live status appropriately as well.

<div class="center">

![Screenshot of KlipTok showing the new LIVE indicator in the sidebar](img/liveIndicator.png)

<span aria-label="caption" class="caption">Implementation of the Live Indicator on KlipTok</span>

</div>

## Behind the feature - How does it work?

As more of KlipTok is built, we're going to use this as an opportunity to __TEACH__ more about how to build websites and the features behind them.  Going forward, look for descriptions about the KlipTok architecture, but not all of our secrets, whenever a new feature is released.  

### Getting the Live Status from Twitch

The [Twitch APIs](https://dev.twitch.tv/docs/api/reference#get-streams) make it easy to run a query to collect the current state of a stream.  When KlipTok's background processes run to discover newly created clips, they inspect the list of currently active streams and ONLY search those streams for new clips.  We know that more than 95% of the clips that are created for a stream are created while the broadcaster is actively streaming, so we focus on collecting those clips as they are created.  Once an hour, we examine ALL channels that KlipTok has indexed.

Since we were collecting this data about the active streams in order to filter the list of channels that the KlipTok processes were searching against, why not save that information and present it to the KlipTok users as they use the site?  Easy enough... we created a `LiveChannels` table and stored the id of the channels that are currently streaming.

I wrote a method on our `LiveChannelsRepository` class that would `Replace` the current contents of the table with the collection of the live channels we discovered.  It's crude, but it works.

```csharp
	await _LiveChannelsRepository.Replace(
		liveChannels.Select(l => new Twitch.TwitchStreamRecord
		{
			user_id = l.ChannelId,
			started_at = l.LiveSince
		})
	);
```

When we assemble the sidebar for a user, we include the `LiveChannels` table in the query and present the Live Indicator when a record is present in the `LiveChannels` table for the channel in the sidebar.

## Summary

Adding this feature was a great re-use of existing data that KlipTok was already working with, and feels almost like re-using content with no additional cost to us.  Stay tuned as we add new Search features, clip metadata, and launch the Top 5 Klips of the Week in August.