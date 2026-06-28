---
title: "Meta's Threads on the Fediverse"
slug: "meta-threads-on-the-fediverse"
published: "2024-03-10T20:27+02:00"
year: 2024
type: "Article"
---

A [short post](https://werd.io/2024/the-fediverse-is-really-happening) from Ben Werdmuller made me aware that Threads is widening tests of its hopefully-upcoming integration with ActivityPub, the microblogging protocol that underpins much of the fediverse, including Mastodon.

It’s been exactly one year since Meta confirmed that it was “exploring a standalone decentralized social network for sharing text updates” ([source](https://www.theregister.com/2023/03/11/meta_twitter_rival/)), eight months since Threads was launched, and two months since their [first public tests with ActivityPub started](https://www.theverge.com/2023/12/13/24000120/threads-meta-activitypub-test-mastodon).

Ben titled his post “The fediverse is really happening”, implying that the fediverse is not *really* happening right now, before Threads, or any other big social media platform, actually joins it.

I follow this sentiment somewhat. The fediverse has been happening for some communities for a long time, but it hasn’t been happening on a larger scale. I think Threads joining the fediverse is good, and I know many don’t agree with this. Here’s my perspective.

## The fediverse has been happening!

The fediverse has been *happening* for a long time, and increasingly so in the last year, with all the drama around X. As a techie, I only [joined](https://indieweb.social/@thorgal) a small year ago, after having being fairly inactive on mainstream social media for years.

I’ve since been having a great time in my small, hand-crafted internet bubble on Mastodon. I mostly [follow](https://indieweb.social/@thorgal/following) fellow web developers and indie web enthusiasts, along with a [former professor](https://mastodon.online/@philipdutre) of mine, and accounts of organizations that I think are worth following.

Mastodon & ActivityPub are already great for tech-adjacent communities, and, as far as I’ve seen, for communities that care less about a wide reach, and more about decentralization, online privacy, and the ability to control and self-moderate their own instances (one example being LGBTQ+ communities). For these communities, the fediverse has already been *really happening.*

The concept of federated servers and applications works really well for niche communities. The grassroots, interpersonal, non-corporate retro feel, often combined with a thematic focus of servers, is also what constitutes most of the appeal for me. I enjoy accessing my “non-algorithmic” Mastodon feed through various clients: [indieweb.social](https://indieweb.social/about)’s default web client with [Bird UI](https://github.com/ronilaukkarinen/mastodon-bird-ui), [Elk](https://elk.zone/), [Raycast](https://www.raycast.com/SevicheCC/mastodon), Mastodon’s iOS app, all using Mastodon's open API.

However, I recognize that Mastodon, Pixelfed and Lemmy are still niche technologies supported by a niche protocol.

## Webmail all over again?

The question is: when is the fediverse *really* happening, on a global scale? You could argue that it’s only *really* happening when it exits its current niche, and is used as widely as incumbent & centralized social media alternatives like X or TikTok. From looking at estimates today on Wikipedia, the account count of Mastodon servers still represents less than 10% of Threads’ account count, while having existed for six more years.

Decentralization and federation present inevitable technical and conceptual hurdles. I believe it's improbable for the fediverse to "really*"* happen in a global sense to without a company like Meta repackaging it as a simple and centralized service.

There is perhaps one other technology where big corporations repackaged an existing open technology, and then managed to distribute it widely: email.

The analogy with email is helpful, because email has had a long history of being an niche technology used in the military, academia, and business. Then, in the 2000s, big corporate webmail providers like Gmail, Yahoo and Hotmail brought it into the homes of “the public”. It wasn’t only easy to create a new email address with a free inbox, you also didn’t have to install any special software to access this inbox. Email practically got equated with the single web service you used to access it.

In a [2022 blog post](https://ar.al/2022/11/09/is-the-fediverse-about-to-get-fryed-or-why-every-toot-is-also-a-potential-denial-of-service-attack/) arguing for more decentralization on the fediverse, Aral Balkan stated this development was bad for email, because it centralized power over email (an open system) to a few entities:

> Have you ever heard of a little old email instance called Gmail? (Or perhaps the term [“embrace, extend, extinguish?”](https://en.wikipedia.org/wiki/Embrace,_extend,_and_extinguish))
> 
> Do you know what happens to your email if Google says (rightly or wrongly) that you’re spam? No one sees your email.

There is something here: in practice, email isn’t anymore the decentralized dream it once was believed to be. But, I’m not as pessimistic about ActivityPub following the email precedent. I’m more on the optimistic side.

First, we can ponder the counterfactual: would email have reached the ubiquitous adoption it has today without being carried by webmail providers? The simplicity of Hotmail made it possible for me and my primary school classmates to use email on our parent’s computers before we were ten years old.

By analogy, Threads could introduce millions of people to the fediverse. Many will never understand or care about the difference between @threads.net and @mastodon.social, but some (millions!) will, and they will realize that they can join independent ActivityPub servers too.

Secondly, I think email, in its current state of oligopoly, is still in a better shape than social siloes like Instagram, Snapchat and TikTok that can’t talk to each other at all. When several big platforms are interconnected, there is at least some healthy diversity. And the more interconnected they become, the harder it is for them to break out without alienating their userbase.

Finally, I don’t believe the dreaded “extinguish” phase presents a real danger to the status quo, or to the intrinsic, relatively slow growth of ActivityPub. Here’s a quote from [a post on the Mastodon blog](https://blog.joinmastodon.org/2023/07/what-to-know-about-threads/) by founder Eugen Rochko, published on July 5th, 2023, the day that Threads launched (emphasis mine):

> There was a time when users of Facebook and users of Google Talk were able to chat with each other and with people from self-hosted XMPP servers, before each platform was locked down into the silos we know today. What would stop that from repeating? Well, **even if Threads abandoned ActivityPub down the line, where we would end up is exactly where we are now.** XMPP did not exist on its own outside of nerd circles, while ActivityPub enjoys the support and brand recognition of Mastodon.

As mentioned before, ActivityPub and Mastodon already have found a home in niche communities. I personally am not planning to leave my niche. I’m avoiding corporate algorithmic feeds out of principle, and from a mental health perspective. I have been doing so for some five years now, and I don’t expect my motivations to change soon.

If I can stay in my hand-picked, ad-free, "algorithm-less" Mastodon feed with 90% Mastodon accounts and 10% Threads or Bluesky accounts; I will be happy.

If, after three years, Threads decides to kill its ActivityPub integration, I will be a little bit sad for a while, but mostly still happy, because I mostly want to stay in touch with people who follow the same principles as I do (and are thus on Mastodon, and not on Threads).

## Practicalities

But how will Threads integrate with ActivityPub, in practice?

From [Evan Prodromou’s first test post](https://indieweb.social/@evanprodromou@threads.net/112056448550748474), as part of a closed beta, it seems that Meta’s ActivityPub implementation works only in one direction today:

- Participating accounts on Threads can be seen and followed by followees from the wider fediverse.
- Replies from participating Threads accounts can also be seen by the wider fediverse.
- … but if the wider fediverse replies to a Threads posts, those replies won’t be visible in Threads.

This single direction limits the usefulness of the integration today, but better interoperability should come gradually. From [a blog post from January by Tom Coates](http://plasticbag.org/archives/2024/01/how-threads-will-integrate-with-the-fediverse/) who met with the team integrating Threads with ActivityPub:

> let’s talk about the roadmap they laid out, which is as follows:
>  **December 2023**
> A user will be able to opt in via the Threads app to have their posts *visible* to Mastodon clients. People would be able to reply and like those posts using their Mastodon clients, but those replies and likes would not be visible within the Threads application. Threads users would not be able to follow or see posts published across Mastodon servers, or reply to them or like then.
> 
> **Early 2024**  (Part One) – the Like counts on the Threads app would combine likes from Mastodon and Threads users
> 
> **Early 2024** (Part Two) – replies posted on Mastodon servers would be visible in the Threads application
> 
> **Late 2024** – A “mixed” Fediverse and Threads experience where you will be able to follow Mastodon users within Threads, and reply to them and like them
> 
> **TBD** – Full blended interoperability between Threads and Mastodon

I am yet to read this post fully at the time of writing, but it looks optimistic!

I think we’re slowly moving towards an interesting, new, social media world order.
