---
created: 2025-03-04
stage: published
---
I recently encountered an issue with SendGrid's Inbound Parse service, or at least, that's what I thought! It turned out to be a situation caused by unintuitive behavior in Fastmail.

## The "problem"

The issue, as I observed it, was as follows: whenever I would send an email from `thor` \<at\> `slowby.travel` to our Sendgrid Inbound Parse domain `reply` \<at\> `chat.welcometomygarden.org`, I would not be able to find a single log for the incoming email. The email just seemed to disappear inside SendGrid, but it also didn't bounce.

Strangely, I could successfully send emails to the same domain from all other email addresses I control, and I could also successfully send emails to _other_ domains from `thor` \<at\> `slowby.travel`. This situation was annoying, since the address was one with which I wanted to test the Inbound Parse endpoint.

What I eventually figured it out was: the emails never left Fastmail!

## The actual problem: Fastmail

We have two Fastmail multi-user email accounts, one for the domain `slowby.travel` and one for `welcometomygarden.org`. Fastmail has MX record control over those root domains, but **not** over its subdomains.

SendGrid has an MX record pointing to its servers on the subdomain`chat.welcometomygarden.org`, which is currently only used for email reply parsing.

What happened was that when I sent an email from Fastmail (that means; composed in Fastmail's webmail, or sent via Fastmail SMTP servers) from`thor` \<at\> `slowby.travel` to `reply` \<at\> `chat.welcometomygarden.org`, Fastmail did two things:
1. Because the MX record of `welcometomygarden.org` was controlled by Fastmail, Fastmail applied **internal routing** ([semi-outdated 2008 docs](https://www.fastmail.com/blog/improved-virtualown-domain-handling-and-activation/)): it did not even bother looking up the MX record of `chat.welcometomygarden.org`, it assumed that the destination email server was Fastmail itself, and specifically the `welcometomygarden.org` email account.
2. Even though it does not control subdomains of `welcometomygarden.org`, Fastmail applied a mechanism called [**subdomain addressing**](https://www.fastmail.help/hc/en-us/articles/360060591053-Plus-addressing-and-subdomain-addressing), whereby `reply` \<at\> `chat.welcometomygarden.org` is rerouted to `[mainuser]+reply` \<at\> `welcometomygarden.org`.

*Thus, my test emails were landing in our WTMG Fastmail support inbox, without me even noticing it.*

This also meant that all WTMG users who had accounts with Fastmail and who tried to reply to a WTMG email were unable to do so successfully. Luckily, judging from our support inbox, there were none.

## Solution

Imagine my relief when I noticed that Fastmail allows one to force external routing, which precludes both problem 1 (MX records _are_ looked up) and 2 (Fastmail doesn't handle the email anymore!).

![](<fastmail-routing-settings.png>)

Still, I think it is counterintuitive that Fastmail is manipulating emails to subdomains over which it has no control, so I filed a feature request to ask them to avoid this behavior by default.

