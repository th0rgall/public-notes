---
stage: draft
---

Recently, I was happily surprised when noticing

## When your user uses iOS

on an iPhone or iPad

iOS has

### Web Push can _only_ be enabled from a "Home Screen app" (PWA)

If you're visiting a site in regular Safari, the needed APIs will simply not be available.

This is a break from all other browsers on Android, although we've had a few (inconsistent) reports of users explaining that they could enable notifications only after installing a PWA.

### Notification badges don't work

## When your user receives the notification (appearance)

- it appears in many different ways, often with the browser URL and icon

## When your user taps a notification

Notification click targets: most Chromium-based browsers on Android do this well. All other browsers we tested have dysfunctional implementations.

- iOS/iPadOS (all browsers): the home page is opened when you click a notification.
- Firefox on Android: there is a bug that this either opens the home page of your host, or, it just opens the last page in the history.

## When your user accidentally rejected your prompt (per-site block)

You can detect this.

They need to reset their notifs.

## When your user disabled notifications for their browser

Notification permission scope: "Installed PWA" vs regular site

What is a PWA?

https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Installing

### Chrome on Android

If you "install" the PWA on your home screen via Chrome, it will be considered _a separate app_ from the Chrome Browser app. - However, Chrome seems to "transfer" any previously registered Web Push subscriptions from the site into the installed PWA. You don't need to allow/enable them again in the PWA, and they will lead to the PWA henceforth.

### Elsewhere... it doesn't work the same elsewhere

**Firefox on Android**: just makes a bookmark.

Ecosia: it's not possible to add a PWA.

---

## General development gotchas

## Firebase Cloud Messaging Web SDK gotchas

- it overrides events

- Notifications will only be sent if the app isn't open in the front-end. I think this can be considered a cool feature.

- Generally, **service workers** only allowed on HTTP on **localhost**. On other hosts, **HTTPS is required**.
- Firebase emulator limitations:

  1.  Firebase emulators do not support being served over HTTPS (all)

  2.  Firebase JS SDK emulator connectors do not support _connecting_ to HTTPS-based emulators (all except Auth)

- The first issue can be worked around with a HTTPS-enabled reverse-proxy. The second can't be worked around, without changing the SDK code (which I only briefly attempted).
- **Implications**: - it's easy to test web push on a **dev machine http localhost with emulators**, with a spoofed mobile phone User Agent (because we only allow notifications on mobile phones, by User Agent) - it's easy to test web push on mobile, using the real staging/production servers - it's _not_ easy to test with emulators on a real mobile device. The only way I got it to work: 1) to run a reverse nginx proxy in iSH on an iPhone, that connects to HTTP emulators on a desktop. This way, you can connect to localhost on the iPhone.
