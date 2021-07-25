---
draft: false
title: Welcome to KlipTok!
publishdate: 2021-07-24 14:22
---

This is our first post on the KlipTok blog, and I'm thrilled to have this live for folks to learn more about how KlipTok is built and to allow us to announce new features as well as milestones reached.  I'll have a few more posts today and later this week talking about what KlipTok is, how it was built, and how you can participate.

## How it started

KlipTok has been built by a Twitch Streamer, for Twitch Streamers, LIVE on Twitch.  It started in November 2020 as an idea to help make Twitch clips more discoverable with a UI that brought your favorite clips from the channels you follow.  That 12-hour stream is archived and available:

<div style="width: 100%; text-align: center;">
<iframe width="560" height="315"  src="https://www.youtube.com/embed/6uMCAHEM8Es?start=310" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

The technical goal of this web application was to build a complete site using the [Blazor web framework](https://blazor.net) with [C# and .NET 5](https://dot.net) running completely on [Microsoft Azure](https://azure.microsoft.com).  We use Azure Static Websites to make that happen along with a number of other Microsoft Azure services, and we'll cover more of that in an upcoming blog post.

## Fast Forward to Today

At the time of the writing of this blog post (at the end of July 2021), we're about 8 months into the evolution of the site and it hosts about 680 channels clips.  There are more than 3.1 million clips that KlipTok has indexed and are available for you to discover.

We've added the ability for the site to be translated to a number of different languages with the help of our KlipTok community.  The translations are all available in JSON format [on GitHub](https://github.com/csharpfritz/KlipTok.Translations) and you are welcome to contribute new or missing translations.

Viewer and user feedback is VERY important to me in the construction of KlipTok, and I've configured a user feedback website using [Fider](https://getfider.com).  You can see the requested features and my notes about the next features that are going to be built at https://feedback.kliptok.com

I'm writing a handful of posts to get things started here to describe some of the direction behind the site, the tenets we follow as development progresses, and the Azure-based architecture used.

I hope to separate the Blazor application into its own repository and release the UI as an open source project before the end of July 2021.  In this way, you can learn from how the application was written and even contribute back some updates to improve the user-interface.

## One more thing...

I've added an amazing feature to KlipTok with the help of the folks at [Assembly AI](https://assemblyai.com) to provide transcription for clips.  This means you can search for clips based on what was SAID in the clip, not just the title of the clip.  This is the ONLY video sharing service on the internet that provides this service, and we have much more planned with our friends at Assembly AI.

## Summary

So.. welcome to KlipTok!  I'm looking forward to building much more for this web application and learning about how to use many more cloud services and taking advantage of the coolest new .NET technologies to grow it and make it an application you find useful.