---
title: "A Mealime - Siri integration"
slug: "mealime-siri-integration"
published: "2023-03-04T20:00+02:00"
year: 2023
description: "This integration allows you to add items to your Mealime grocery list using a Siri voice command."
type: "Article"
---

For several years I’ve used the meal planner & grocery list app [Mealime](https://www.mealime.com/). It’s great, since it neatly combines both concepts in a slick app.

However, there was one thing that I have always sorely missed in the app: being able to say “Hey Siri, add apples [or whatever] to my grocery list”, without pulling my phone out.

This feature stayed out, and I kept having occasions where I wanted it to be there.

So, built it myself:

<video controls src="./assets/mealime-siri-integration/img_0627.mp4"></video>

Do you have a Mealime account, a little bit of technical know-how, and would you also like to be able to do this?

You can set this up yourself relatively easily, without coding. I included instructions [on the open-source GitHub repository](https://github.com/th0rgall/mealime-groceries).

## How it works

The solution consists of, (1) a Shortcut, and (2), a small server application.

### 1. The Shortcut

In the video, “Add groceries” is a simple iOS/ipadOS/macOS Shortcut that listens to what you want to add. It then sends this text verbatim to the `/add` endpoint on the server application. The shortcut fits onto one screenshot:

![Mealime iOS shortcut](./assets/mealime-siri-integration/image.png)

When I say “Hey Siri, Add groceries”, the Shortcuts gets invoked.

### 2. The server application

The server application was a Node.js application that I recently rewrote in [Deno](https://deno.land/). When starting it, you supply the server application with your Mealime username & password.

The server does two things:

1. It receives the request from the Shortcut. It does some basic processing on it to recognize multiple items in your query (”apples AND pears”), as well as the product category an item belongs to (”Produce”). For this, it uses a static 
  [YAML database file](https://github.com/th0rgall/mealime-groceries/blob/main/src/products.yaml).
2. After the request has been processed, it uses the Mealime username and password to simulate the user logging in on the (old) Mealime web app. From there, it simulates that same user adding the requested items, and voilà!

The server application can run anywhere where Deno runs, but in particular, it works for free and out-of-the-box on [Deno Deploy](https://deno.com/deploy). There are also [Docker images](https://github.com/th0rgall/mealime-groceries/pkgs/container/mealime-groceries) available.

## Caveats

It works well enough (and I use it often!), but not everything is dandy in this integration.

### **Shortcut invocation limitations**

What unfortunately doesn’t work when invoking the shortcut, are the following more natural and direct expressions:

- “Hey Siri, add apples and pears to my grocery list” → this will add it to Reminders
- “Hey Siri, add apples and pears to my Mealime grocery list” → this will make Siri attempt to contact the actual Mealime app, which doesn’t have support for it (“Sorry, you’ll need to continue that in the app”)

### Incomplete product database

The integration can determine the section a product belongs to (”Produce”, “Baking & Spices”, …), but this process currently has limitations, as it is [based on a static mapping file](https://github.com/th0rgall/mealime-groceries#adding-more-product-mappings).

It might be possible to reverse-engineer the search function of the Mealime app, and in that way support all the product-to-section mappings that Mealime understands. However, I didn’t feel this was worth the effort. I only looked at the endpoints used in the web app, where you have to manually specify to which category you want to add your product:

![Mealime grocery list modal](./assets/mealime-siri-integration/6Q8MWp-image.png)

### Possibility that it breaks

The web app that I simulate will likely be replaced with a new version that is coming up, at some point in the future:

![Mealime new version](./assets/mealime-siri-integration/7o1tHv-image.png)

Unfortunately, this new web app does not have the feature yet of manually adding items to your grocery list. I hope it still comes, or that Mealime makes an official Siri integration!

