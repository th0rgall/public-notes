---
title: "Learn vocabulary from the environment"
slug: "learn-vocabulary-from-the-environment"
published: "2018-05-02T00:00+02:00"
year: 2018
description: "Learning advanced English vocabulary with vocabulary.com, supported by a plugin"
tags: ["voc-enhancer", "project", "language learning"]
---

**TLDR: I made a browser plugin for the English language study tool** [**vocabulary.com**](https://vocabulary.com)**. It supports quick word adding from any site and translation when studying.**

Huh, why are you learning vocabulary? Yes! Be it in a paper or a book, I regularly encounter words or expressions I do not yet know, mostly in academic texts.

One option is not to worry and to carry on. *I can derive the meaning from the context*. However, you can get a seriously wrong idea of a word with that strategy. For example, for long I thought the word 'gloomy' had something to do with 'glow', just because it sounded similar.

Once I finally looked it up, that didn't seem too right:

![glowgloom.png](./assets/learn-vocabulary-from-the-environment/glowgloom.png)

I wanted to tackle this problem by keeping track of the words I did not know, and to study them systematically.

## 101 quick fix: the spreadsheet

![spreadsheet.png](./assets/learn-vocabulary-from-the-environment/spreadsheet.png)

I made a list. A long list. With the first entry planted in December 2015, it contained **540 words (!)** that I did not completely understand yet.

I added on metadata about part-of-speech types, possible synonyms, possible Dutch translations, originating sentence, I-thought-it-meant entries, multiple languages,...

I kept collecting words while reading papers, books, articles, … but it was annoyingto keep track. Waiting until the Google spreadsheet opened to put in a word interrupted the flow of what I was doing. And even worse, it was fruitless. I never took a look at the list to study it.

## Vocabulary.com

Then I found [vocabulary.com](https://vocabulary.com), it is a web app that

> . . . combines the world's smartest dictionary with an adaptive learning game that will have you mastering new words in no time.

In addition, it features funny and not-so-formal definitions of words. It's really cool. Spending about 2-3 minutes on it almost daily for half a year, **I've now 'mastered' about 150 words, and added around 400.**

A few things could still be improved. That's why I made a browser plugin with **three main features:**

## 1. Adds words fast, everywhere

You can select any word on any web page and add it to one of you vocabulary.com lists. The plugin will pick up the surrounding sentence and source and include it in the Vocabulary.com list:

![adding.png](./assets/learn-vocabulary-from-the-environment/adding.png)

## 2. Shows translations in the learning game

Associating a far-fetched English word with their Dutch equivalent helps me to get a grip on the nuances. That's why it is helpful to show translations next to word definitions, which the addon does. You can select any language by clicking the language abbreviation.

![game.png](./assets/learn-vocabulary-from-the-environment/game.png)

## 3. Links to useful services

For every word, easy clickable shorcuts are shown next to the translations for [Google Images](https://images.google.com/) and [DuckDuckgo Images](https://duckduckgo.com/) (seeing what a word is can be really useful), [YouGlish](https://youglish.com) (for real-life pronounciations) and [GIPHY](http://giphy.com/) (for fun).

## Download & install

The add-on is now enjoyed by some ~400 people! (update: March 2020)

Find it here for your browser:

- [Google Chrome](https://chrome.google.com/webstore/detail/vocabularycom-enhancer/diddacjdgklfjgnlmknacnakjcgdiegn)
- [Mozilla Firefox](https://addons.mozilla.org/en-US/firefox/addon/vocabulary-com-enhancer/)

The code is available from my Github page<https://github.com/th0rgall/voc-adder>. It will be published in the add-on stores soon.

*Update Nov '18: A cross-desktop platform destkop word adder is in the works. It has some neat features, like a selection of word meaning and integration with OS-wide text on macOS (including .pdf's in Preview!)*
