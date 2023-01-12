---
title: "Artificial Intelligence and Mature Content"
author: Jeffrey T. Fritz
publishdate: 2023-01-12T09:45
draft: false
description: "KlipTok has introduced new features to help detect clip thumbnails that some communities would consider too racy.  This blog posts introduces the features and discusses the scenarios where they are in effect."
---
In the time since KlipTok launched in November 2020, our intent with the content presented is that it would closely mirror the content of Twitch, its parent platform. It's been great to see that the popular games and content from Twitch appear in a similar pattern of growth and waning on KlipTok. To that end, we've seen some content appear that various communities around the world would consider racy.

We want to focus on improving the experience for the casual visitors to KlipTok.  We define 'casual visitors' as folks who browse to the website without logging in.  The casual visitor browses the site and does not want KlipTok to interact with their Twitch account or save preferences for interacting with the site.

One of the features requested for some time on the [KlipTok feedback site has been a 'mature flag'](https://feedback.kliptok.com/posts/47/mature-flag) that identifies content that could be considered risqué or racy that some folks would like to avoid. In this post, we're going to introduce a new 'racy' safety warning on KlipTok and discuss the feedback that went into the feature as well as introduce the next round of features to support this type of community standards enforcement.

## Nothing is removed from KlipTok

Let's get this clear from the start: KlipTok still delivers all the content that you would find on Twitch. This new feature is intended to make the initial visit from folks who are not logged-in to KlipTok a friendlier and more welcoming experience.

All visitors to KlipTok who are not logged-in will by default receive a more family-friendly version of the 'Discover' page that mirrors the standards of the Twitch front page. As the Twitch front page does, certain categories, will not be included in the list of clips for casual visitors. The initial restriction will be the "Pools, Hot Tubs, and Beaches" category. Once a user logs in to KlipTok, clips from this category may appear in the 'Discover' page. This category filtering is only applied to the 'Discover' page for casual visitors. If you are following channels with content in this category, those clips will continue to appear in your 'Following' tab and the individual channel's pages.

### Additional Detection

We all know that folks like to broadcast on different categories with content that crosses the boundaries. To help detect content that might be a little racy for some folks, we have introduced a thumbnail detection feature using [Microsoft Azure's Computer Vision and Adult Content Detection feature](https://learn.microsoft.com/azure/cognitive-services/computer-vision/concept-detecting-adult-content). Clips that are considered for the 'Discover' page on KlipTok will have their thumbnails analyzed by this feature and the **racy** score attached.

The Computer Vision feature [defines racy images](https://learn.microsoft.com/azure/cognitive-services/computer-vision/concept-detecting-adult-content#content-flag-definitions) as "... sexually suggestive in nature and often contain less sexually explicit content than adult images (adult images are those containing nudity and sexual acts)"

For casual visitors to the KlipTok 'Discover' page, they will now receive a content warning and a blurred thumbnail for those clips identified with 90% or higher confidence to contain racy material. Additionally, they will be presented with a lock icon that requires a click to reveal the original thumbnail.

<img alt="Sample thumbnail detected as racy" src="img/racy-sampleRacyClip.png" style="margin: 2em auto; display: block; max-height: 300px;" />

This blurring and content warning is only presented on the 'discover' page for users who are not logged in.  On the 'following' page and on individual channel pages, this content is presented as is with no blurring or content warnings added.

## Future Considerations

We believe that adding this type of initial 'safe-search' capability to the 'Discover' page means that KlipTok is a little friendlier for new visitors to the site. We acknowledge that not everyone wants to see all the different types of content on KlipTok, and that's ok.

In the future, we want to make it easier for users to mute categories and adjust the racy indicator threshold to their preference for clips presented on the 'Discover' page. You may want to hide content from categories like ASMR or Casinos, or perhaps you would hide categories to help elevate the presence of the categories you do like. You may want to allow a little more racy content to appear on the front-page, or you may not want any racy content to appear.

You will eventually find these controls in the [user-profile page of KlipTok](https://kliptok.com/profile) with your other preferences like 'muted streamers'.

These are your preferences, and we want to be transparent about giving you the KlipTok viewer the control that you want over the content we share with you.
