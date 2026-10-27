---
name: reaction-hook-videos
description: Find a RE/UGC creator reaction clip and edit it into a short vertical hook video (TikTok, Reels, Shorts, UGC ads) with captions, product footage and a call to action. Use when the user wants a UGC-style hook, a reaction intro, or to combine a RE/UGC clip with their own footage.
---

# Reaction hook videos with RE/UGC

RE/UGC clips are real creators reacting on camera (shocked, laughing, excited,
confused and so on). They work best as the first 1-3 seconds of a vertical ad:
the reaction stops the scroll, a caption sets up the joke or problem, and the
product footage pays it off.

## 1. Pick the reaction

1. Read the brief: product, audience, platform, the feeling the hook should
   create, and the caption or "POV" line if the user has one.
2. Call `list_categories` once to learn the real category and creator names.
3. Call `search_clips` with a `category` and, if useful, `max_duration_seconds`
   (hooks are usually 2-6 seconds). Offer 3-5 options with their title,
   creator, duration and preview link so the user can choose.
4. Call `get_account` to see whether the chosen clip is owned. If
   `access` is `unlock_required`, tell the user it costs 1 credit from their
   balance and wait for a yes before calling `unlock_clip`.

## 2. Download the original

Call `get_download_link` and download the file straight away; the link expires
after 5 minutes. Never paste the link into shared documents.

```bash
curl -L --fail -o reaction.mp4 "<downloadUrl>"
```

## 3. Edit (ffmpeg)

Check that `ffmpeg` is installed (`ffmpeg -version`). Normalise every input to
1080x1920, 30 fps, before joining:

```bash
norm='scale=1080:1920:force_original_aspect_ratio=decrease,pad=1080:1920:(ow-iw)/2:(oh-ih)/2,fps=30,format=yuv420p'
ffmpeg -y -i reaction.mp4 -vf "$norm" -c:v libx264 -crf 18 -c:a aac -ar 48000 -ac 2 hook.mp4
ffmpeg -y -i product.mp4  -vf "$norm" -c:v libx264 -crf 18 -c:a aac -ar 48000 -ac 2 body.mp4
```

Add the hook caption in the top safe area (keep text inside the middle 80% of
the width and clear of the top 220 px and bottom 420 px, where platform UI sits):

```bash
ffmpeg -y -i hook.mp4 -vf "drawtext=text='POV\: you finally found it':fontcolor=white:fontsize=64:borderw=6:bordercolor=black:x=(w-text_w)/2:y=260" -c:a copy hook-captioned.mp4
```

On macOS add `:fontfile=/System/Library/Fonts/Supplemental/Arial Bold.ttf` to
`drawtext` if ffmpeg cannot find a default font. Escape `:` and `'` in caption text.

Join hook and product footage:

```bash
printf "file 'hook-captioned.mp4'\nfile 'body.mp4'\n" > parts.txt
ffmpeg -y -f concat -safe 0 -i parts.txt -c copy final.mp4
```

If a clip has no audio track, add silence first
(`-f lavfi -i anullsrc=r=48000:cl=stereo -shortest`) so the concat streams match.

## 4. Deliver

Report the output path, length and resolution (`ffprobe -v error -show_entries
format=duration:stream=width,height -of csv=p=0 final.mp4`). Suggest one or two
alternative reactions for A/B testing the hook.

## Licensing

Under the RE/UGC Terms (https://reugc.com/terms), clips the account owns can be
used in personal and commercial content, including paid ads and client work.
Don't resell or redistribute the raw clips as a standalone library, and don't
imply endorsement by RE/UGC or the person shown without their permission.
