

# Android PdfViewer

Library for displaying PDF documents on Android, with `animations`, `gestures`, `zoom` and `double tap` support.
It is based on [PdfiumAndroid](https://github.com/barteksc/PdfiumAndroid) for decoding PDF files. Works on API 11 and higher.
Licensed under Apache License 2.0.

## What's new in 2.0.0?
* few API changes
* improved rendering speed and accuracy
* added continuous scroll - now it behaves like Adobe Reader and others
* added `fling` scroll gesture for velocity based scrolling
* added scroll handle as a replacement for scrollbar

2.0.1 fixes NPE when onDetachFromWindow is called.

2.0.2 fixes exceptions caused by improperly finishing rendering task.

2.0.3 fixes scroll handle NPE after document loading error

## Changes in 2.0 API
* `Configurator#defaultPage(int)` and `PDFView#jumpTo(int)` now require page index (i.e. starting from 0)
* `OnPageChangeListener#onPageChanged(int, int)` is called with page index (i.e. starting from 0)
* removed scrollbar
* added scroll handle as a replacement for scrollbar, use with `Configurator#scrollHandle()`
* added `OnPageScrollListener` listener due to continuous scroll, register with `Configurator#onPageScroll()`
* default scroll direction is vertical, so `Configurator#swipeVertical()` was changed to `Configurator#swipeHorizontal()`
* removed minimap and mask configuration

## Installation

Add to _build.gradle_:

`compile 'com.github.barteksc:android-pdf-viewer:2.0.3'`

Library is available in jcenter repository, probably it'll be in Maven Central soon.

## Include PDFView in your layout

``` xml
<com.github.barteksc.pdfviewer.PDFView
        android:id="@+id/pdfView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
```

## Load a PDF file

All available options with default values:
``` java
pdfView.fromUri(Uri)
or
pdfView.fromFile(File)
or
pdfView.fromAsset(String)
    .pages(0, 2, 1, 3, 3, 3) // all pages are displayed by default
    .enableSwipe(true)
    .swipeHorizontal(false)
    .enableDoubletap(true)
    .defaultPage(0)
    .onDraw(onDrawListener)
    .onLoad(onLoadCompleteListener)
    .onPageChange(onPageChangeListener)
    .onPageScroll(onPageScrollListener)
    .onError(onErrorListener)
    .enableAnnotationRendering(false)
    .password(null)
    .scrollHandle(null)
    .load();
```

* `enableSwipe` is optional, it allows you to block changing pages using swipe
* `pages` is optional, it allows you to filter and order the pages of the PDF as you need
* `onDraw` is also optional, and allows you to draw something on a provided canvas, above the current page

## Scroll handle

Scroll handle is replacement for **ScrollBar** from 1.x branch.

If you want to use **ScrollHandle**, it's important that the parent view is **RelativeLayout**.

To use scroll handle just register it using method `Configurator#scrollHandle()`.
This method accepts implementations of **ScrollHandle** interface.

There is default implementation shipped with AndroidPdfViewer, and you can use it with
`.scrollHandle(new DefaultScrollHandle(this))`.
**DefaultScrollHandle** is placed on the right (when scrolling vertically) or on the bottom (when scrolling horizontally).
By using constructor with second argument (`new DefaultScrollHandle(this, true)`), handle can be placed left or top.

You can also create custom scroll handles, just implement **ScrollHandle** interface.
All methods are documented as Javadoc comments on interface [source](https://github.com/barteksc/AndroidPdfViewer/tree/master/android-pdf-viewer/src/main/java/com/github/barteksc/pdfviewer/scroll/ScrollHandle.java).


## Additional options

### Bitmap quality
By default, generated bitmaps are _compressed_ with `RGB_565` format to reduce memory consumption.
Rendering with `ARGB_8888` can be forced by using `pdfView.useBestQuality(true)` method.

### Double tap zooming
There are three zoom levels: min (default 1), mid (default 1.75) and max (default 3). On first double tap,
view is zoomed to mid level, on second to max level, and on third returns to min level.
If you are between mid and max levels, double tapping causes zooming to max and so on.

Zoom levels can be changed using following methods:

``` java
void setMinZoom(float zoom);
void setMidZoom(float zoom);
void setMaxZoom(float zoom);
```

## Possible questions
### Why resulting apk is so big?
Android PdfViewer depends on PdfiumAndroid, which is set of native libraries (almost 16 MB) for many architectures.
Apk must contain all this libraries to run on every device available on market.
Fortunately, Google Play allows us to upload multiple apks, e.g. one per every architecture.
There is good article on automatically splitting your application into multiple apks,
available [here](http://ph0b.com/android-studio-gradle-and-ndk-integration/).
Most important section is _Improving multiple APKs creation and versionCode handling with APK Splits_, but whole article is worth reading.
You only need to do this in your application, no need for forking PdfiumAndroid or so.

## One more thing
If you have any suggestions on making this lib better, write me, create issue or write some code and send pull request.

## License

Created with the help of android-pdfview by [Joan Zapata](http://joanzapata.com/)
```
Copyright 2016 Bartosz Schiller

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
