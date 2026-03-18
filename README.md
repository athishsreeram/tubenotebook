# tubenotebook

# 📓 TubeNotebook

> Feed any YouTube video or channel into NotebookLM in seconds.

-----

## What This Does

NotebookLM accepts YouTube URLs as sources — it transcribes, indexes, and lets you chat with any video. **TubeNotebook** removes all friction: one click (extension) or one command (CLI) gets you from YouTube → NotebookLM.

-----

## 🧩 Chrome Extension — Setup

### Install (Developer Mode, ~2 min)

1. Open Chrome → go to `chrome://extensions/`
1. Toggle **Developer mode** ON (top right)
1. Click **“Load unpacked”**
1. Select the `tubenotebook-extension/` folder
1. The 📓 icon appears in your toolbar

### How to Use

**On a video page:**

- Click the extension icon → see video info → hit **“Open in NotebookLM”**
- OR look for the floating **“📓 Add to NotebookLM”** button (bottom-right of the page)
- URL is auto-copied; paste it into NotebookLM as a new source

**On a channel page (`/@aidotengineer`, `/channel/...`):**

- Click the extension icon → channel videos load automatically
- Check the ones you want (or “Select all”)
- Hit **“Send Selected to NotebookLM”** → opens NotebookLM + copies URLs
- Paste each URL as a source in NotebookLM

-----

## ⌨️ CLI Tool — Setup

### Requirements

- Node.js 18+ (has built-in `fetch`)
- No npm install needed

### Usage

```bash
# Single video
node tubenotebook.mjs https://youtube.com/watch?v=VIDEO_ID

# Channel by @handle — get last 10 videos
node tubenotebook.mjs https://youtube.com/@aidotengineer

# Channel with options
node tubenotebook.mjs @fireship --limit 20 --copy --open

# Short video URL
node tubenotebook.mjs https://youtu.be/dQw4w9WgXcQ --open
```

### Flags

|Flag       |Short|Description                                   |
|-----------|-----|----------------------------------------------|
|`--limit N`|`-l` |Max videos to fetch from channel (default: 10)|
|`--copy`   |`-c` |Copy all URLs to clipboard                    |
|`--open`   |`-o` |Open NotebookLM in browser                    |
|`--help`   |`-h` |Show help                                     |

### Example Output

```
📓 TubeNotebook

Channel: AI Dot Engineer
Found:   10 videos

──────────────────────────────────────────────────────────────────────
[ 1] Build an AI Agent from Scratch               3 days ago
     https://www.youtube.com/watch?v=abc123
[ 2] RAG vs Fine-Tuning: When to Use Each         1 week ago
     https://www.youtube.com/watch?v=def456
...
──────────────────────────────────────────────────────────────────────

NotebookLM Import Steps:
  1. Go to https://notebooklm.google.com/
  2. Click "+ New Notebook"
  3. For each URL above → "Add source" → YouTube → paste URL

  ✓ 10 URLs copied to clipboard!
  ✓ NotebookLM opened in browser
```

-----

## 🔑 NotebookLM Import Steps (Manual)

1. Go to **https://notebooklm.google.com/**
1. Click **”+ New Notebook”**
1. Click **“Add source”**
1. Choose **YouTube** from the source types
1. Paste the video URL → click **Add**
1. Repeat for more videos (up to 50 sources per notebook)
1. Once indexed, use the chat to ask questions, get summaries, generate study guides, etc.

-----

## 🛠️ Master Prompt (Build This Yourself)

Use this prompt with Claude or any LLM to rebuild or extend TubeNotebook:

-----

```
You are an expert Chrome Extension developer and Node.js engineer.

Build a tool called "TubeNotebook" that helps users feed YouTube videos into Google NotebookLM (https://notebooklm.google.com), which accepts YouTube URLs as sources.

## How NotebookLM ingestion works
NotebookLM does NOT have an API. Users must:
1. Open https://notebooklm.google.com
2. Create a new notebook
3. Click "Add source" → choose YouTube → paste a clean URL (https://www.youtube.com/watch?v=VIDEO_ID)
So the tool's job is: extract clean YouTube URLs + open NotebookLM + minimize copy-paste friction.

## Deliverable 1: Chrome Extension (Manifest V3)

Files needed: manifest.json, popup.html, popup.js, content.js

### Popup behavior:
- On a YouTube watch page (/watch?v=...):
  - Show video thumbnail (use https://img.youtube.com/vi/VIDEO_ID/mqdefault.jpg)
  - Show title and channel name
  - Show clean URL (strip all params except ?v=)
  - Button: "Open in NotebookLM" → opens notebooklm.google.com in new tab + copies URL to clipboard
  - Button: "Copy URL"

- On a YouTube channel page (/@handle, /channel/, /user/):
  - Scrape visible video elements from the DOM using ytd-rich-item-renderer
  - Show a scrollable list with checkboxes
  - "Select all" toggle
  - Button: "Send Selected to NotebookLM" → opens NotebookLM + copies selected URLs (one per line)
  - Button: "Copy Selected URLs"

- On any other page:
  - Show "Navigate to a YouTube video or channel to get started"

### Content script behavior:
- Inject a floating "📓 Add to NotebookLM" button on /watch pages (bottom-right)
- On click: copy clean URL to clipboard + open notebooklm.google.com
- Handle YouTube SPA navigation via yt-navigate-finish event

### Permissions: activeTab, scripting, clipboardWrite, storage
### Host permissions: https://www.youtube.com/*, https://notebooklm.google.com/*

## Deliverable 2: Node.js CLI (single file, tubenotebook.mjs)

### Requirements:
- Node 18+ only (use built-in fetch, no npm installs)
- Single .mjs file

### Commands:
  node tubenotebook.mjs <url-or-@handle> [--limit N] [--copy] [--open]

### Input types to handle:
- Single video URL (youtube.com/watch?v=, youtu.be/)
- Channel @handle (@fireship, @aidotengineer)
- Channel URL (youtube.com/@handle, youtube.com/channel/ID)

### For single videos:
- Fetch the page, extract title from <title> tag
- Print clean URL
- Print step-by-step NotebookLM import instructions

### For channels:
- Fetch /<@handle>/videos page
- Extract ytInitialData JSON from the page HTML (look for `var ytInitialData = {...};`)
- Walk the JSON tree to find objects with both videoId and title fields
- Print a numbered table: [index] title  meta  \n  URL
- If --copy: copy all URLs to clipboard (use pbcopy on mac, xclip on linux, clip on windows)
- If --open: open https://notebooklm.google.com in default browser

### Styling: Use ANSI escape codes for colored terminal output (purple, cyan, green, yellow, dim)

## Constraints:
- No external npm dependencies
- Must work without a YouTube API key (scrape public pages)
- Clean URLs only: https://www.youtube.com/watch?v=VIDEO_ID (no extra params)
- Always add a --help flag with usage examples

## Design principles:
- Minimal friction: the fewer steps between "I want this video" and "it's in NotebookLM" the better
- Copy + open in one action wherever possible
- Show exactly what steps the user needs to take in NotebookLM (it has no API)
```

-----

## Limitations & Notes

- **No NotebookLM API exists** — ingestion is always manual (paste URL into the UI). This tool minimizes that friction.
- **Channel video scraping** relies on YouTube’s public page HTML — may break if YouTube changes their DOM structure.
- **YouTube API key alternative**: For production, use the [YouTube Data API v3](https://developers.google.com/youtube/v3) with a free key for more reliable channel video fetching.
- NotebookLM supports up to **50 sources per notebook**.
- NotebookLM works best with videos that have auto-generated or manual captions.