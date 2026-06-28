---
title: "Installing your Firefox add-on permanently"
slug: "installing-your-firefox-add-on-permanently"
published: "2018-05-01T00:00+02:00"
year: 2018
---

When you're developing a cross-browser add-on, you probably want to try it out for a while in your daily browser. Unfortunately, contrary to Chrome, if you **temporarily load your extension in Firefox, it will be gone after a restart.**

That's because Firefox **needs to sign your add-on before you can install it anywhere**.

[The documentation](https://developer.mozilla.org/en-US/Add-ons/Distribution) explains so, but in a convoluted way. It's actually pretty simple:

1. Register at FF's [Developer Hub](https://addons.mozilla.org/en-US/developers/) (top right) if you don't have a FF account yet.
2. Go to the [add-on submission](https://addons.mozilla.org/en-US/developers/addon/submit/distribution) page.
3. Choose **'on your own'**. This wil **immediately sign** your add-on, but it won't be listed in the add-on site for distribution. Ideal for a test version.
4. Upload your zipped add-on files.
5. Sign, download the `.xpi` file & enjoy. It can be installed from `about:addons → gear icon → Install Add-on from file`
