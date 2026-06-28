---
title: "My default apps in 2024"
slug: "default-apps-2024"
published: "2024-02-24T12:00+02:00"
year: 2024
description: "Following a list that's been traveling around the internet, here are the apps I'm using in 2024"
type: "Article"
---

Inspired by [Dan Hannigan’s entry of last November](https://danhannigan.dev/post/default-apps-2023/) (which was in turn inspired by several others), I’m making a list of the main apps that I use today, and reflecting a little on them too.

Before we dive in:

1. App lists are ordered from most to least frequently used.
2. I’m adding the categories “Photo Storage & Viewing” & “Video editing”, because those apps deserve a shoutout 🙂

---

# My default apps in 2024

📨 **Mail Client:** Mail (iOS), Gmail web client (macOS).

📮 **Mail Server:** Gmail

📝 **Notes:** TextEdit (macOS), Notion (personal & work), Apple Notes, Obsidian.

✅ **To-Do:** Apple Reminders, Notion.

📷 **Photo Shooting:** iPhone 11 Pro

🎨 **Photo Editing:** iOS Photos, [GIMP](https://www.gimp.org/), some tools in SetApp, [Darktable](https://www.darktable.org/)

🖼️ **Photo Storage & Viewing** Apple Photos, [Nextcloud Memories](https://memories.gallery/), [PhotoPrism](https://www.photoprism.app/), Google Photos (for shares)

**🎥 Video Editing:** Kdenlive, iMovie

📆 **Calendar:** Apple Calendar (Mac), Google Calendar (iOS)

📁 **Cloud File Storage:** Nextcloud (self-hosted), Google Drive, iCloud.

📖 **RSS:** [FreshRSS](https://www.freshrss.org/), just switched from [Readwise Reader](https://readwise.io/read)

🙍🏻‍♂️ **Contacts:** macOS Contacts synced with Google

🌐 **Browser:** Google Chrome

💬 **Chat:** Signal (preferred), Slack (work), WhatsApp (by necessity), Apple Messages, Facebook Messenger, Discord.

🔖 **Bookmarks:** Notion

📑 **Read It Later:** [Readup](https://readup.org/), [NetNewsWire](https://netnewswire.com/)

📜 **Word Processing:** Google Docs, Apple Pages

📈 **Spreadsheets:** Google Sheets, Apple Numbers

📊 **Presentations:** Google Slides (it’s been a while)

🛒 **Shopping Lists:** Reminders

🍴 **Meal Planning:** [Mealime](https://www.mealime.com/); including [my Siri integration](https://thorgalle.me/articles/mealime-siri-integration/).

💰 **Budgeting and Personal Finance:** [Firefly III](https://www.firefly-iii.org/), Google Sheets

📰 **News:** Readup, FreshRSS.

🎵 **Music:** Spotify

🎤 **Podcasts:** Spotify (every weekday: [Het Kwartier](https://www.vrt.be/vrtmax/podcasts/vrt-nws/h/het-kwartier/))

🔐 **Password Management:** 1Password, moving to Bitwarden & Vaultwarden

🧑‍💻 **Code Editor:** [VS Code with neovim](https://github.com/vscode-neovim/vscode-neovim), vim

✈️ **VPN:** Self-hosted WireGuard PiVPN, ClearVPN (from SetApp), Google One VPN

# Some commentary

## FOSS + Google

Fifteen years ago, and for about a decade, I was mostly using Linux on computers. This is where my familiarity with GIMP, Darktable, and Kdenlive comes from. I also only owned Android smartphones throughout that time, and had no budget for software subscriptions. Google’s services worked well cross-platform, and were “free”, so I came to use many of them. I still do today!

## **Enter Apple**

After I started working professionally with design tools in 2017, and appreciating a stable desktop more, Apple increased its footprint on my tech use step-by-step, both in hardware & software.

Here’s a timeline of when I got my Apple hardware:

2010: iPod (Nano, 5th gen)

2017: MacBook (MBP, 13”)

2020: iPhone (iPhone 11 Pro)

2022: iPad (6th gen); new M1 MBP

2023: Apple Watch (Series 9)

Because of the ecosystem effects, I started using Apple services more. But I kept Google services and a suite of other independent software around, because of cross-platform compatibility, and a feeling of unease with a full Apple lock-in.

How do I feel today about Apple today, in short? It’s a love-hate relationship.

On the positive side, it’s absolutely amazing that a [Late 2009 (!!) 27” iMac](https://thorgalle.me/articles/upgrading-an-aging-imac/) is still in daily use by my parents for web browsing, web TV, office & email. It still has software and hardware that looks great and works well today. Its speaks to a culture of top-notch design across the stack to make this possible. I also think the work they’ve done recently with things like Focus Modes is a necessary evolution in tech.

Conversely, I absolutely hate Apple’s often-repeated anti-consumerist behavior: their recent plan to petulantly [kill PWA’s in the EU](https://open-web-advocacy.org/blog/its-official-apple-kills-web-apps-in-the-eu/). The impossibility of using Sidecar to extend your display to an iPad connected to a different iCloud account (even if you have access to, and can “trust” said iPad). And so many more…

## Towards self-hosting & independent vendors

More recently, I’ve become more averse to *any* Big Tech provider, especially Google, and especially for personal use. It’s a long-term goal to eventually replace whatever Big Tech services possible with self-hosted or reliable, independent alternatives. My main motivation is to keep control and ownership of my data and freedom in my computing, but cost savings and privacy concerns also play a role.

I’m not rushing this transition though. Practicality remains important.

In terms of **photo management,** I’ve recently pushed forward a years-long effort to archive and organize family and personal photos while minimizing Big Tech’s involvement.

It started with fragmented backups on external hard drives and computers, a personal Digikam collection, a 15-year-old Synology NAS, and a handful of Google Photos shares. I’ve now brought this together in two systems:

- a shared family library using 
  [PhotoPrism](https://www.photoprism.app/)
- a personal photo collection on Nextcloud with 
  [Nextcloud Memories](https://memories.gallery/)
   (also as a service to other family members 🙂)

These apps (and modern single-board computers) are truly amazing. Both services are hosted on a single Raspberry Pi 4 8GB with 4TB of USB-attached SSD storage, and they are a joy to use. They do not match the Big Tech cloud services on all metrics of usability, but they bring the essence of the feature set: easy access on any device, high capacity, and fast-ish results. And all this without having features you like at the mercy of business decisions, without succumbing to surveillance tech, and without having your photo experiences held hostage by perpetually increasing monthly subscriptions. It takes maintenance work, but I like it.

## What has changed recently

In the department of **news & reading,** I’ve recently cancelled my [Readwise Reader](https://readwise.io/read) subscription, which I had kept for several months. I like the app and “all-in-one reader” concept, and mostly used it for RSS intake & highlighting. But I just didn’t use it enough. I still kept reading on [Readup](https://readup.org/), and highlighting in Notion, despite having another paid reading app.

I’m refocussing on open solutions. Two weeks ago, I experimented with NewsBlur and FreshRSS, and decided to set up [FreshRSS](https://www.freshrss.org/) on my Raspberry Pi, with the [NetNewsWire](https://netnewswire.com/) client on all iDevices. So far, I really like it! I’ve never been this actively using RSS. And it fits together nicely with Readup.
