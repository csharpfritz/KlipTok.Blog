---
title: Pin Channels and Performance Updates
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

## Patching records in RavenDb
