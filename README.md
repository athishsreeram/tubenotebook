# 🚀 YouTube Channel Data Extractor (Simple Guide)

## 👋 What is this?

This is a simple tool that lets you:

👉 Enter a YouTube channel
👉 Run one script
👉 Get all video data (titles, views, tags, etc.)

No complex setup. Works in your browser using Google Colab.

---

## ⚡ What you get

For each video:

* Title
* URL
* Views
* Likes & comments
* Duration
* Tags (keywords)
* Engagement score

👉 Output files:

* CSV (open in Excel)
* JSON (for developers)

---

## 🧑‍💻 How to use (Step-by-step)

### 1. Open Google Colab

Go to: https://colab.research.google.com/

---

### 2. Sign in

* Use your Google account (Gmail)
* Click **“New Notebook”**

---

### 3. Paste the code

* Copy the Python code from this repo
* Paste it into the notebook

---

### 4. Run the code

* Click ▶️ Run
* Wait ~30–60 seconds

---

### 5. Download results

* On the left sidebar → click **Files**
* Download:

  * `youtube_data_*.csv`
  * `youtube_data_*.json`

---

## ⚙️ Change the channel

Edit this line in the code:

```python
CHANNEL_URL = "https://www.youtube.com/@rajshamani/videos"
```

👉 Replace with any YouTube channel you want

---

## ⚙️ Optional settings

```python
MAX_VIDEOS = 100        # how many videos to scan
DEEP_FETCH_TOP_N = 10   # how many videos to analyze deeply
```

---

## 🧠 What is engagement score?

```python
engagement_score = (likes + comments) / views
```

👉 Helps you find videos that people actually interact with

---

## 💡 What can you use this for?

* Analyze any creator’s content
* Find popular topics
* Build content ideas
* Feed data into AI tools

---

## ❗ Common issues

* Not working? → Run this first:

```python
!pip install yt-dlp
```

* Too slow? → Reduce:

```python
MAX_VIDEOS = 50
```

---

## ✅ That’s it

👉 Copy → Paste → Run → Download

You now have a **YouTube data engine**.

---
