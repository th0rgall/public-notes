---
stage: published
---

*Disclaimer: YMMV. Your Kindle may have a better browser than my 2015 model. Let me know if it does!*
## Introduction

**The Kindle web browser seems like an absolute afterthought**. As if nobody at Amazon in the last decade expected people to actually use it.

Not only was it already absurdly old at its release date (based on code [[#^b04b86|from 2009]], even many _old_ standard browser features are also unsupported or badly supported. This makes many websites unusable. Nowadays, many top websites (or their JS) even freeze the Kindle browser, so that you have to restart it.

Aside from that, the user experience of the browser is *terrible*. One big problem here is that the browser reports a viewport width equal to its actual pixel width (1072 pixels): this makes every site appear as a tiny, zoomed-out desktop site. If the Kindle were to report half the viewport size, then the it would at least present sites in a mobile format more suitable to its screen.
Now, even if a site works, a user invariable has to muck around with slow zooming-and-panning to read any text.

From a web developer's perspective, the situation is equally abysmal. I couldn't find any documentation on the browser from Amazon (or from anywhere, for that matter); so if you want your site to work on the Kindle, you're on your own.

These reasons are probably why virtually nobody seems to care about this browser, except some quirky developers like me, who played with it for their own projects.

And yet, the browser has potential! However bad it is, it gives the Kindle a view onto the interactive open web. Sure, you can use *Send to Kindle* and enjoy web articles this way, but the web is more than just static information. What if we could design a web *app*, _specifically_ for the Kindle browser? What value would that bring?

My Kindle also has great assets: a high-resolution e-ink display. And even a pretty workable virtual keyboard. 

This is what I set out to do with **Readup.ink**: to port the [Readup reading platform](https://readup.org) to the Kindle web browser. I'm happy with how it turned out! My Kindle now works as a perfectly serviceable social web reader.

It has limitations for sure (you can't select or highlight text, and the browser bar/chrome always stays on top), but 
## My test device

Everything here should be prefaced with the observation that I tested the browser on my Kindle device, which is a **Kindle Paperwhite 3 (7th generation) from 2015.** ([Wikipedia](https://en.wikipedia.org/wiki/Amazon_Kindle#Kindle_Paperwhite_(third_iteration))).

At the moment of writing, it is running the "Firmware Version" 5.16.2.1.1 (40974470002)

Relatively recently, it got an update with a UI overhaul. This update also dropped the word "Experimental" from the name "Experimental Web Browser". It still remains very experimental, as you can read here.

## Browser details

^b04b86

- The Kindle browser is based on WebKit.
- It's user agent is `Mozilla/5.0 (X11; U; Linux armv7l like Android; en-us) AppleWebKit/531.2 (KHTML, like Gecko) Version/5.0 Safari/533.2 Kindle/3.0`, reporting a WebKit version of 531.2. 
	- This corresponds to **Safari 4** versions released for Mac OS X Tiger in **the summer of 2009** ([Wikipedia](https://en.wikipedia.org/wiki/Safari_version_history#Safari_4)). This may be a useful indicator to look up in MDN/CanIUse "Browser Compatibility" tables, because WebKit versions are generally not mentioned in compatibility tables.
- Amazon distributes (parts of?) its Kindle WebKit code in this [open-source disclosure](https://www.amazon.com/gp/help/customer/display.html%3FnodeId%3D200203720). At the time of writing, it reports a webkit version of `webkit-1.0_1.4.2` for my 5.16.2.1.1 Kindle Paperwhite (7th gen).

## Viewport

- The reported viewport size TODO
- But the actually usable screen TODO


## Tips for adventurers

If you want to make a website suitable for the Kindle, these tips might help.
### jQuery 3.7 seem to work generally!
I haven't encountered something that it couldn't do, that it was supposed to be able to do. Granted, I am polyfilling the browser in several uses of jQuery.

### Transpilation & polyfilling helps
TODO

## Quirks

The browser is not just old, it's also very limited and weird. Here are some quirks I dealt with.

#### Document height is always larger than screen height
TODO: mention

#### The scroll bar is a (supposedly) permanent _overlay_
Related to the above document height issue.
- You would expect that setting your document container to `height: 100%; overflow: hidden;` would hide any scrollbar (because there is no content to scroll). But no. No standard method of hiding the scrollbar works (I've tried).
- You would also expect that your page content can't go under the scrollbar, even if you set `body` margin/padding properties to 0. In the Kindle, this happens. 

**I haphazardly found one hack that worked**: overflowing the width by a few % (`103%`), in a specific configuration with several nested scroll containers (TODO: link). I suspect this triggers some browser bug.

#### `<a>` links are always underlined
^kindle-links

`text-decoration: none` has no effect on them.

#### Editing the browser URL

## Browser feature support reference

These are the features I've tested.
### HTML

| Feature | Working? | Comment |
| --- | --- | --- |
| `<svg>` | ❌ | |
| `<meta name="viewport">` | ❌ |

### CSS

| Feature | Working? | Comment |
| --- | --- | --- |
| flexbox, grid | ❌ |
| `position: sticky`, `position: fixed` | ❌ | they seem just static |
| `rem` | ✅ |


### JS / DOM
Generally, only **ES5** is supported. No ES6 features. I'm only mention a few very popular ES6 features.

| Feature | Working? | Comment |
| --- | --- | --- |
| template strings \`${}\`| ❌ | ES6 |
| arrow functions `() => {}`| ❌ | ES6 |
| `element.scrollTo()` | ❌ |
| `scrollTop = ...` | ⚠️ | only on non-window/document elements |
| window.clientWidth | ⚠️ | requires `body {width: 100%}` to give a non-0 result |
| `localStorage` API | ✅ | surprisingly! |

#### Events
Very limited. There is no way to implement a dragging operation, or interact in any meaningful way with the touchscreen beyond "taps". 

Here is what I tried:

✅ **Working as expected**
- `click`
- `mouseout` is interesting: it only triggers on the first click outside an element that was clicked. Not sure if that's the specced behavior.
- `scroll`: it looks like a long drag triggers several `scroll` events.

⚠ **Triggering, but useless**
- `mouse{move, down, up, over}` are essentially all aliases of  the `click` event. They get triggered, but only _after_ a single tap.

❌ **Not triggering
- `mouse{enter, leave}; pointer*; drag*, touch*; visibilitychange; 
- `dblclick` does not trigger an event, but results in a native toggle-able zoom action on an element. It's not possible to `preventDefault()` this.

## Things I tried, but failed to accomplish

- Configuring SvelteKit for Kindle/ES5-compatible code.
- Configuring NextJS for Kindle/ES5-compatible code.

Here is what I still want to test [[Kindle Browser benchmarking]]