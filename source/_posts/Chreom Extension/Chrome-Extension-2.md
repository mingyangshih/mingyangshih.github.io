---
title: Before Start to Develop Chrome Extensionsd
date: 2025-01-02 21:44:24
description: When to reload the extensions, show the logs and errors.
categories: Extension
tags:
  - Extension
---
## When to reload the extension
The following table shows which components need to be reloaded to see changes:

| Extension component | Requires extension reload |
| -------- | ------- |
| The manifest  | Yes |
| Service worker | Yes |
| Content scripts | Yes (plus the host page) |
| The popup | No |
| Options page | No |
| Other extension HTML pages | No |

## Find console logs and errors
### Console logs

During development, you can debug your code by accessing the browser console logs. 
Right click the popup html, then you can check the console message just like develop the web.

## Structure an extension project
There are many ways to structure an extension project; however, the only prerequisite is to place the manifest.json file in the extension's root directory.


