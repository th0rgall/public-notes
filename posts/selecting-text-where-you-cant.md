---
title: "Selecting text where you can't"
slug: "selecting-text-where-you-cant"
published: "2018-11-29T00:00+02:00"
year: 2018
---

Sometimes you want to copy-paste text from a web page, but it won't work. Here are a few common reasons and workarounds.

### 1. The text is embedded in a link

The text might be contained by a link. If you click+drag to select text, you will move the link element! Sometimes it is not visible that the text you're trying to select is, in fact, a link.

**Solution: hold the ALT key while selecting (Option key on Mac)**. This will allow you to select text in a clickable area like a button

<a href="" class="example-select-link">Try to select me!</a>
<script>
function addStyleString(str) {
    var node = document.createElement('style');
    node.innerHTML = str;
    document.body.appendChild(node);
}

addStyleString(`.example-select-link:visited, .example-select-link:link, .example-select-link:hover {
	font-size: 1.2em;
	text-align: center;
}`);
</script>

### 2. Selecting is intentionally disabled on the site

The site developer probably does not want you to copy the text. This happens regularly on news websites. They include JavaScript code that captures all your clicks and selections and 'kills' these events. **Do not try this on a site where you don't want to lose progress of some sort**

**Solution: block JavaScript on the site**

1. Install the μBlock Origin plugin ([Firefox](https://addons.mozilla.org/nl/firefox/addon/ublock-origin/), [Chrome](https://chrome.google.com/webstore/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm), [Safari](https://safari-extensions.apple.com/details/?id=com.el1t.uBlock-3NU33NW2M3), [Microsoft Edge](https://www.microsoft.com/fi-fi/p/ublock-origin/9nblggh444l4?rtc=1&activetab=pivot:overviewtab))
2. Click on the μBlock icon to scripts for the site 

  
  ![Screenshot 2018-11-29 at 18.20.35](./assets/selecting-text-where-you-cant/screenshot-2018-11-29-at-18.20.35.png)
  
3. Reload the page
4. Copy/pasting is now possible. 

You want to re-enable the JavaScript after your copy-paste, a lot of sites depend on it today to function properly.

PS: μBloc Origin is a versatile, lightweight and open-source ad-blocker. It is probably better than other adblock plugins you might have installed.
