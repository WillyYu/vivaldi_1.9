# vivaldi_1.9

## Skip console log for Vivaldi Render Process

### Detail
Each WebView's console log message will sent to the host.
Some website has lots message (Debug/Warning/Error) during page loading.
Vivaldi didn't listen WebView's console message event, so that can skip it to reduce the loading of Vivaldi's CrRenderMain.


