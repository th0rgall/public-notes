---
excitement: medium
---

In a non-existent perfect world, the Readup article and metadata parser would be impeccable: they would always correctly detect the authors, article publisher, and article content.

In reality, such perfection is impossible. The internet is inherently messy, and structured information can't be extracted without making some mistakes.

To combat this, humans could be involved with ensuring the quality of data in Readup. 

### Existing admin tools

Currently, a limited set of tools is available to Readup "admins" via an Admin dashboard.

The dashboard provides these features to admins:
- Unlinking an author from an article
- Linking an an author to an article

I think one or more additional roles that permit users to fix article data would be a helpful addition to this.

### Types of edits

Ideally, librarians/editors could **assist the parser** and fill in the gaps where the parser can not do its job automatically, by providing it with rules.

Only in very limited cases would it be necessary to allow manual overrides/fixing of metadata of individual articles.

Examples where humans could by **providing rules**, beyond the currently existing ones, are:
- Readup's **article content and metadata parsers** frequently incorrectly determine properties of article. Some "hardcoded" overriding identifiers fix wrong automatic behavior, but these are very limited and currently stored in code. They could be stored in the database, and/or maintained as part of a [[Collaborative article selector list|separate open-source project]].
	- [[Gated article access|Marking entire websites as subscriber-only]], when the website only publishes paid content, or adding rules with which the Readup parser could detect that an article is subscriber-only.
- [[Writer-Publisher article intake from RSS feeds|Adding RSS feeds]] for publications where these can not be auto-detected.
- Blocklisting entire publications that do not contain articles (this is currently a hardcoded list, too).
- Creating "author rewrite" rules. The idea is that certain sites are published by a single writer, but this fact isn't mentioned on article pages (which complicates detecting the article author with [[Collaborative article selector list|selectors]]). In such case, a rule can be added that maps a website domain name to a writer in Readup. Such rules already exist, but they are hardcoded now. 

Examples where humans could apply per-article fixes, are;
- [[Deletion capabilities]]: cleaning up imported pages that are not articles
- Fixing author names
- Updating an article's canonical source URL, in case it has changed.
- Merging article entries and threads, and selecting one canonical version, especially when an old version became inaccessible, and a new version remains accessible, while both already have independent activity in Readup.
	- ... but perhaps also in case the exact same article appears multiple times as accessible in the database, selecting one canonical publication's version would be a win for centralized discussion on Readup.
- Specifying [[Using article archives|an archived version]] of articles that have become inaccessible on the internet.
### Gamification & rewards

Incentives

Edits could be tied to rewards, much like Google Maps does.