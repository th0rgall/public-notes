---
stage: published
---
While trying to set up this notes section on my Gatsby blog, sourced from an Obsidian Markdown vault, I noticed that references to images in Obsidian with one or more spaces in their path weren't picked up by the `gatsby-remark-images` plugin.

Using the `![](<some path>)` syntax, it seems to work everywhere though.

Markup | Example | Works in Obsidian? | Works in `gatsby-remark-images`?
--- | --- | --- | ---
URL-encoded (Obsidian default) | `![](some%20link%20with%20spaces.png)` | ✅ | ❌
Plain | `![](some link with spaces.png)` | ❌ | ❌
CommonMark | `![](<some link with spaces.png>)`  | ✅ | ✅


The CommonMark syntax wins! Thank you, [legends2k](https://superuser.com/users/50345/legends2k) on [SuperUser](https://superuser.com/a/1517072). It's unfortunate that this isn't the Obsidian default.