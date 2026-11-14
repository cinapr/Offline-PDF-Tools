# Offline PDF Tools

Two single-file, browser-based PDF tools. No server, no upload, no internet connection needed. Every library is inlined, so each tool is one `.html` file you open by double-clicking.

| Tool | File | Purpose |
|------|------|---------|
| PDF Merger | `pdf-merger-offline.html` | Merge PDFs with reordering, page omission, cover page, multi-page TOC and nested bookmarks |
| PDF Compressor | `pdf-compressor-offline.html` | Reduce PDF size with four compression levels |

Built with [pdf-lib](https://github.com/Hopding/pdf-lib) and [pdf.js](https://github.com/mozilla/pdf.js). Files never leave your machine.

## Quick start

1. Download the `.html` file(s).
2. Open in Chrome, Edge, Firefox or Safari.
3. Drop your PDFs in.

## PDF Merger

### Features

- Merge any number of PDFs into one file.
- **Reorder files** by dragging the ☰ handle.
- **Reorder pages** inside a file: click **Pages** and drag thumbnails.
- **Omit pages**: click a thumbnail, or type ranges such as `2,5-7` in the omit box.
- **Optional cover page**: upload a 1-page PDF. It becomes page 1.
- **Optional table of contents**:
  - Spans as many pages as needed.
  - Long names wrap instead of being cut off.
  - Dot leaders and right-aligned page numbers.
  - Every entry is a clickable link.
  - Order: cover, then TOC, then content.
- **Nested bookmarks**: the filename becomes H1 and each file's own bookmarks drop one level (H1→H2, H2→H3, and so on).
- **Custom TOC from a TXT file** (see below).
- Smaller output through object-stream compression and removal of unused objects.

### TXT structure

Upload a `.txt` file to define the TOC yourself:

```
[LEVEL] NAME | PAGE
```

```
[1] NAME1 | 1
[2] NAME1.1 | 2
[3] NAME1.1.1 | 3
[2] NAME1.2 | 4
[3] NAME1.2.1 | 3
[3] NAME1.2.2 | 13
[1] NAME2 | 7
```

Renders as:

```
1. NAME1.................................1
    1.1. NAME1.1.........................2
           1.1.1. NAME1.1.1..............3
    1.2. NAME1.2.........................4
           1.2.1. NAME1.2.1..............3
           1.2.2. NAME1.2.2.............13
2. NAME2.................................7
```

**Validation:** before merging, the number of entries and their level sequence must exactly match the merged files (filename = level 1, file bookmarks = level 2 and deeper). If they do not match, the tool shows an error and downloads `existing-structure.txt` in the same format, so you can edit it and upload it again.

If no TXT is given, the TOC is generated from the filenames and each file's bookmarks.

### Notes

- TOC text uses the built-in Helvetica font. Characters outside Latin-1 (for example CJK) appear as `?` in the TOC. Bookmarks keep full Unicode.
- Bookmarks that use named destinations are resolved when possible; otherwise they fall back to their parent's page.
- Password-protected PDFs may fail to load.

## PDF Compressor

Shrinks PDFs while keeping everything else intact: text stays selectable, and links, annotations, forms and bookmarks are unchanged. It works by re-encoding the images inside the PDF (JPEG and uncompressed/Flate RGB or grayscale images) at lower resolution and quality, dropping page thumbnails, and rewriting the file with compressed object streams.

### Options (choose one)

| Option | Behaviour |
|--------|-----------|
| **Quality** (dropdown) | Low compression (best quality), Recommended, High compression, Extreme compression (lowest quality) |
| **Max size** (KB) | Finds the **highest quality** that produces a file at or below the limit, applied to each file separately |

Max size works by trying a ladder of 12 quality/resolution steps (JPEG quality 90% to 10%, longest image side 3200 px to 350 px) and binary-searching for the mildest step that fits. If the file is already under the limit it is left alone. If even the strongest step is too big, the tool gives you the smallest possible result and tells you the target was not reached.

Each file shows its before and after size. If compression gives no gain, the original is kept.

### Notes

- Files that are mostly text, fonts or vector graphics have little to shrink. A tight Max size may not be reachable for them without removing content, which this tool never does.
- Indexed-colour, CMYK, masked images and images with TIFF predictors are skipped.
- Fonts are not re-subset and JPEG2000 or JBIG2 images are not re-encoded.

## Browser support

Any modern browser with ES2020 support. Tested approach: Chromium-based browsers first; Firefox and Safari should work, but large files may be slower.

## Privacy

All processing happens locally in your browser tab. Nothing is sent over the network.

## Third-party licenses

- pdf-lib: MIT
- pdf.js: Apache-2.0

The inlined library code retains its original license headers.
