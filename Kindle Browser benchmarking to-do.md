---
stage: draft
---
To build Readup.ink, and to compile the [[Documentation for the Kindle Browser]], I've created several test HTML pages to test the features of the web browser.

Here is a checklist of features that I still want to test.
## To check

**CSS**

- [ ] Media queries: do they work?
- [ ] Can `background-color: transparent;` be used?
- [ ] Does `text-decoration`, see [[Documentation for the Kindle Browser#^kindle-links|this]].
- [ ] Does `pointer-events: none` work? Apparently a Safari 4 feature.

**JS**

- [ ] Can kindle know its own user agent from JS?
- [ ] Does the overlay hack affect background text? It seems to persist more, but that might be an illusion.
- [ ] Can Deno Fresh islands run on the Kindle? Or are the using unsupported Web Components?
- [ ] Can Preact in general run on the Kindle? (maybe with transpilation)
