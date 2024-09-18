The blog.readup.com embed as discussed in [[Read-only Comment Embed]] was a limited but interesting feature. It was only available on blog.readup.com, but it also embedded a Readup read tracker onto the blog, even for visitors who hand't installed the Readup extension yet:
	- The tracker was supported by API & database backend code that worked with "provisional readers", allowing to store the reading of anonymous/not-logged-in visitors to the blog.
	- Once anonymous were finished reading, the comment section would load (IIRC). Visitors could then create an account through a shortened onboarding flow, and immediately leave a comment, because their activity as a provisional reader could be merged with their actual eventual account.
	- If you were logged into Readup, the blog.readup.com read tracker would save your reading progress to your account automatically.

Building on top of the hypothetical [[Read-only Comment Embed]], we could reinstate the provisional reader system and the included read tracker for other sites than blog.readup.com, too. This would lower the barrier to leave a comment via Readup.

### Anonymous comments

One aspect that wasn't developed was allowing "provisional readers" to comment after reading the article. This would be the ultimate way to remove barriers for Readup comments on external sites, and it is a feature supported by many commenting systems. But it has downsides:

- Complexity throughout all Readup systems is added when comments can be anonymous
- Readup already works only with usernames, and doesn't require real names. Purely for public privacy reasons, anonymous comments don't make much sense.
- Anonymous comments typically don't require an email, and may be more likely to invite automated spam. Then again, Readup currently doesn't enforce email verification either, and any spam bot would have to trick Readup's read tracking system first.

### The effect of provisional reads on the AOTD

Since this system was only ever active on the Readup blog, and Readup blog posts were [heavily demoted](https://github.com/reallyreadit/db/blob/6f1a07bee3b941e47101e6151c3ec1ab2e807da7/migrations/13.1.2/migrate.sql#L35-L43) from the AOTD ranking (perhaps due to provisional reads!), we haven't seen the wider effects of external read tracking

However, Readup's blog post are among the most widely-read articles on Readup, because they are the only articles that include anonymously tracked provisional reads from outside the Readup. As an illustration, see my own written articles:

![[CleanShot 2024-03-23 at 21.09.16@2x.png]]

If an external site with articles, which are fully read by five people per day, would include the Read tracker with anonymous provisional reader functionality, it can almost be guaranteed that these articles would win the AOTD today!

Probably a case could be made for the demotion of all external anonymous reads, in favor of logged-in reads.
### Privacy & local-first

One additional concern with the provisional reader system is that it tracks visitors who haven't agreed to such tracking, and sends this tracking information to Readup's servers. It's reasonable to think that might be at odds with regulation like the GDPR.

A privacy-friendly alternative to read tracking data immediately to the Readup server, slightly related to [[Offline mode]], would be to perform tracking only locally within the browser, using some kind of local storage. Then, when the user wants to log in and comment, or to create a new account, this read-tracking state could be included with the login request and merged with the (new) account.

Furthermore, with most browsers [having blocked third-party cookies by default](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct), it may be difficult to keep someone logged in into Readup seamlessly across different websites.

The Readup login on external websites could work with:
- the web extension (if installed), which can communicate with its service worker, and has credential access to readup.org.
- the [Federated Credential Management API](https://developers.google.com/privacy-sandbox/3pcd/fedcm) (suggesting a login with Readup when opening a Readup-enabled website)
- the [Storage Access API](https://developers.google.com/privacy-sandbox/3pcd/storage-access-api) (leading to a pop-up permission request for the comments iframe to use shared state, which could perhaps be passed up to the embedding website)
- [CHIPS](https://developers.google.com/privacy-sandbox/3pcd/chips) (requiring a one-time new login on every Readup-enabled website)