---
created: 2025-07-14
stage: published
---

And that's good!

On Replit, the free plan limits you to "public" apps.

It seems rather obvious, but a public Replit app does not publicly expose the **values** of its environment variables (secrets) when it is "remixed" or forked by others. Only the variable **keys** (names) are copied, with empty values.

I couldn't find this behavior explicitly documented, and given the security risks of exposing back-end secrets, I verified it by creating a test app that remixed another test app from another test account.

An older Replit blog post (https://blog.replit.com/secrets) hinted at this behavior:
> This change will also unlock some features for us such as **retaining keys** when someone forks a repl with secrets. 

(implying that neither keys nor values were retained before. Now only keys are retained.)

I wrote this note because I could find at least one person who also wasn't sure about the security of env vars of public replit apps: https://forum.freecodecamp.org/t/how-to-maintain-security-with-env-on-repl-it/436193/2
