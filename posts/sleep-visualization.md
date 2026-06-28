---
title: "Sleep Visualization"
slug: "sleep-visualization"
published: "2019-05-26T20:22+02:00"
year: 2019
cover: "./assets/sleep-visualization/sleepviz.png"
---

![sleepviz.png](./assets/sleep-visualization/sleepviz.png)

Last week I spent some hours on visualizing my sleep pattern. It is based on 99 days of manually tracked data (yes!). This is a first part of a much larger data tracking project in progress. [See a rendered full screen version here](https://web.thorgalle.me/sleepviz1/).

Stylistically I used a minimalistic color scheme that could've come straight from Hundred Rabbits[^1]. Obviously, this visualization still misses a legend or visual cues to give meaning to the elements. But maybe the air of mystery around my actual sleep/wake times is not too bad ;) 

### A little legend:

- Each vertical white line represents a night, time goes from left to right.
- The top red dots are wake-up times. The higher the earlier.
- The bottom white dots are sleep times. The lower the later.
- The dotted lines are stay-awake-in-bed times - somehow I found this data interesting - maybe because I want to reduce that time 🙃

Technically I had fun implementing a parser that could understand my ambiguous notes on sleep & wake times. I used an Airtable[^2] spreadsheet with a row for every day and columns for sleep and wake times. The format is not consistent though. I could write "2:15" to mean a bed time at 2:15 AM the next day, or "11" to mean 11 PM that same day. [Moment.js](https://momentjs.com/) was helpful. For the visualization itself I  used the JS vector interface library [paper.js](http://paperjs.org/) just to try it out. It's great for what it does - more advanced than the [p5.js](https://p5js.org) library I had used before. But I might still replace it it with D3.js as the latter offers more tools for working with data dynamically, as I used in a similar project [for visualizing the movies I've seen](http://thorgalle.me/articles/movieviz).

You might already be able to spot that starting a 9 to 5 job had an effect on my sleep stability the last weeks. To be continued!

[^1]: [Hundred Rabbits](https://100r.co/) is an 2-person artistic collective making tools, games, recipes, and videos on board of a sailboat. Hundred Rabbits is cool, check them out.
[^2]: [Airtable.com](http://airtable.com/) is spreadsheets on steroids. Google Sheets but better, with a nice auto-generated API. My [Net Promoter Score](https://en.wikipedia.org/wiki/Net_Promoter) is skyrocketing here.
