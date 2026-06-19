# Script Skill — short-video scripts in YOUR voice

Generate ready-to-record short-video scripts (Reels / TikTok / YouTube Shorts)
that sound like **you**, built on a **120-formula viral-hook library** and
calibrated to your real videos and your competitors' best hooks.

This is a merge of two skills:
- **Script Skill** — the voice-calibration + full-script pipeline
  (brand voice, competitor mining, De-AI checklist, humanizer).
- **viral-hooks** — a library of **120 formulas / 12 psycology triggers** and localized to **RU + EN**.

---

## What makes it complete

- **Real hook engine, not vibes.** 120 fill-in-the-bracket formulas across 12
  psychology triggers (curiosity, contrarian, authority, emotional, listicle,
  question, story, negation, specificity, confession, **urgency/FOMO**,
  **discovery/result-lead**) — each with an example and a platform tag.
- **Three hook layers.** Every script's hooks come from: the universal formula
  library → your competitors' proven outlier hooks → your own opening patterns.
- **Voice match.** Learns how you actually talk from your transcripts, then writes
  in that voice and runs a De-AI checklist + humanizer pass.
- **Bilingual.** Full RU and EN formula sets; pick the language per run (or auto).
- **Local-first.** Downloading (yt-dlp) and transcription
  ([transcribe-media-local](https://github.com/valerymad/transcribe-media-local))
  run on your machine. Apify is optional and only used to *find* which videos to grab.

---

## Progressive — works instantly, gets better as you feed it

| Tier | What you do | What you get | Needs |
|------|-------------|--------------|-------|
| **0 — instant** | nothing | 10 hooks from the 120-formula library + full script + De-AI + humanizer | — |
| **1 — manual** | paste **reel/TikTok/Shorts links** (yours + competitors') or local files | + your voice profile, + competitor hooks database | yt-dlp + transcribe-media-local |
| **2 — auto** | connect Apify | auto-discovery of competitor *outliers* + optional weekly refresh | Apify (paid, IG) |

The key Tier-1 insight: **send links one at a time** and the skill downloads +
transcribes each locally — no Apify required to build voice or competitor hooks.

---

## Commands

- **Run the skill** — the slash command matches the install folder name (e.g.
  `/script-short-skill`). Full flow: language → topic → format → **10 hook options
  across 12 categories** → you pick → full script → De-AI → humanizer → deliver.

Hooks are handled **inside the skill via natural language** — no separate command
(the word "hooks" is overloaded in Claude Code):
- *"give me 3 hooks for [topic]"* → 3 quick variants (stop-scroll + retention)
- *"save this hook: <URL>"* → download → transcribe → categorize → store in your DB

---

## Install

User-level (every Claude Code session):
```bash
git clone https://github.com/valerymad/script-short-skill.git ~/.claude/skills/script-short-skill
```
Project-level (just this project):
```bash
mkdir -p .claude/skills && git clone https://github.com/valerymad/script-short-skill.git .claude/skills/script-short-skill
```

### Dependencies
- **transcribe-media-local** skill (offered automatically on first `/script` if
  missing): `git clone https://github.com/valerymad/transcribe-media-local.git ~/.claude/skills/transcribe-media-local`
- **yt-dlp**, **ffmpeg**, **whisper** (`pip install yt-dlp openai-whisper`,
  `brew install ffmpeg`).
- **Apify MCP** — optional, only for Tier 2. Add an `apify` server to `~/.claude.json`.

First `/script` runs setup (language, brand-voice interview, optional voice +
competitor mining). After that, `/script` goes straight to writing.

---

## Files

| File | Role |
|------|------|
| `SKILL.md` | the `/script` skill (tiers, setup, generation, De-AI, humanizer) |
| `hooks-formulas-en.md` | 120 hook formulas, 12 triggers — **English** (static library) |
| `hooks-formulas-ru.md` | 120 hook formulas, 12 triggers — **Russian** (native adaptation) |
| `preview.html` | styled visual reference (EN, the original 100 — open in a browser) |
| `plan_short_video.md` | the build plan + implementation progress |

Your personal data lives separately in `~/Documents/script-short-skill/`
(`config.json`, `brand-voice.md`, `hooks-database.md` ← your swipe file,
`my-transcripts/`, `competitor-transcripts/`, `competitor-analysis.md`). Note:
`hooks-database.md` (your saved competitor hooks) is **different** from the static
`hooks-formulas-*.md` (the universal templates).

---

## Credits

- Local transcription: [transcribe-media-local](https://github.com/valerymad/transcribe-media-local) by [@valerymad](https://github.com/valerymad).
