---
title: Milestone Reached - 4 MILLION Clips Indexed!
draft: false
publishdate: 2021-08-19T18:26:35.2829183-04:00
---

KlipTok just crossed another major milestone on the evening of August 18th:

<div class="center">

![4 million clips on the KlipTok dashboard](img/4million.PNG)

</div>

We're now service more than 4 million klips for about 750 Twitch channels with more than 11k transcriptions.  It's a huge step, and is stored on about 10GB of storage for the database and search indexes.

## How are those 4 million clips stored and indexed?

The clips are stored in a MySql database hosted on Microsoft Azure that allows us to join in and query based on the channel id or the unique 'slug' text that identifies the clip.

There are entries in [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) for the titles of the clips and the transcriptions of the clips.  This table storage is further indexed by Azure Cognitive Search so that you can search for clips based on the title of the clip or the words spoken in the clip.

That's... that's a lot of storage to manage and indexes to update.  What if we could make that a lot more concise and simpler?

## Enter RavenDb

Starting next week, we're going to start migrating data to RavenDb.  RavenDb is a NoSQL