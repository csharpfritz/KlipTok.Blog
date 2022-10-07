---
title: Version 0.23 - Introducing IAB Categories and Content Safety
author: Jeff Fritz
draft: false
publishdate: 2021-08-14T14:15
---

This week we are happy to announce the release of Version 0.23 with a number of small fixes, updated translations, a new translation for Slovenian, and a new feature: IAB Categories and Content Safety tags for transcribed clips.  I'm definitely burying the lede here, but I think it's worth it to give you a quick summary of what's new.

## IAB Categories and Content Safety

With our partners at [Assembly AI](https://assemblyai.com), we've introduced IAB Categories and Content Safety tags to clips.  You'll recall that we added clip transcriptions to KlipTok in July and made those transcriptions available to search.  We're the only website on the internet where you can search for videos based on the dialogue in the video.  

<div class="center">

![Search for 'Thanks Amanda' and finding the clip](img/transcription_search.png)

<span aria-label="caption" class="caption">Search for 'Thanks Amanda' and finding the clip where I say "Thanks Amanda"</span>

</div>

The [Interactive Advertising Bureau (IAB)](https://www.iab.com) has a set of categories that you can use to categorize your videos.  They're a great way to make sure that your videos are appropriate for children and young people.  A [full list of categories](https://www.iab.com/guidelines/video-ad-categories/) is available online.  We've added the categories to KlipTok and you can see them tagged on clips that have been transcribed.

<div class="center">

![IAB Category Tags and Content Safety Tags for a clip](img/IabAndSafetyTags.PNG)

<span aria-label="caption" class="caption">IAB Category Tags and Content Safety Tags for a clip</span>

</div>

AssemblyAI provides a feature to [detect the IAB Categories](https://docs.assemblyai.com/guides/iab-categorization) of your videos based on their transcript.  These categories can be VERY accurate... and sometimes a little funny in what they are guessing your clips are about.  KlipTok is only going to show the first 4 categories with more than 60% confidence that it thinks your video falls into.  There's a hidden tooltip on each of the tags showing the confidence percentage reported by AssemblyAI's detection algorithm for that category on the clip.

### Content Safety tags

The second AssemblyAI detection feature we are implementing is the [detection of content safety](https://docs.assemblyai.com/guides/content-safety-detection) warnings.  This feature uses the same transcription analysis and detection request to detect foul language and other topics that may require content safety warnings in various cultures.  These tags are added with a red background to separate them from the category tags.  Just like the IAB category tags, you can inspect the hidden tooltip on each tag to see the confidence percentage reported by AssemblyAI for each tag.

## Translation Updates

Translations for KlipTok (to date) have been created and maintained by the user community.  We saw a few updates over the last week for Farsi, Finnish, German, Indonesian, Italian, and Polish translations.  Additionally, Slovenian was added to our roster of 23 languages supported by KlipTok.

Finally, we have updated the translation combobox to list each supported language in their native language to make it easier to locate and translate KlipTok to your preferred written language.

<div class="center">

![Native translations of the languages in the language selector](img/translation_box.PNG)

<span aria-label="caption" class="caption">Native translations of the languages in the language selector</span>

</div>

### How does KlipTok translate to your language?

KlipTok uses a library called [Toolbelt.Blazor.I18nText](https://github.com/jsakamoto/Toolbelt.Blazor.I18nText) to translate its text.  There is an object embedded in every Blazor component delivered on KlipTok called `Localize` that loads up the correct translation file for your language.  

```csharp
	if (Localize == null)
	{
		Localize = await I18nText.GetTextTableAsync<I18nText.KlipTok>(this);
		StateHasChanged();
	}
```

All text that is rendered for the web page passes through that collection.  The Search bar at the top of the screen has it's watermark text placed with:

```csharp
@Localize?["SearchKlipTok"]
```

The search results text referencing the `SearchTerm` submitted is formatted like this:

```csharp
<p>@string.Format(Localize?["SearchedKlipTokAndFoundTemplate"], SearchTerm)</p>
```

The translations are all stored in JSON format and open for community contributions [on GitHub](https://github.com/csharpfritz/KlipTok.Translations)

## Up Next

KlipTok has grown and become a bit more complex over the last few months.  We're service more than 3.8 million clips and need to provide some assurance that updates to the site don't break things.  I've started working on building integration tests using [Playwright](https://playwright.dev) and will be making those test scripts publicly available [on GitHub](https://github.com/csharpfritz/KlipTok.IntegrationTests) as well.

## Summary

Introducing these new features and translations has added a lot of value to KlipTok and made it easier for your to understand the content you'll find in the clips we present.  I hope you find these new features useful as I move forward with additional improvements.



