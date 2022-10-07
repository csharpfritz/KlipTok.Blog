---
title: Pin Channels and Performance Updates
author: Jeff Fritz
draft: false
publishdate: 2022-06-19T09:11:32.1193557-04:00
---

The "secret sauce" of KlipTok has been that it is a service for streamers, written by a streamer, live on stream. This week, we added a new wrinkle to that mix.  We added a feauture requested by viewers, designed by viewers.

## Pin Channels to the Sidebar

On Tuesday the 14th, we received [a feedback entry for the most requested feature](https://feedback.kliptok.com/posts/109/add-feature-pinned-streamers) to date for KlipTok:  Pin Channels to the sidebar. 

[![Feedback entry for Pin Channels](img/7_1-Feedback.png)](https://feedback.kliptok.com/posts/109/add-feature-pinned-streamers)

It's an idea that makes sense: allow viewers to pin their favorite 3 channels to the sidebar so that they can quickly jump to those channels.  The last part of the request, add these channels to the front page of KlipTok, is a little more complicated and not delivered yet.

You can now find the **Pin Channel** feature in the _kabob menu_ on the sidebar and with the pin icon in the header of every channel on KlipTok.  You can pin a channel and unpin channels with these two buttons.

![Pinned channels on the sidebar with the context menu to unpin](img/7_2-pinned.png)


![Pin icon in the header of the streamer page](img/7_3-pin-header.png)

We're evaluating and preparing to add the last requirement of this feature, the 'missed clips on homepage'

## Performance Updates

Additionally, we tuned the interactions the sidebar and streamer pages were having with the database services.  For the sidebar, we moved queries for the sections to a parallel request model.  On streamer pages, we tuned the interaction the clip list component was using so that it no longer blocks rendering and makes 1 less request to the database service for content.

Another update that didn't make a significant change in performance for this feature, but I want to highlight was the conversion of the pin action from an update to a patch.  Before I explain the difference, let's take a look at how the new **Pinned Channels** feature is stored in the database.

## Pinned Channels data schema

Pinned channels are a feature that only logged in users can use.  As such, those logged in users also have a collections of channels the follow on Twitch that are maintained in the KlipTok database.  By Twitch's definition, a [user can follow up to 2000 channels on the Twitch service](https://blog.twitch.tv/en/2013/03/26/on-channel-following-limits-33997332dd34/).

In KlipTok, we store that collection of channels so that each user has 1 document with the channels they follow stored in an array like so:

```json
{
   "ChannelIds": [
      "Channels/102402222",
      "Channels/32402099",
      "Channels/63208102",
      "Channels/52093029"
   ]
}
```

This lets me run the following query in C# against our [RavenDb database](https://ravendb.net) to return the collection of Channels that a user follows:

```csharp
using var session = Store.OpenAsyncSession();
var followRecord = await session.LoadAsync&lt;FollowerRecord&gt;(
    "Followers/96909659",
    i => i.IncludeDocuments(f => f.ChannelIds)
);

var followedChannels = await session.LoadAsync&lt;ChannelProfile&gt;(followRecord.ChannelIds);
```

The first query loads identifies the record in the `FollowRecords` collection immediately as it is loading that record using a document id.  In a relational database, you would call the document id a primary key.  The include of the ChannelIds using the `IncludeDocuments` method will fetch all of the `ChannelProfile` records into .NET memory that are referenced in the `ChannelIds` property.  In the case of our sample data above, we load 4 `ChannelProfile` records at the same time as the `FollowRecord` object from RavenDb into .NET memory.

The final query that populates the `followedChannels` variable from those `ChannelProfile` objects already stored in memory.

To add the *PinnedChannels* feature, we simply added another collection of ChannelIds to the `FollowRecord` object.  This means that sample data from above now looks like the following:

```json
{
   "ChannelIds": [
      "Channels/102402222",
      "Channels/32402099",
      "Channels/63208102",
      "Channels/52093029"
   ],
   "PinnedChannelIds": [
      "Channels/221740520"
   ]
}
```

We can now use a similar query to fetch the profiles of those pinned channels.

```csharp
using var session = Store.OpenAsyncSession();
var followRecord = await session.LoadAsync&lt;FollowerRecord&gt;(
    "Followers/96909659",
    i => i.IncludeDocuments(f => f.PinnedChannelIds)
);

var pinnedChannels = await session.LoadAsync&lt;ChannelProfile&gt;(followRecord.PinnedChannelIds);
```

Easy...  and we can update the `PinnedChannelIds` collection with a simple *Patch* command:

```csharp
followRecord.PinnedChannelIds = new string[] { "Channels/148746662", "Channels/221740520" };
session.Advanced.Patch&lt;FollowerRecord, IEnumerable&lt;string&gt;&gt;(
    "Followers/96909659",
    f => f.PinnedChannelIds,
    newPinned);
await session.SaveChangesAsync();
```

In a relational database, this *patch* command would feel like an update statement that updates a record based on its primary key.

## What's next for pinned channels?

The last part of the pinned channels feature request was to populate the first clips that you find on the KlipTok home page with the _latest clips_ that you have not seen from the channels that you have pinned.  This is a tricky change that involves tracking which clips you watch and using that to determine which clips to add to the home page.

I'm working through the schema adjustments for this, as well as the privacy implications of tracking this information.  I'll have more to share as that feature completes over the next week.

## Looking forward.. to the KlipTok mobile app

I'm getting very comfortable working with [.NET MAUI](https://docs.microsoft.com/dotnet/maui/what-is-maui) and have made some initial steps into building a mobile app to complement KlipTok.  I hope to move into working full-time on the mobile app in July.

The playlists feature has received some cool updates and we're lookng at completing a rollout of a borderless playlist player that you can use with OBS or other tools to create a 'be right back' or 'intro' screen that shows some highlights for you.  We're [planning an integration](https://feedback.kliptok.com/posts/107/streamelements-widget-that-embeds-playlists) with [Stream Elements](https://streamelements.com) so that you can add a playlist widget to your StreamElements layouts.

We're also planning updates to the notifications system as well as some user-interface updates around the login process.  Look for these in the August / September timeframe.

How do you use KlipTok?  What can we do to make it more interactive for you and other viewers?  Vote on the [feedback site](https://feedback.kliptok.com), add your comments to existing items, or tweet us at [twitter.com/thekliptok](https://twitter.com/thekliptok)
