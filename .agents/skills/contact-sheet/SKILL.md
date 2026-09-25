---
name: contact-sheet
description: Make contact sheets — a grid of thumbnails from one clip, from every clip in a folder, or from a folder of images or renders — labelled with file names. Use for "contact sheet", "thumbnail grid", "overview of this folder", "one image showing all the shots".
---

# Contact sheets

Built locally with ffmpeg and Python. Making the sheet does **not** upload
anything. Only look at the finished sheet yourself if the artist asks you to.

## One clip → grid of frames

```
ffmpeg -i "IN.mp4" -vf "fps=1/5,scale=480:-2,tile=5x4:padding=8:margin=8" -frames:v 1 "_converted/IN_sheet.jpg"
```
`fps=1/5` = one frame every 5 seconds. Adjust so 20 frames span the clip:
`fps=20/<duration in seconds>`.

## Folder of clips → one sheet, one thumbnail per clip

Write `scripts/contact_sheet.py` (run with `uv run --with pillow scripts/contact_sheet.py FOLDER`):

1. For each clip, read its duration with ffprobe and grab the **middle** frame:
   `ffmpeg -ss <duration/2> -i CLIP -frames:v 1 -vf scale=480:-2 thumb.jpg`.
   If the clip is HDR (see the `video-convert` skill), add the tone-map filter
   so thumbnails aren't washed out.
2. Lay the thumbnails out in a grid with Pillow (default 5 columns), with the
   file name under each one and the folder name plus date as a header.
3. Save as `_converted/<folder>_contact_sheet.jpg` and delete the temporary
   thumbnails (they're temporary files, not the artist's).

## Folder of images or renders

Same as above but read the images directly. For EXRs, convert to sRGB for the
thumbnails (OpenImageIO: `oiiotool IN.exr --colorconvert linear sRGB -o thumb.jpg`)
and note it's a preview transform, not show color.

## Prove it

Report the sheet's path, pixel size and how many items it contains, and list
any files that were skipped and why.

---
*Part of the AI Coding for VFX Artists starter pack, by John Huikku.*
