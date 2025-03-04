---
stage: published
title: Challenges using SendGrid as a multilingual start-up & how to work around them
source:
  - https://www.notion.so/thorgalle/Little-SendGrid-rant-cafd5b563b1e4df8bdb93dddb16773e9?pvs=4
created: 2025-02-25
updated:
---
At [Welcome To My Garden](/projects/welcome-to-my-garden) and other projects of Slowby, we've used SendGrid's Transactional Email API since the start in 2020. During 2023 and later, I integrated our systems more deeply with SendGrid's Marketing offering: _Contact Lists_, _Single Sends_ and _Automations_.

Along the way, I've stumbled over more than a few bumps, which I want to cover here, in the hope that our solutions and workarounds may be of use to someone in a similar situation, or to someone doing a product comparison of email service providers.

To give you an idea of whether you are in a similar situation, here are **some details on the scope of our SendGrid usage in 2024:**

- We have about 50K contacts synchronized with SendGrid. To target specific sub-audiences, we make extensive use of Contact Lists and their Segments, as well as custom fields on contacts (for example: a _language_ custom field). We synchronize these contacts using the SendGrid Marketing API.
- We sent a few hundreds of thousands of "marketing" emails in 2024. Those mostly consist of one-off newsletters (_Single Sends)_, but also some tens of thousands of _Automation_ flow emails, especially in the last year.
- We use SendGrid's _Dynamic Templates_ built with the _Design Editor_ for almost all transactional email. We use some template logic based on contact (custom) fields that makes these emails actually dynamic.  We sent tens of thousands of transactional emails using the API in 2024.
- (!) We require a multilingual setup. For example, every Segment we created has three versions, one for French, Dutch and English. Similarly, every Dynamic Template has three versions.

Among others, caring about the user experience in multiple languages causes a waterfall of challenges with SendGrid, as you'll see below.
## 1. 👍 The good sides

First, I'll review some things I appreciate about SendGrid. 

- **The core:** SendGrid does the most essential thing well: **sending email**. As far as we can verify, when we use the transactional email API, or click "Send" on a newsletter, SendGrid does its job.
- **Pricing:** from a shallow comparison, SendGrid is still competitively priced compared to competitors.
	- In terms of marketing, MailChimp seems more expensive, but Brevo seems cheaper.
	- In terms of transactional email, SendGrid seems cheaper than MailGun, but more expensive than Amazon SES or Brevo.
- The **[REST API](https://www.twilio.com/docs/sendgrid/api-reference)** of both Marketing and Transactional side is fairly **well-documented**, and accessible through an acceptable (but only partially typed) Node.js library, which we use.
- The **Marketing WYSIWYG drag & drop "Design editor" works**. It's a pain to fine-tune colors and font-sizes sometimes when dragging in new blocks, but it works for non-programming colleagues too.
- Over the last three years, dashboard login has improved (no more mandatory _Twilio Authy_ for TOTP), as well as handling something called _Subuser Management_.
- The **inbound parse** API also works pretty well.
- Their support is pretty responsive and helpful!

Now, over to meat of this note, **the caveats:** some features in the SendGrid offering have frustrating holes and limitations, or counteract a good developer and user experience. I'm sure the technical difficulty of making a scalable email marketing & sending system is enormous, but when I compare this to other big cloud services we're using, I am still left with the feeling that developer experience less of a priority for SendGrid. The following should illustrate that.

Despite the challenges, we have (laboriously) found workarounds for most of our issues. We are now in a situation where we take from SendGrid what works well for us, and where we leave the rest.

If you think another email marketing provider would be a better fit for us in the long run, or your would like to share your experiences in a similar situation, [I'm interested](/contact)!
## 2. 😕 Unsubscribe Groups are English-only

First up is an issue that underpins many of the other issues. 

Unsubscribe Groups are SendGrid's only built-in way for email recipients to unsubscribe from one of your email lists (Contact Lists). This feature is finely integrated into *Single Sends* (manually sent campaign emails/newsletters) and *Automations*.

To make your life easy as a developer, it's a feature you probably want to use. However, [SendGrid's hosted email preference pages are only available in English](https://stackoverflow.com/a/73325947/4973029), and they are not customizable, at least not for multiple languages at the same time. Like many U.S. companies, SendGrid/Twilio has an explicit and implicit Anglo-American product bias.

OK then, if there is an API, wouldn't it be possible to implement localizable unsubscribe pages yourself? It might be even better because you could keep the pages on-brand. There is at least one bottleneck: you have to *choose* between using Unsubscribe Groups and rolling your own system, you can't combine them.

For these reasons, we worked around the "Unsubscribe Groups" system by using **Contact List membership** to denote being subscribed or unsubscribed, and we host our own unsubscribe pages in our app. This naturally takes some work to implement via the API, but it's possible.

In addition to adding and removing contacts based on our own email preferences system, we also synchronize a `secret` custom field to SendGrid contacts with a personal secret key per user. This can be substituted automatically into the "Custom Unsubscribe Link" in SendGrid (in our case, `https://welcometomygarden.org/email-preferences?e={{email}}&s={{secret}}`), so it is possible for us to authenticate email preference actions on our own hosted pages even when a user is not logged in.

Removing contacts from lists to unsubscribe them is probably not the intended use of Contact Lists (because of the expected use of Unsubscribe Groups). The main issue is that removing a contact from a list does not immediately remove it from its child Segments, which means your unsubscribe may take 24 hours to take effect (see more below for our workaround)! Any type of add/remove action may also take tens of seconds to a minute to be observable. 

If you can just use "Unsubscribe groups", your should probably do so.
## 3. 🤔 Having two accounts (subusers) is recommended by default

Let's say you accept that your users only see an English, SendGrid-hosted email preferences page, and you decide to use Unsubscribe groups. There is one more pitfall: SendGrid's hosted unsubscribe pages always include a "global unsubscribe" option.

If you are using your single SendGrid account for both transactional (e.g. chat notifications) emails, and marketing newsletters, it may happen that someone globally unsubscribes using your newsletter's group unsubscribe link. **Now, they will not receive any email from your SendGrid again, not even important transactional account notices** (because they were globally unsubscribed).

[It is not possible to hide the global unsubscribe link (StackOverflow)](https://stackoverflow.com/a/69262560/4973029). For this situation, a SendGrid developer evangelist suggests either creating your own unsubscribe page (which we did, already because of [[#1. Unsubscribe Groups are English-only|the language issue]]), applying some hacky override parameters to send requests, or, *recommended*, to use "subusers" (which are only available on more expensive tiers):

> If you are on a Pro account or higher, you can set up subusers within your account. **The recommendation is to set up a subuser for your marketing emails and a different subuser for your transactional emails.** That way unsubscribes from the marketing list won't affect the transactional email side of things.

**Subusers** are basically separate SendGrid accounts combined under the same billing account. We do use one subuser for an entirely different project ([Slow Travel Pass](/projects/slow-travel-pass)), where the concept makes sense, but it would be inconvenient to start creating separate subuser accounts for "WTMG - transactional" and "WTMG - marketing".

Here's why: our **contacts** would be split, too. We want to reference the same contacts' (custom) fields for both transactional & marketing emails. Syncing contacts between subusers would cause even more work, and expose a larger surface for issues to appear. I think the recommendation to use separate subusers for transactional/marketing email is a crude patch for a flawed conceptual model, at least for us.

## 4. 🔎 The API design has inconsistencies 

While the API is mostly well-documented and structured logically, you should read the docs very carefully, because there are some inconsistencies and peculiarities.

Some examples:

- Some methods require unique contact IDs as parameters (*delete a contact*), some require email addresses (*upsert contact, add a contact to a list*)
- Some methods expect a JSON-encoded request body (*upsert contact*), some expect a comma-separated list in a query parameter (*delete a contact from a list*)
- The method to *add* emails to a suppression group (= unsubscribe) supports multiple emails. The method to *remove* emails from a suppression group (= resubscribe) only supports one email.
- For a contact export job, the resulting job ID has the key `id`. For a contact import job, it has the key `job_id`.
- When you create a contact, you have to supply custom fields by using SendGrid-generated custom field IDs as keys, for example `{communication_language: "nl" host: 0}` (A) should be given as `{ e3_T: "nl", e2_N: 0 }` (B), where I believe `e3_T` means it's the **3**rd **e**xtra field of the **T**ext type. You have to look up these field key IDs from [the custom field definitions API](https://www.twilio.com/docs/sendgrid/api-reference/custom-fields/get-all-field-definitions), they are not shown in the dashboard where you might have created them. Finally, when you retrieve a contact, the `custom_fields` are returned using the full field names (A). Not the most convenient create-retrieve API.
- There is a powerful contact search endpoint which supports an expressive query language that can accept queries with array-contains clauses of at least 5000 values (the most I've tested). However, it also only returns max 50 contacts. And there is no way to get the next page (or maybe there was, they [killed it](https://community.n8n.io/t/sendgrid-get-all-contacts-limited-to-50/13106/2)). 

## 5. 🔨 Adding a single contact? Use async bulk upsert endpoints

[Add or Update a Contact](https://www.twilio.com/docs/sendgrid/api-reference/contacts/add-or-update-a-contact) is the simplest endpoint you get to add contacts. It supports "30,000 contacts, or 6MB of data, whichever is lower", but if you just want to add one contact, this is what you have to use. In all cases, it does the following:

> Because the creation and update of contacts is an asynchronous process, **the response will not contain immediate feedback on the processing of your upserted contacts**. [...] 
> [...]
> You will see a `job_id` in the response to your request. This can be used to check the status of your upsert job. To do so, please use the [Import Contacts Status endpoint](https://www.twilio.com/docs/sendgrid/api-reference/contacts/import-contacts-status "Import Contacts Status endpoint").
> [...]
> Should you wish to get the resulting contact's ID or confirm that your contacts have been updated or added, you can use the [Get Contacts by Identifiers operation](https://www.twilio.com/docs/sendgrid/api-reference/contacts/get-contacts-by-identifiers "Get Contacts by Identifiers operation").

So, if you want to store the SendGrid Contact ID in your own database, you have to **wait until the job completes (polling its status)**, and then retrieve the contact ID in yet another separate call.

You might want to do this anyway because keeping IDs bidirectionally is a good syncing practice to avoid unnecessary lookup requests. You will need the SendGrid contact IDs, because you *have to* use it later to [remove someone from a contact list](https://www.twilio.com/docs/sendgrid/api-reference/lists/remove-contacts-from-a-list), or to [delete their contact altogether](https://www.twilio.com/docs/sendgrid/api-reference/contacts/delete-contacts).

The contact upsert job takes at least 20 seconds, but **it might take up to 6 minutes** and even longer to finish a job (even it only contains one contact). What if a user deletes themselves before the job is finished? It's a race condition you'll have to handle in your polling code/queue!

One more unhappy surprise: sometimes (admittedly rare, we had ~60/40.000 cases = 0.15%), the creation **job simply fails**. You can then fetch a report URL from the job status endpoint, and perform yet another call to retrieve the XML error document stating the cause of the job failure. It will likely contain an internal timeout error like `<Code>AccessDenied</Code> <Message>Request has expired</Message>`. Maybe SendGrid's internal queue was temporarily overloaded? A simple retry always fixed it for me, but it's good to know their system isn't 100% reliable, not even 99.99%.

## 6. 🐢 Slow Segmentation Lists as a basis for delayed Automations

[Contact List Segments](https://www.twilio.com/docs/sendgrid/ui/managing-contacts/segmenting-your-contacts) are an important part of our SendGrid workflow. For example, they split our "Agreed to the newsletter" parent list into automated sub-lists (segments) for each language (which is a contact custom field), so we can later target these segments with language-specific Single Sends. If a contact changes their language, they will automatically move to the correct segment. 

We also use segments as the basis of all our Automation flows. For these we add additional criteria for segmentation beyond language, like a segment with contacts who added garden, to send new hosts a welcome email. Segments are flexible because they can cover [all kinds of SQL-inspired expressions](https://www.twilio.com/docs/sendgrid/for-developers/sending-email/getting-started-the-marketing-campaigns-v2-segmentation-api) to segment contacts based on their default fields (country, name, ...) and custom fields.

However, Segments also have serious **timing-related limitations**, _especially_ when you don't use of Unsubscribe Groups.

SendGrid [claims](https://www.twilio.com/docs/sendgrid/ui/managing-contacts/segmenting-your-contacts#segment-refresh-cadence) in the docs:

> Twilio SendGrid checks for **newly added** or **modified contacts** who meet a segment's criteria on an **hourly schedule**. Only existing contacts who meet a segment's criteria will be included in the segment **searches** within 15 minutes.

So, if your user does something in your app and you update a contact custom field based on that, which then makes the contact match the entry conditions for a segment, it may take an hour before the contact is actually put into that segment. However, from observations in our app, we noticed that **there are non-negligible outliers where it may take several hours before the change is effected**, sometimes even 10 hours.

This is annoying and slightly misleading when SendGrid's Automation feature also claims to be able to send an "instant" email as soon as a contact enters a segment: the emails will almost never be instant due to the segmentation delays. 

Finally, we have also observed that if a contact **gets removed from the parent list** of a child segment, for an Automation uses that segment with the exit criterium `When contacts no longer meets the entry criteria`, it can take up to **24 hours** before that contact stops receiving emails from the automation flow (from an n=3 sample). This basically means that an unsubscribe would take 24 hours to take effect, which is not acceptable in an email flow where multiple emails may be sent within hours from each other. Imagine: you unsubscribe from a newsletter after receiving email 1, and you still get email 2.

Luckily, we noticed that the **custom exit criteria based on custom fields** act much more quickly, within an hour at least. Therefore, we are now also using a custom field to denote a contact's newsletter subscription status, next to their list membership, and we are using `newsletter = 0` as an exit criterium.

Again, your experience may be better if you use Unsubscribe groups, and don't rely on unsubscribers exiting source lists.

## 7.  ✨ Various caveats

Here is a list of more minor annoyances.

### Confusing & semi-documented handlebar syntax

The SendGrid template handlebar syntax takes some getting used to. Some parts of it are undocumented, see for example this [great discovery of Matias Kinnunen](https://mtsknn.fi/blog/handlebars-else-equals/), who seemed to be in similar sinking multilingual SendGrid boat as I was at some point (we use different templates per language though, not complex if/else constructs).

### Blank/null & value comparisons in Segment conditions

Likely because Segments v2 use SQL internally, segment and search conditions also follow SQL-logic when it comes to null & value comparisons.

In a language like JavaScript, if `const host = null`, then `host !== 1` will be `true`.

In SQL (and SendGrid's conditions), `host != 1` will be `false`. If you want to include the "null" values, you have to use the condition `host IS NOT 1 OR host IS BLANK`

### Inflexible Automation editing

When you want to change the order of emails in a flow, or add an new email in between, you need to fully start again with a new flow, from scratch. An unfortunate limitation, especially considering the cost of the Automations add-on.

If you need to make order changes to a big flow with Design Editor emails (and therefore restart from scratch), I recommend you use the "Export HTML" and "Import HTML" features in the Design editor, so you don't have to re-create every email from scratch.

If you want to change delays between emails, you can edit a duplicated flow, but you can't do this by editing the original.

To make other changes, like an unsubscribe link change, or change in exit conditions, you should disable the flow first before the flow becomes editable. This will lead to (small) downtimes for your flow.

### Permanent ownership

There is no such thing as an "Owner role" that can be transferred onto an existing "workspace" member of the account. The recommended process to transfer SendGrid account ownership is basically the [same process as changing the email address of the owner](https://support.sendgrid.com/hc/en-us/articles/1260802704009-Change-the-Owner-for-a-Twilio-SendGrid-Account).

This might change now that SendGrid is more tightly integrating with Twilio account management.

### Updating a contact's email address? Delete & recreate

Hundreds of developers over the last years have probably wondered, [is changing an email of a SendGrid contact really as complicated as it seems to me? (StackOverflow)](https://stackoverflow.com/questions/79348876/is-changing-an-email-of-a-sendgrid-contact-really-as-complicated-as-it-seems-to)

The answer is *yes*!

It sounds like something pretty common, but no, SendGrid [doesn't support updating a contact's email address directly (StackOverflow)](https://stackoverflow.com/a/71200584/4973029). After you have jumped through the above hoop of getting the SendGrid ID of a contact, you should delete the original contact and create a new one.

# 8. Conclusion

This (rather large!) note should have shown that using SendGrid to drive the needs of a multilingual startup can be a challenge, especially when considering its Marketing components. At the same time, our integration with SendGrid is a sunk cost which meets our present needs. I hope some of these insights might relieve the cost for others just a little.

I am wondering if the grass is greener in other pastures with regards to native support for template translations, but even researching this is not on our radar right now.

For another project ([Women Don't Cycle](/projects/women-dont-cycle-documentary)), I combined [Maizzle](https://maizzle.com/) with a simple storage bucket to write emails in Markdown, render them into HTML in the bucket. These templates are then used together with substitution tags when sending transactional email via SendGrid. I personally appreciated the simplicity of Markdown over WYSIWYG templates, because all three language versions of the same email can actually reuse the same HTML template, and both the template and content can be independently updated.

Another benefit was that we didn't have to use SendGrid's _Contacts_ and _Contact list_ features (and suffer its caveats), every email audience was queried straight from our own database without synchronization steps. Maybe this custom solution could be extended to fit WTMG's needs and the needs of other multilingual organizations better. If we add more languages to our email repertoire, I will consider building our own solution.