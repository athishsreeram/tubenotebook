# 📺 TubeNotebook

> **YouTube Channel Intelligence — No API Key. No Setup. Just Run.**

Extract video titles, URLs, metadata, engagement scores, and full transcripts from any YouTube channel. One notebook, all in Google Colab.

-----

## What It Does

|Feature           |Details                                                                 |
|------------------|------------------------------------------------------------------------|
|🎯 Channel scan    |Get all video titles + URLs from any `@handle` or channel ID            |
|📊 Rich metadata   |Views, likes, comments, duration, tags, description                     |
|💡 Engagement score|`(likes + comments) / views` — find your highest-impact videos          |
|📝 Transcripts     |Full text transcript for any video (auto-fallback if no manual captions)|
|💾 Exports         |CSV (open in Excel/Sheets) + JSON (for devs / AI pipelines)             |

-----

## Notebooks

|File                                                                                                                                                                      |What it does                                 |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|
|[`TubeNotebook.ipynb`](./TubeNotebook.ipynb)                                                                                                                              |⭐ **Main notebook** — everything in one place|
|[`YoutubeVideoUrl.ipynb`](./YoutubeVideoUrl.ipynb)                                                                                                                        |Legacy — channel URL listing only            |
|[`Transcript_For_Video.ipynb`](./Transcript_For_Video.ipynb)                                                                                                              |Legacy — single video transcript             |
|[`top3videos_metadata_transcript_preview_and_saves_everything_to_CSV_and_JSON.ipynb`](./top3videos_metadata_transcript_preview_and_saves_everything_to_CSV_and_JSON.ipynb)|Legacy — top 3 video deep fetch              |


> 💡 Use **`TubeNotebook.ipynb`** — it replaces all three legacy notebooks.

-----

## Quick Start

### 1. Open in Google Colab

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/athishsreeram/tubenotebook/blob/main/TubeNotebook.ipynb)

Or go to [colab.research.google.com](https://colab.research.google.com/) → File → Open notebook → GitHub → paste this repo URL.

### 2. Install dependencies (Step 1 cell)

```python
!pip install yt-dlp youtube-transcript-api
```

### 3. Set your channel (Step 2 cell)

```python
CHANNEL_URL      = "https://www.youtube.com/@rajshamani/videos"
MAX_VIDEOS       = 50      # None = all videos
DEEP_FETCH_TOP_N = 5       # how many to analyze deeply
FETCH_TRANSCRIPTS = True   # set False for faster runs
TRANSCRIPT_LANG  = "en"    # "hi", "ta", "es", etc.
```

### 4. Run all cells → Download your files

Go to **Runtime → Run all**, then find your files in the **left sidebar → Files tab**.

-----

## Output Files

After running, you’ll get:

```
tube_videos_YYYYMMDD_HHMMSS.csv     ← All videos: title, URL, date, duration
tube_rich_YYYYMMDD_HHMMSS.csv       ← Top N: views, likes, comments, engagement%
tube_full_YYYYMMDD_HHMMSS.json      ← Full data + transcripts (for AI/dev use)
transcript_<video_id>.txt           ← Single-video transcript (Step 8)
```

-----

## What Is Engagement Score?

```
engagement_score = (likes + comments) / views × 100
```

A video with 1M views but 500K likes + comments (50%) is far more impactful than one with 5M views and 10K interactions (0.2%). Use this to find your channel’s real winners.

-----

## Use Cases

- **Content creators** — Analyze what’s working on competitor channels
- **Researchers** — Build transcript datasets for NLP / AI training
- **Marketers** — Find high-engagement topics in a niche
- **AI pipelines** — Feed structured YouTube data into LLM workflows
- **Personal use** — Archive a channel’s content index

-----

## Troubleshooting

|Problem                                       |Fix                                                                           |
|----------------------------------------------|------------------------------------------------------------------------------|
|`ModuleNotFoundError`                         |Run the install cell again: `!pip install yt-dlp youtube-transcript-api`      |
|No videos found                               |Check the channel URL — make sure it ends in `/videos` or is a valid `@handle`|
|Transcript returns `[No transcript available]`|The video has no captions. Try `TRANSCRIPT_LANG = "hi"` or another language   |
|Slow on large channels                        |Reduce `MAX_VIDEOS = 50` for faster scans                                     |
|Rate limited                                  |Add `time.sleep(1)` between calls or reduce `DEEP_FETCH_TOP_N`                |

-----

## Tech Stack

|Tool                                                                         |Purpose                                     |
|-----------------------------------------------------------------------------|--------------------------------------------|
|[`yt-dlp`](https://github.com/yt-dlp/yt-dlp)                                 |Channel + video metadata (no API key needed)|
|[`youtube-transcript-api`](https://github.com/jdepoix/youtube-transcript-api)|Transcript extraction                       |
|Python stdlib (`csv`, `json`, `datetime`)                                    |Export + formatting                         |

-----

## Local Usage (Outside Colab)

```bash
pip install yt-dlp youtube-transcript-api jupyter
jupyter notebook TubeNotebook.ipynb
```

-----

## Author

Built by **[@athishsreeram](https://github.com/athishsreeram)**

-----

*No YouTube Data API key required. Uses public metadata only.*