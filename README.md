# vivaldi_1.9

## Skip console log for Vivaldi Render Process

### Detail
<a href="https://github.com/WillyYu/vivaldi_1.9/commit/996e6b6a78b3a0e3aeccfa0fd9bdb4bcb6c1e69d">Commit</a>

Some websites have lots message (Debug/Warning/Error) during page loading, like Yahoo, ...
By Chromium's design, each WebView's console log message will be sent to the Vivaldi's app.
But Vivaldi app doesn't listen and handle those console message event.
So that we can skip it to reduce the loading of Vivaldi's CrRenderMain and enahce the performance.

