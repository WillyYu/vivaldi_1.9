# vivaldi_1.9

## 1. Skip console log for Vivaldi Render Process

Some websites have lots message (Debug/Warning/Error) during page loading, like Yahoo, ...
By Chromium's design, each WebView's console log message will be sent to the Vivaldi's app.
But Vivaldi app doesn't listen and handle those console message event.
So that we can skip it to reduce the loading of Vivaldi's CrRenderMain and enahce the performance.

<a href="https://github.com/WillyYu/vivaldi_1.9/commit/996e6b6a78b3a0e3aeccfa0fd9bdb4bcb6c1e69d"> >>Commit<< </a>

## 2. Dispatch the events to right script context

By Chromium's design, the extension events (ex: webViewInternal, tabs, ..) must sent to each script context (Vivaldi's host window) sequentially. It means the more Vivaldi window is opened, the more contexts need to process those events.
In Vivaldi's design, events will be discard after id examining. But it must do it in javascript contexts.
Like below trace, the tabsPrivates event was disptached to each script context. But not all script context need to handle it.

<img src="https://github.com/WillyYu/vivaldi_1.9/blob/master/image/dispatchEvents.png?raw=true"/>

It is not efficiency, It will
 1. execute more unnecessary code
       need switch to v8 context but nothing to do
 2. redundant copy
    Every event arguments must be copyed to v8 context.
    especially the tabs.onUpdated and tabsPrivate.onFaviconUpdated events, there are thumbnails were included in those event args. it will cause large size of redundant copy.

How do I do:<br>
Record/Remove the vivaldiWindowId and tabId when onElementAttached/Detached. and then decide whether the event needs to be dispatched according to the event's arguments. The event handling will be faster

<a href="https://github.com/WillyYu/vivaldi_1.9/commit/19f15503fdbf9d7161a1aa3f30a7f919fc8b5b0f"> >>Commit<< </a>

<img src="https://github.com/WillyYu/vivaldi_1.9/blob/master/image/dispatchEvents_opt.png?raw=true"/>
