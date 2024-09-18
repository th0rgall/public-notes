### Context

Readup's first version could inject Readup comments into article pages on other websites (see [[The original reallyread.it web extension]]). Eventually, Readup's web extension reader mode became the only way to view Readup comments on an article, with one exception: an independent comments embed script [(source code)](https://github.com/reallyreadit/web/tree/master/src/embed) was still used on the readup.com blog (now [blog.readup.org](https://blog.readup.org)), _without the need to install the web extension._

This embed got removed from the blog in efforts to simplify the code when making Readup free & open-source.

### Why should we bring the embed back?

**Commenting platforms and libraries** [like Disqus](https://alternativeto.net/software/disqus/) (which itself [questionable](https://supunkavinda.blog/disqus) ) form a market of their own.

What Readup can offer to this market is an opinionated approach: all comments come from people who read the article.

For example, [these comments](https://readup.org/comments/thorgalleme/little-adventures-on-a-cycling-trip--thor-galle) on one of my older blog post could be shown right on my website. I'd like that!

### Read-only as a first step

Having read-only embeddable comment sections would simplify the initial technical setup. The purpose is to show past comment threads on articles, and to invite people to check out Readup as a platform. Commenting to Readup wouldn't be possible on the article page itself.

Allowing people to comment straight from the external websites is interesting too, but involves more complexity, which is discussed in [[Comment embed including tracker]].

### Steps to take

- Restore and simplify the embed code, removing components related to [[Comment embed including tracker|the tracker]] & "provisional tracking".
- Currently, the Readup API disallows access from non-Readup origins.
  - Allow writers to configure web origins in their Readup account where they on which they want to embed Readup comments. This could be done automatically given [[Automated verification for writers]].
  - The API should allow access for comments on articles from these writers, on these origins.
- Document how to add embedded comments to your site, including the origin/writer verification process.
  - Distributing an embed script through `npm` would probably help.
