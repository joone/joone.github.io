---
title: "Fixing Drag-and-Drop in Chromium: What Was Broken and What Changed"
date: 2026-04-20
description: ""
tags: "drag-and-drop, chromium, BlinkOn"
---

I gave a lightning talk at **BlinkOn 21** on fixing drag-and-drop in Chromium.
Drag-and-drop is one of the oldest and most intuitive interactions in graphical
user interfaces, yet several long-standing gaps in Chromium made it unreliable
when data crossed the boundary between the browser and other applications. This
post summarizes the three problems I worked on and how each was fixed.

- **Slides:** [Fixing Drag-and-Drop in Chromium: What Was Broken and What Changed](https://docs.google.com/presentation/d/11enYYyB0JvbfcQti3CGdcB_D__MjB5dm1CH7mbfVO34/edit?usp=sharing)
- **Demo videos:** [X thread](https://x.com/joone/status/2048919863134212592)

## Drag-and-Drop Issues in Chromium

The talk covered three distinct issues with the `DataTransfer` API:

- **`text/uri-list` drag type** — multiple URLs were concatenated into a single,
  invalid URL.
- **`DownloadURL` drag type** — only a single URL was supported.
- **JavaScript `File` object** — dragging a constructed `File` out of the browser
  was not implemented, even though Firefox supports it.

## 1. `text/uri-list`: Multiple URLs Were Concatenated

When a web page calls `setData('text/uri-list', uriList)` with multiple URLs,
Chromium's internal data handling stripped the CRLF delimiters. As a result, when
the data was read back via `getData('text/uri-list')` during a `drop` event, the
result was a single, concatenated, and invalid URL string.

For example, this correct input:

```text
https://mozilla.org\r\nhttps://webkit.org\r\nhttps://chromium.org
```

was turned into this malformed URL:

```text
https://mozilla.orghttps://webkit.orghttps://chromium.org
```

In contrast, Firefox returns the correct, original string. This bug broke both
internal drops (for example, to another Chromium window) and external drops (for
example, onto the desktop or another application), because the receiving target
could not parse the malformed URL. **This issue has been fixed since M145.**

### Implementation Details

- **Issue:** [text/uri-list: Dragging multiple URIs results in a single concatenated URL (41011768)](https://issues.chromium.org/issues/41011768)
- **Change lists:**
  - [Allow setting multiple URLs for text/uri-list drag type (6608042)](https://chromium-review.googlesource.com/c/chromium/src/+/6608042)
  - [Introduce ui::ClipboardUrlInfo for multiple URL drag and drop (7059146)](https://chromium-review.googlesource.com/c/chromium/src/+/7059146)
  - [Refactor OSExchangeData to use ui::ClipboardUrlInfo (7084017)](https://chromium-review.googlesource.com/c/chromium/src/+/7084017)
  - [Refactor OSExchangeData to consolidate URL retrieval into GetURLs() (7499503)](https://chromium-review.googlesource.com/c/chromium/src/+/7499503)

## 2. `DownloadURL`: From a Single URL to a List

The `DownloadURL` drag type was introduced in Chromium in 2009 to support
downloading a URL to the local desktop via drag-and-drop. The idea goes back to a
WHATWG proposal, [*Proposal to drag virtual file out of browser*](https://lists.w3.org/Archives/Public/public-whatwg-archive/2009Aug/0388.html):
if a draggable element contains a URL, dragging it out of the browser normally
copies only the URL value, but in many scenarios we actually want to download the
file the URL points to. `DownloadURL` makes that possible, and it has long been
used by apps such as Outlook and Gmail to download mail attachments to the user's
desktop.

The limitation was that `DownloadURL` only supports a **single** URL:

```js
e.dataTransfer.setData(
  "DownloadURL",
  "image/png:myImage.png:https://example.com/path/to/image.png"
);
```

To support multiple downloads in one drag, I introduced a new drag type,
`DownloadURL-list`, whose payload is a JSON array that can hold information for
multiple files:

```js
const data = [
  {
    type: 'image/png',
    name: 'file2.png',
    url: 'http://example.com/file2.png'
  },
  {
    type: 'image/jpeg',
    name: 'file1.jpg',
    url: 'http://example.com/file1.jpg'
  }
];

event.dataTransfer.setData('DownloadURL-list', JSON.stringify(data));
```

### Implementation Details

- **Issue:** [Multi files drag out to filesystem via DownloadURL (40736398)](https://issues.chromium.org/issues/40736398)
- **Design:** Enabling Multi-File Drag-and-Drop in Chromium
- **Explainer:** Drag Multiple Virtual Files Out of Browser
- **Chrome Platform Status:** DownloadURL-list: A drag-and-drop type for multi-file downloads
- **Change list:** [Support dragging multiple files via DownloadURL drag type (6625735)](https://chromium-review.googlesource.com/c/chromium/src/+/6625735)

## 3. JavaScript `File` Object: Delivering the Bytes

Theoretically, a JavaScript `File` object can be attached to the `DataTransfer`
API at `dragstart` and read back at `drop`:

```js
source.addEventListener('dragstart', (e) => {
  // Create a File object with text content.
  const file = new File(['Hello from a JS File!'], 'hello.txt', {
    type: 'text/plain'
  });

  // Add the File to the drag data.
  e.dataTransfer.items.add(file);
  e.dataTransfer.effectAllowed = 'copy';
});

dropZone.addEventListener('drop', async (e) => {
  e.preventDefault();
  const files = e.dataTransfer.files;

  for (const file of files) {
    const text = await file.text();
    result.textContent += `${file.name} ${file.type} ${file.size} ${text}`;
  }
});
```

In practice, Chromium only dropped the file *name* as a long-missing fallback in
`third_party/blink/renderer/core/clipboard/data_object.cc`; the actual bytes were
discarded. Firefox, by contrast, already supports dragging and dropping
JavaScript-created `File` objects out of the browser, with no file-type
limitations.

I implemented the missing path for Windows, Linux, and macOS so that the bytes of
a constructed `File` are delivered to the OS drop target. The initial change
lists support image types only; support for other file types will be added after
they land. Once merged, this lets users drag a `File` out of Chromium and drop it
even onto Firefox.

For more details on the design, see my earlier post,
[Design Doc: Support Dragging JS File Objects to Native Drop Targets](/posts/design-doc-support-dragging-js-file-objects-to-native-drop-targets/).

### Implementation Details

- **Issues:**
  - [Support dragging constructed Files across renderers (41120809)](https://issues.chromium.org/issues/41120809)
  - [HTML 5 Drag n Drop for Mac OS bugs out (40183464)](https://issues.chromium.org/issues/40183464)
- **Change lists:**
  - [Win: Support TYMED_ISTREAM for CFSTR_FILECONTENTS (7566722)](https://chromium-review.googlesource.com/c/chromium/src/+/7566722)
  - [Support dragging JS File objects to native drop targets (7603160)](https://chromium-review.googlesource.com/c/chromium/src/+/7603160)
  - [mac: Support dragging JS File objects across webviews and apps (7689255)](https://chromium-review.googlesource.com/c/chromium/src/+/7689255)
- **Live demo:** <https://joone.github.io/web/dnd/JS_file_object/>

## Demos

Demo videos for all three scenarios are available in the
[X thread for this talk](https://x.com/joone/status/2048919863134212592).

## References

- [Working with the drag data store — Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API)
- [Slides: Fixing Drag-and-Drop in Chromium](https://docs.google.com/presentation/d/11enYYyB0JvbfcQti3CGdcB_D__MjB5dm1CH7mbfVO34/edit?usp=sharing)
