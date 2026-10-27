# RE/UGC plugin for Claude

RE/UGC is a library of real creator reaction clips: short vertical videos of
people reacting with shock, laughter, excitement and more. Marketers use them
as the opening hook of TikTok, Reels, Shorts and UGC-style ads. This plugin
connects Claude to your RE/UGC account so Claude can find the right reaction,
show you previews, download the originals you own and edit them into a
finished hook video.

## What's included

- **RE/UGC MCP server** (`https://reugc.com/mcp`): tools to list reaction
  categories, search clips, view clip details, check your account, get
  download links and unlock clips with your existing credits.
- **`reaction-hook-videos` skill**: a workflow for picking a reaction and
  cutting it together with your product footage, captions and a call to action.

## Requirements

A RE/UGC account with library access or credits. The first time Claude uses a
RE/UGC tool, you sign in to RE/UGC and approve access. Manage or disconnect it
anytime at https://reugc.com/dashboard/agents.

## Install

```
/plugin marketplace add <this repository>
/plugin install reugc@reugc
```

## Example prompts

- "Find three shocked reactions under 5 seconds and show me the previews."
- "Download the laughing clip I picked and add the caption 'POV: you found the
  cheapest flights' at the top."
- "Make a 9:16 hook video: reaction clip first, then my demo.mp4, then an end card."

## Privacy and support

Privacy policy: https://reugc.com/privacy · Terms: https://reugc.com/terms ·
Docs: https://reugc.com/agents · Support: support@reugc.com
