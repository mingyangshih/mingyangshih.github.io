---
title: Run Content scripts on pages.
date: 2025-01-06 21:33:14
description: Run content scripts on the pages that would like to use.
categories: Extension
tags:
  - Extension
---

## Declare the content script
Extensions can run scripts that read and modify the content of a page. These are called content scripts. They live in an isolated world, meaning they can make changes to their JavaScript environment without conflicting with their host page or other extensions' content scripts.

## Register a content script called `content.js`.
`matches`: Allow the browser to identify which sites to inject the content scripts into. Match patterns consist of three parts: `<scheme>://<host><path>`. They can contain '*' characters.

Link: [Match Patterns](https://developer.chrome.com/docs/extensions/develop/concepts/match-patterns)

```JSON
{
  "content_scripts": [
    {
      "js": ["scripts/content.js"],
      "matches": ["https://*/*"]
    }
  ]
}
```

## Content scripts
Content scripts can use the standard Document Object Model (DOM) to read and change the content of a page.


## Other Notes
- [Optional chaining used to access an object property that may be undefined or null.](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Optional_chaining)
- [Nullish coalescing returns the <heading> if the <date> is null or undefined.](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing)

