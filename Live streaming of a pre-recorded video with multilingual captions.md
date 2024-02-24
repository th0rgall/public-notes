---
stage: draft
---

I did some broad research on this, because we wanted a solution for Women Don't Cycle. Warning: from this research, we concluded it was not suitable for our use case. We ended up playing embedding a pre-recorded video with Bunny CDN's player, where it's easy to configure multiple caption, and then added a live stream Q&A alongside (without live captions).

My research on **live streaming of a pre-recorded video with multilingual captions:**  

- It seems to only be supported by smart “RTMP” stream provider platforms like the above (which can stream to YT/Vimeo/…), but not out-of-the-box by Vimeo/YouTube’s own tools.

- It is possible, as proven by [https://www.clevercast.com/demo-multilingual-livestream-captions/](https://www.clevercast.com/demo-multilingual-livestream-captions/) and [https://www.clevercast.com/multilingual-live-captions/](https://www.clevercast.com/multilingual-live-captions/) - in its pricing 
- It’s hard to find how to configure this, except the above page.


Youtube's LiveStreaming API has [an endpoint](https://developers.google.com/youtube/v3/live/docs/liveBroadcasts#contentDetails.closedCaptionsType) which supports EIA-608 and/or CEA-708 formatted catpions
https://stackoverflow.com/questions/66143575/sending-captions-to-a-youtube-live-stream

tutorial here: https://www.spf.io/2021/03/30/how-to-translate-youtube-live-subtitles-for-livestream/ seems one target

not possible on youtube, one person suggests without clear example
https://stackoverflow.com/questions/32614603/youtube-live-streaming-captions-more-than-one-language


- It’s not worth it in terms of cost (hundreds to thousands of euros) because it’s really aimed at large-scale enterprise use.