# Script Skill
### A Claude Code Skill by [@tenfoldmarc](https://www.instagram.com/tenfoldmarc)

Generate ready-to-record video scripts that sound like YOU — not like AI. Script Skill analyzes your real videos, studies your competitors' top performers, builds a hooks database, and writes scripts calibrated to your actual voice.

**Your scripts get better every time you use it. Your hooks database grows every week.**

---

## What It Does

1. Analyzes your Instagram videos to learn how you actually talk
2. Scrapes your competitors' top-performing videos for hooks and patterns
3. Builds a personal hooks database from competitor outliers
4. Generates scripts in YOUR voice with hooks proven to stop the scroll
5. Runs a De-AI checklist + humanizer pass so nothing sounds robotic
6. Optionally auto-refreshes your competitor data every 7 days

---

## What You'll Need

- A Mac or Linux computer
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed
- An [Apify](https://apify.com) account (free tier works — pay-per-scrape)
- An Instagram account with posted videos (so the skill can study your voice)
- 5–10 competitor Instagram handles in your niche

That's it. The skill installs everything else for you.

---

## Install

### Step 1 — Open your terminal

**Mac:** Press `Command + Space`, type `Terminal`, hit Enter.

### Step 2 — Run this command

```bash
git clone https://github.com/tenfoldmarc/script-skill ~/.claude/skills/script-skill
```

### Step 3 — Connect Apify

If you don't already have Apify connected to Claude Code:

1. Create a free account at [apify.com](https://apify.com)
2. Go to **Settings → Integrations** and copy your API token
3. Open `~/.claude.json` in any text editor and add this under `"mcpServers"`:

```json
{
  "apify": {
    "command": "npx",
    "args": ["-y", "@anthropic-ai/apify-mcp-server@latest"],
    "env": {
      "APIFY_TOKEN": "your_token_here"
    }
  }
}
```

If you already have other MCP servers in that file, just add the `"apify"` block alongside them.

### Step 4 — Restart Claude Code

Close Claude Code completely and reopen it.

### Step 5 — Run it

```
/script
```

The first time you run it, Script Skill walks you through the full setup (takes about 10–15 minutes). After that, every `/script` goes straight to writing.

---

## First-Time Setup

The setup runs automatically on your first `/script`. Here's what happens:

### 1. Tool Check
Script Skill checks for `yt-dlp`, `whisper`, and `ffmpeg`. If anything is missing, it installs it for you (with your permission).

### 2. Brand Voice Interview
Four quick questions: your Instagram handle, your niche, your ideal viewer, and your on-camera vibe.

### 3. Video Analysis
Downloads your 5 most recent videos plus any recent outliers (videos with 2x+ your average views). Transcribes them all and analyzes how you actually talk — your vocabulary, rhythm, energy, filler words, hook patterns, and content types.

### 4. Competitor Research
You give it 5–10 competitor handles (8–10 recommended). It pulls their last 25 videos each, finds the outliers, downloads and transcribes them. Every outlier hook gets saved to your database.

### 5. Hooks Database
All competitor outlier hooks are extracted, categorized by type, and saved to a searchable database. Full outlier transcripts are saved too.

### 6. Auto-Refresh (Optional)
Set up a cron job that automatically scrapes for new outlier videos every 7 days. Your hooks database grows on autopilot.

---

## After Setup — Writing Scripts

Every time you type `/script`, Claude:

1. Loads your voice profile, hooks database, transcripts, and competitor data
2. Asks what the video is about
3. Asks which content type (based on YOUR actual video patterns)
4. Generates 10 hook options using proven patterns from your database
5. You pick a hook
6. Writes the full script in your voice
7. Runs your custom De-AI checklist
8. Runs a humanizer pass
9. Delivers one clean, ready-to-record script

---

## Adding Hooks Manually

See a reel with a fire hook? Save it:

```
/hooks https://www.instagram.com/reel/XXXXX/
```

Claude downloads, transcribes, extracts the hook, and adds it to your database.

---

## Where Your Data Lives

```
~/Documents/script-skill/
├── config.json              ← your settings and competitor list
├── brand-voice.md           ← your voice profile
├── hooks-database.md        ← hook swipe file
├── scripts-database.md      ← full competitor outlier transcripts
├── competitor-analysis.md   ← what's working for competitors
├── my-transcripts/          ← your own video transcripts
├── competitor-transcripts/  ← competitor video transcripts
└── refresh-log.md           ← auto-refresh activity log
```

These files live on your machine. They never get overwritten by git updates.

---

## Managing the Auto-Refresh Cron Job

If you set up the optional cron job during setup:

**Check logs:**
```bash
node ~/perma-cron/manage.js logs script-skill-refresh
```

**Stop it:**
```bash
node ~/perma-cron/manage.js stop script-skill-refresh
```

**Remove it completely:**
```bash
node ~/perma-cron/manage.js remove script-skill-refresh
```

**Re-enable it:**
Ask Claude: "Set up the script-skill cron job"

---

## Troubleshooting

**"git: command not found"**
```bash
xcode-select --install
```
A popup will appear — click Install. Then try again.

**"yt-dlp: command not found" after install**
Try running the install with the full Python path:
```bash
python3 -m pip install yt-dlp
```
Then restart your terminal.

**"whisper fails to install"**
Whisper needs Python 3.8+ and about 1GB of disk space for the model. Try:
```bash
pip3 install git+https://github.com/openai/whisper.git
```

**"Apify scraper returns no results"**
Make sure the Instagram profile is public. Private profiles can't be scraped.

**"Videos won't download"**
Some reels are restricted. yt-dlp can't download every video. The skill skips failures and continues with what it can get.

**Claude doesn't recognize /script**
Make sure you fully closed and reopened Claude Code after installing.

**"I want to re-run setup"**
Delete `~/Documents/script-skill/config.json` and run `/script` again. It will start fresh.

**"I want to add more competitors later"**
Open `~/Documents/script-skill/config.json`, add handles to the `competitors` array, and ask Claude to "refresh my script-skill competitor data."

---

## How It Gets Better Over Time

- Every `/hooks [url]` adds to your database
- Every auto-refresh cycle finds new outliers
- The more hooks you collect, the better your hook options get
- Your voice profile can be refined — just ask Claude to "re-analyze my videos"

---

## Apify Costs

Script Skill uses Apify's pay-per-use pricing. Typical costs:

- Scraping one Instagram profile (25 posts): ~$0.05
- Full initial setup (your profile + 10 competitors): ~$0.60
- Weekly auto-refresh (11 profiles): ~$0.55/week

No subscriptions. You only pay for what you scrape.

---

Built by [@tenfoldmarc](https://www.instagram.com/tenfoldmarc) — follow for Claude Code automations and AI agency content.
