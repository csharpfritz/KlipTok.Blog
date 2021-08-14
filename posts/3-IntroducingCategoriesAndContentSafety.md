---
title: Version 0.23 - Introducing IAB Categories and Content Safety
draft: true
publishdate: 2021-08-14T15:00:57.0057601-04:00
---

This week we are happy to announce the release of Version 0.23 with a number of small fixes, updated translations, a new translation for Slovenian, and a new feature: IAB Categories and Content Safety tags for transcribed clips.  I'm definitely burying the lede here, but I think it's worth it to give you a quick summary of what's new.

## IAB Categories and Content Safety

With our partners at [Assembly AI](https://assemblyai.com), we've introduced IAB Categories and Content Safety tags to clips.  You'll recall that we added clip transcriptions to KlipTok in July and made those transcriptions available to search.  We're the only website on the internet where you can search for videos based on the dialogue in the video.  

<< INSERT IMAGE OF SEARCH RESULTS >>

The [Interactive Advertising Bureau (IAB)](https://www.iab.com) has a set of categories that you can use to categorize your videos.  They're a great way to make sure that your videos are appropriate for children and young people.  A [full list of categories](https://www.iab.com/guidelines/video-ad-categories/) is available online.  We've added the categories to KlipTok and you can see them tagged on clips that have been transcribed.

<< INSERT IMAGE OF IAB CATEGORY TAGS >>

AssemblyAI provides a feature to [detect the IAB Categories](https://docs.assemblyai.com/guides/iab-categorization) of your videos based on their transcript.  These categories can be VERY accurate... and sometimes a little funny in what they are guessing your clips are about.  KlipTok is only going to show the first 4 categories with more than 60% confidence that it thinks your video falls into.  There's a hidden tooltip on each of the tags showing the confidence percentage reported by AssemblyAI's detection algorithm for that category on the clip.

### Content Safety tags

The second AssemblyAI detection feature we are implementing is the [detection of content safety](https://docs.assemblyai.com/guides/content-safety-detection) warnings.  This feature uses the same transcription analysis and detection request to detect foul language and other topics that may require content safety warnings in various cultures.  These tags are added with a red background to separate them from the category tags.  Just like the IAB category tags, you can inspect the hidden tooltip on each tag to see the confidence percentage reported by AssemblyAI for each tag.

## Translation Updates

Translations for KlipTok (to date) have been created and maintained by the user community.  We saw a few updates over the last week for Farsi, Finnish, German, Indonesian, Italian, and Polish translations.  Additionally, Slovenian was added to our roster of 23 languages supported by KlipTok.

Finally, we have updated the translation combobox to list each supported language in their native language to make it easier to locate and translate KlipTok to your preferred written language.

<< INSERT TRANSLATION COMBOBOX IMAGE >>

## Summary



