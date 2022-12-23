---
title: Milestone Reached - 4 MILLION Clips Indexed!
author: Jeff Fritz
draft: false
publishdate: 2021-08-19T21:00
---

KlipTok just crossed another major milestone on the evening of August 18th:

<div class="center">

![4 million clips on the KlipTok dashboard](img/4million.PNG)

</div>

We're now service more than 4 million klips for about 750 Twitch channels with more than 11k transcriptions.  It's a huge step, and is stored on about 10GB of storage for the database and search indexes.

## How are those 4 million clips stored and indexed?

The clips are stored in a MySql database hosted on Microsoft Azure that allows us to join in and query based on the channel id or the unique 'slug' text that identifies the clip.

There are entries in [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) for the titles of the clips and the transcriptions of the clips.  This table storage is further indexed by [Azure Cognitive Search](https://azure.microsoft.com/services/search/) so that you can search for clips based on the title of the clip or the words spoken in the clip.

I use [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) to process all of these records as they arrive from Twitch, get processed by AssemblyAI for transcripts, and store the clip data for processing and search indexing.

That's... that's a lot of storage to manage and indexes to update.  What if we could make that a lot more concise and simpler?

## Enter RavenDb

Starting next week, we're going to start migrating data to RavenDb.  [RavenDb](https://ravendb.net) is a NoSQL database provider that has a bunch of smart features built into it that allow it to detect and build indexes appropriately for our queries.  It also has a built in search indexing feature that allows us to generate a search index and search fields in document records.

Additionally, I have found and witnessed a few demos that show how to build an elementary recommendation engine using RavenDb queries.  When I saw these demos, I was sold on being able to build and roll out a better list of recommended channels and clips to discover on the front page of KlipTok.

RavenDb feels like a perfect match for the various ways that we store and interact with clip data for KlipTok.  I hope you tune in to the [csharpfritz Twitch stream](https://twitch.tv/csharpfritz) in the week ahead as I migrate and roll out this update.