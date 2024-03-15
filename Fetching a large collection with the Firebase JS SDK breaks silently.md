---
title: Requesting thousands of documents with Firestore’s JS SDK breaks silently on low-end Android devices
created: 2024-03-15
---
This week, I worked around a nasty Firebase problem. A critical part of our web app stopped working for some people.

The [Welcome To My Garden map](https://www.notion.so/firebase-bug-d1044651b7e544ceb2678be7f7b77af1?pvs=21) queries and fetches all ~4500 listed gardens on each visit. In our app, [gardens](https://github.com/WelcometoMyGarden/welcometomygarden/blob/793991fe73ed035149055235e43970b182377cfe/src/lib/types/Garden.ts) are small Firestore documents, with about 20 fields each, that live in a single collection.

Recently, we got occasional reports of the **gardens not appearing for some people**, mostly on low- to mid-range Android devices: they somehow didn't load, while the rest of the site (and other Firebase data) did.

My cofounder was experiencing the issue on his midrange 2019 Android phone, on Firefox and Chrome, and I could reproduce it on a 2019 Samsung Galaxy Tab S5e on BrowserStack too.

**A successful workaround for us was to [use the Firestore REST API](https://cloud.google.com/firestore/docs/reference/rest/?apix=true) instead of the Firebase JS SDK for this specific query.** Because WTMG is open-source, I can also [link you to the fix](https://github.com/WelcometoMyGarden/welcometomygarden/compare/7bde8d4..89de6c834fe3aa4946d1d8d6717b186c55d06f1c)!

I’m writing this note for two reasons: 
1. I couldn’t find an exact precedent for this problem on the internet, which I found surprising. Surely we aren't the first to fetch 4500 documents with the Firebase SDK on low-end Android devices? Maybe the workaround I found might help out someone else in this situation.
2. I still haven’t pinpointed the root cause of this JS SDK problem (bug?), and I would like to know if someone has any pointers. Because I don't have a fix yet, only a workaround.

What follows are more details about my debugging journey.

------
## The problem

When investigation the reproduction in BrowserStack, it became clear that [the query Promise executed with Firebase JS SDK's](https://github.com/WelcometoMyGarden/welcometomygarden/blob/793991fe73ed035149055235e43970b182377cfe/src/lib/api/garden.ts#L43-L44) `await getDocs(query(...))` just didn't resolve. But it also didn't reject, nor time out.

This happened about ~50% of the time on affected devices. The other half o the time it worked fine.

## Trying chunked/paginated data

I got some extra insight when I chunked the 4.5K documents into 500-documents chunks using query pagination.

**The first chunk would always get in, but later chunks (totalling 1.5K, 2K, 3K...) would get stuck with the infinite loading problem.** When exactly it got stuck varied greatly on different attempts.

I also tried inserting a delay between fetching each chunk (10ms - 2 seconds), which had a positive effect on the Samsung tablet, but not on my cofounder's phone.

This lead me to suspect some kind of memory problem.

It was strange though: 4500 documents is maybe more than you would typically fetch in one query, but if we estimate that each one is about 0.5 KB in size, we still end up with about 2.25 MB of data. That should be manageable even by five-year-old smartphones, right?

## Trying to find precedents

### A super slow query?

Searching online for "firestore fetch hangs up with many documents" resulted in many issues and complaints about *slow queries*. Maybe our query was just super slow?

Firebase has a blog post [covering how to avoid "slow" queries](https://medium.com/firebase-developers/why-is-my-cloud-firestore-query-slow-e081fb8e55dd), but our query didn't seem to fit these cases.
- our query wasn't fetching 150MB of data (75x less)
- our client didn't have an offline cache (persistence is off),
- and since the query still resolved relatively fast on most devices, the idea that slow (server-side) zig-zag merges played a role, or that network geography mattered, also didn't make sense.

This query wasn't slow. The answer just didn't come in, even after waiting minutes!

### Memory constraints?

Because reports came in mostly for low-end Android devices, I suspected a memory-related issue from the start.

Searching for "firebase get documents hangs up when thousands items requested on older android phone memory usage" mostly lead to complaints about possible memory leaks with hundreds of megabytes of memory used.

I tried to investigate memory usage by looking at the Chrome "Performance" profile of the query on my laptop. I generally saw the JS heap size jump from ~20 MB to ~80 MB when fetching gardens on a working system: that didn't seem abnormally high.

**But how much queried data is too much?**

Some more searching eventually lead to [this very interesting Firestore Android SDK issue](https://github.com/firebase/firebase-android-sdk/issues/1052) from Dec. 2019. It sounded so similar to our issue:

> **Title: Low-end and mid-range devices cannot handle large collections of documents.**
> 
> The main issue we are having is with large collections of documents. **When we have for example a collection of 6000 document and we need to have them all on device, the phone/app runes out of memory**, or it's allocating is lasting for hours.

The responses from the Firebase engineer that tried to help the poster were also illuminating. Some quotes:

> Unfortunately, you're running up against limitations in the Firestore SDK and the VM on these kinds of devices. **From the log in the screenshot you posted, the heap is limited to 192 MB, which sounds like a lot except it's shared with code, and whatever else you're using.**

> Firestore exposes the whole result set and shows what changed from snapshot to snapshot, making it really easy to directly integrate into UIs. **However, this means it has to be able to hold the whole snapshot plus the metadata and overhead in RAM plus the next snapshot for comparison.**

The code was only fetching one snapshot (as far as I know), and it wasn't listening for query updates. But still: maybe the mobile browser couldn't allocate 80MB of RAM for the Firestore JS SDK to use? It sounds plausible given these limitations on the Android SDK.

It was strange though that other websites that consume more than 100MB of memory still worked on these devices.

**Maybe there was some "hidden" memory usage?**

In that regard, I also found this more recent issue: [Huge Memory Leak Issue](https://github.com/firebase/firebase-js-sdk/issues/4416), where another engineer stated:

> I can confirm that **with high traffic, the Web Channel memory consumption can increase significantly** (up to 300MB on newer versions of Chrome and even higher than that on older ones). This is because new server responses get attached to the same `XmlHttpRequest` which only gets recreated periodically (I believe it's every minute). Unfortunately, this isn't something we can fix in the SDK.

Maybe server responses didn't qualify as memory used by the web app in profiling, but as memory used by the browser?

In the latter case, it might explain why the memory usage seemed normal in the Performance tab, but that the browser had internal memory management issues due to this Web Channel buffering behavior. I'm not sure.

## Finding the workaround

I was under pressure to find a solution for our users quickly. The app was critically broken for them, after all.

Since the issue *seemed* related to the Web Channel or Firebase SDK snapshot/caching behavior, I realized that changing the API channel might help. And it did!

**Using the Firestore REST API was a simple and functional plug-in replacement** for the JS SDK query. It was applicable to our case, since we didn't need streaming realtime listeners, which would have necessitated the use of the SDK.

**The results:** 
- All gardens now come in with 7.7MB of JSON data (1.1MB gzipped).
- The query works well on affected devices. 
- Despite shipping raw JSONs over the net, it seems to return data faster than it did with the overhead of the JS SDK Web Channel.

Maybe using [grpc-web with the Firestore gRPC API](https://stackoverflow.com/a/66248206/4973029) would make it faster still.

## What I didn't try

It's fair to point out that I didn't try the following:

- I didn't manage to run a performance (memory) profiler on an affected device. Accessing my cofounder's phone remotely was cumbersome, and BrowserStack's devtools also didn't have built-in memory profiling. I didn't attempt this any further.
- I didn't try to create a **minimal reproduction**: generating and querying 5000 or 10000 docs with a similar size and content in a minimal (local) vanilla JS Firebase app, and seeing if the query didn't work on the affected devices.
- Updating my `firebase` dependency. It is currently on `10.4.0`, which is five minor versions behind the latest `10.9.0` at the time of writing. 

**The above would be a to-do list to actually file this as a bug with Firebase.** Maybe it's not an (memory) issue in the Firebase SDK, but related to some unknown other factors of our app.

I also didn't try these approaches to fix the issue, because the REST route worked out first:
- The second memory issue mentioned ["long polling" settings](https://firebase.google.com/docs/reference/js/firestore_.experimentallongpollingoptions) for the Firebase SDK. Maybe force-disabling long polling could have helped with the memory management.
- I could have implemented geohashes to cut down on requested data, depending on the map view state. This would have changed the UX of our site: we'd have needed deeper default zoom levels for it to make a difference, and potential small delays to requery data when the map view changed. That's not something we wanted to implement as a bug fix. This will be an inevitability as some point, though.
