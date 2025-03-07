---
stage: published
created: 2025-03-05
---

Unlike 1Password, Bitwarden/Vaultwarden does not have a dedicated "archival" feature. It's the 10th most popular [feature request](https://community.bitwarden.com/t/archive-old-accounts/7191/156) in the Bitwarden forums at the time of writing.

Before I switched to Bitwarden, archiving was a feature I used regularly in 1Password. The cases are rare, but sometimes you need to check which now-expired card was used for some service, or you need to dig up the former PIN code, phone number or address you used to use.

At the same time, it is annoying to have those outdated items pollute day-to-day searches & browsing when you are almost never going to need them. They should still be around, but also hidden. They should be _archived_.

## My workaround

Since there isn't a native feature to support this, all solutions are workarounds. 

For a self-hosted Vaultwarden instance with a handful of users, the following approaches should work fine. They probably don't extend well to larger organizations. You need to have full admin ownership over your instance to make this work.

I'm suggesting two different approaches, depending on how you currently use Bitwarden. 
### Case 1: if you are already part of an organization where collections can be made.

Bitwarden/Vaultwarden collections in organizations are separate concepts from "folders", see [this explanation](https://bitwarden.com/help/about-collections/).

The idea is to:
1. **Create a new "Archive" collection** in your organization. 
2. **Create a new archival account** and invite it as an "Owner" of your organization. Only use the web interface for this account, and save its master password in your main account vault.  I used the `+`-addressed email address like `<main>+archive@mail.com` to still get related emails in my main mail inbox. Use something like a private window or Firefox Tab Container to manage both accounts at the same time.
3. **Revoke access to the "Archive" collection from your main account, using the admin console.** This will ensure that archived items don't appear in search results or listings from your main account. 
	1. ⚠ Caveat: you will need to make your main account a regular Vault "User", but grant specific access to all the other collections you have in your organization, except the "Archive" collection. This is a small annoyance, especially if you have many collections. You will need to update access rights for every new collection you add.
	2. ⚠ For all organization admin tasks going forward, you will now need to use the archival account.
4. Using the archival account logged in on the web interface, move items to be archived into the "Archive" collection, and remove them from any other collections they were in.
	- If you were using the My Vault folders only, you will need another approach where you temporarily grant access to the "Archive" collection to your main account in your new archival organization, so you can move your personal items into that collection. Afterwards, revoke access again.
5. Repeat step 4 whenever new items need to be archived.
### Case 2: if you only use your personal "My Vault" with "folders"

In case you want to keep a workflow where you only use My Vault in day-to-day use, I'd suggest the following approach:

1. Create a new "Archive" organization
2. Create a new archival account and invite it as an "Owner" of the new organization. Only use the web interface for this account, and save its master password in your main account vault.  I used the `+`-addressed email address like `<main>+archive@mail.com` to still get related emails in my main mail inbox. Use something like a private window or Firefox Tab Container to manage both accounts at the same time.
3. Temporarily grant access to the new organization to you main account
4. Using your main account on the web interface, move items to be archived to your archival organization's default collection.
5. Revoke access again from the organization from your main account.
6. Repeat steps 3-5 whenever you need to archive items.

---

Whenever you need to dig up something from the archive, log in using archive account in the instance's web interface, and find the archived items there.

Depending on how much you care about sharing passwords with other instance users, you may need a separate personal organization for each user, although you may also work with granular access to per-person archival collections.