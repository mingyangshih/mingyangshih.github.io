---
title: Chrome Extension - 1
date: 2024-01-04 23:43:29
description: Try to create a extension for work
categories: Extension
tags:
  - Extension
---
## Create a chrome extension project
Create a `manifest.json`, `background.js` and an icon.
* `manifest.json` is for the browser to know how to use your project's files, where to read your file and what authority you need.

* name: Extension's title.
* description: Extension's description.
* version: The version of extensions that is used to version control.
* manifest_version: What does the version of develop kit the extension use.
* background: The path of the js file that run in the background.
* action: Setting some actions when we clicking the icon
* icons: Setting the icons position.
* permissions: The browser authorities that the extension will use.

``` json
//manifest.json
{
  "name": "Chrome Extension Ad Request",
  "description": "Chrome Extension Ad Request Intercept",
  "version": "1.0.0",
  "manifest_version": 3,
  "background": {
    "service_worker": "background.js"
  },
  "action": {},
  "icons": {
    "16": "./icon.png",
    "32": "./icon.png",
    "48": "./icon.png",
    "128": "./icon.png"
  },
  "permissions": ["tabs"]
}
```
* `chrome.tabs.onUpdated.addListener`: Used to listen whether the tab's status is changed.
* `chrome.tabs.remove`: Used to close one one or more tabs.
```js
// background.js
chrome.tabs.onUpdated.addListener(function (tabId, changeInfo, tab) {
  // tabId : The id of the tab which triggered the event.
  // changeInfo: The change information.
  // tab: The tab object.
  if (tab.url.includes("netflix")) {
    chrome.tabs.remove(tabId);
  }
});
```
