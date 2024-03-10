Currently, an article from a writer is only registered in Readup if a reader reads it.

Readup could support RSS or Atom feeds of writers, which provide information about their latest post, and then auto-import those articles to the writer's profile.

RSS feed links can often be detected and/or guessed from a website: Readup could scan for the RSS feed the first time a reader has imported an article from that site, and then periodically import the feed again. As a reference implementation, we could look at an RSS reader like [FreshRSS](https://www.freshrss.org/) .

RSS feeds may be messy. We can rely on Readup's built-in metadata and content parsers to clear up the mess, but the automated nature of an RSS feed leads to more risks (a human wouldn't post a garbled result, but with automation, we'd have to make all automatically scraped articles visible).

Sometimes, "Comments" RSS feeds are also exposed. For both these reasons, adding and managing RSS feed URLs would be a good task for a [[Readup Evolution/The Readup Librarian Role|Readup Librarian]].