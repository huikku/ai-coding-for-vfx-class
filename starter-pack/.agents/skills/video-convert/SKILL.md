---
name: video-convert
description: Convert video and image sequences with ffmpeg — review copies, ProRes for editorial, proxies, image sequence to MP4 and back, GIFs, audio extraction — including correct handling of HDR phone footage and linear EXRs. Use for any transcode, format change or "make this playable" request.
---

# Video conversion

All conversion runs locally with `ffmpeg`. The footage never leaves the
machine. Don't extract frames to look at unless the artist asks; use `ffprobe`
specs to check your work.

## 1. Inspect first (always)

```
ffprobe -v error -select_streams v:0 -show_entries stream=codec_name,width,height,r_frame_rate,pix_fmt,color_transfer,color_primaries,color_space -of default=nw=1 "INPUT"
```

**HDR check:** if `color_transfer` is `smpte2084` (PQ) or `arib-std-b67` (HLG),
or `color_primaries` is `bt2020`, the clip is **HDR**. iPhone and many Android
clips are. A plain conversion will look washed out and too bright. Use the
HDR recipe below.

## 2. Outputs go to a new folder

Write to `_converted/` (or a folder the artist names). Never overwrite the
source. Keep the original base name and add a suffix (`_review`, `_proxy`).

## Recipes

**Review copy (SDR source):**
```
ffmpeg -i "IN.mov" -c:v libx264 -crf 18 -preset slow -pix_fmt yuv420p -c:a aac -b:a 192k -movflags +faststart "_converted/IN_review.mp4"
```

**Review copy from HDR phone footage (tone-map to Rec.709):**
```
ffmpeg -i "IN.mov" -vf "zscale=t=linear:npl=100,format=gbrpf32le,zscale=p=bt709,tonemap=tonemap=hable:desat=0,zscale=t=bt709:m=bt709:r=tv,format=yuv420p" -c:v libx264 -crf 18 -preset slow -color_primaries bt709 -color_trc bt709 -colorspace bt709 -c:a aac -b:a 192k -movflags +faststart "_converted/IN_review.mp4"
```
Needs an ffmpeg build with `zscale` (libzimg). The standard Windows (gyan.dev),
Homebrew and most Linux builds have it. If `zscale` is missing, say so and
offer to install a full build.

**ProRes 422 HQ for editorial:**
```
ffmpeg -i "IN.mov" -c:v prores_ks -profile:v 3 -pix_fmt yuv422p10le -c:a pcm_s16le "_converted/IN_prores.mov"
```

**Proxy (half-res, small):**
```
ffmpeg -i "IN.mov" -vf "scale=iw/2:-2" -c:v libx264 -crf 23 -preset fast -pix_fmt yuv420p -c:a aac -b:a 128k "_converted/IN_proxy.mp4"
```

**Image sequence → MP4** (ask the frame rate if unknown; default 24):
```
ffmpeg -framerate 24 -start_number 1001 -i "shot.%04d.png" -c:v libx264 -crf 18 -pix_fmt yuv420p "_converted/shot.mp4"
```
**EXR sequences are linear.** Without a color transform they look dark and
contrasty. Quick sRGB preview: add `-apply_trc iec61966_2_1` **before** `-i`.
For show-accurate color (OCIO/ACES), say this is a preview only and offer an
OpenImageIO + OCIO route.

**MP4 → image sequence:**
```
ffmpeg -i "IN.mp4" -start_number 1001 "_converted/IN/IN.%04d.png"
```

**GIF for chat (small):**
```
ffmpeg -i "IN.mp4" -vf "fps=12,scale=640:-1:flags=lanczos,split[a][b];[a]palettegen[p];[b][p]paletteuse" "_converted/IN.gif"
```

**Pull the audio:**
```
ffmpeg -i "IN.mov" -vn -c:a pcm_s16le "_converted/IN.wav"
```

## Batch jobs

For more than a few files, write a small Python script in `scripts/` that loops
over the folder, runs ffprobe first on each clip (so HDR clips get the HDR
recipe automatically), skips files that already have an output, and prints a
summary at the end.

In shell loops, add `-nostdin` to every ffmpeg call, or ffmpeg swallows the
loop's input and silently skips files.

## Prove it

After converting, run `ffprobe` on each output and report codec, resolution,
frame rate, duration and color tags next to the source's. The durations should
match.

---
*Part of the AI Coding for VFX Artists starter pack, by John Huikku.*
