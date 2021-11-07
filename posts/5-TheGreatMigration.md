---
title: The Great Migration - Introducing NoSQL and RavenDb
draft: false
publishdate: 2021-11-07T10:32:16.0194591-05:00
---

It's been a long few months since I last posted an update about the state of KlipTok... and we've learned a lot in those three months.  In August, shortly after the last post I discovered that at our current growth rate I would run out of space in our Azure Search subscription.  To use the next step up in search subsciption would have tripled the costs of KlipTok, and that was not a service charge that I could accept.

I explored a few options to migrate Search to, and also saw the single instance of MySQL database in use for KlipTok as a single-point-of-failure.  Was there an option available that could solve both problems?

The answer is yes... because that week in August I received a message from Oren Eini on LinkedIn about RavenDb seeing major improvements since updating to .NET 5.  I wanted to learn more, and thought this could be my way forward.

## The Architecture Problem

KlipTok stores a lot of data.  It doesn't currently store video for clips from Twitch, but rather sticks to its principles about driving viewership back to Twitch and instead stores data about the clips it has discovered on Twitch.  KlipTok helps Twitch by making those clips more discoverable, and that means storing information about millions of clips and someday billions of clips.

All of those clips were initially stored in Azure Table Storage.  It was a decent repository for the first million or so records, but the latency and changes to the data made this storage service costly.  I migrated data to a trusted and well-known database, MySQL.  With lower and more predictable costs, things felt good and I knew how to work with the relational database technology.

### Search channels feature

I wanted to make it possible to search channels on the application, and the easiest way that I saw available to me was to use the Azure Cognitive Search service.  There's a free instance that would let me store up to 50MB, and that would be PLENTY of space for me to store information about hundreds of thousands of channel names that we could allow KlipTok users to search through.  But searching channels doesn't help users discover content, and I wanted more.

I decided to start indexing the titles of the clips that were on KlipTok.  This would be an easy process, stashing the title and id for the clip in Azure Search.  With millions of clips already on KlipTok, I would need to configure a separate search service.  This new service would be larger than the free instance that I previously had, with a maximum size of 2GB.  That should hold until we hit about 6 million clips.

In August, when we crossed 4 million clips, I saw velocity increasing and knew that we would need the next larger Search service instance and that growth trajectory was not going to work with the additional fees and limits that would come with it.

### Database Redundancy

At roughly the same time I recognized the search issue, I was having some read/write lock contention in the MySQL database.  To remedy the situation, I stood up a read-only replica of the database and configured KlipTok to read all records from the replica and write all records to the read/write instance.  This separation of features helped the performance of the application and gave me some confidence that I had a warm backup of the database available should I lose the primary instance.  It did however also mean that my database costs just doubled.

Uh oh...

## Enter RavenDb


