---
name: hooks
description: "Save viral hooks from Instagram reels to your Script Skill database. Trigger when you type /hooks followed by a URL. Downloads the reel, transcribes it, extracts the hook, and saves it for future script inspiration."
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
---

# /hooks — Add a Hook to Your Database

## Purpose

Save high-performing hooks from Instagram reels to your personal hooks database. Every hook you save makes your `/script` output better — Claude reads the full database for inspiration when writing new scripts.

## Trigger

User types `/hooks [instagram_reel_url]`

Example: `/hooks https://www.instagram.com/reel/DVh5BtIiS_L/`

---

## Step 0 — Find Config

Read `~/Documents/script-skill/config.json` to get:
- `dataDir` — where all Script Skill data lives
- `toolPaths.ytDlp` — path to yt-dlp
- `toolPaths.whisper` — path to whisper

If config doesn't exist, tell the user:
```
You need to run /script first to set up Script Skill. That will install your tools and create your database.
```
Stop here.

---

## Step 1 — Download the reel

```bash
[ytDlpPath] "[URL]" -o /tmp/hook_reel.mp4 --merge-output-format mp4 -q
```

If download fails (private reel, deleted, geo-restricted, etc.) — tell the user and stop.

---

## Step 2 — Transcribe

```bash
[whisperPath] /tmp/hook_reel.mp4 --model base --output_format txt --output_dir /tmp/ --fp16 False
```

Read the output from `/tmp/hook_reel.txt`.

---

## Step 3 — Extract the hook

The hook is the **opening 1–3 sentences** — everything said in roughly the first 5–10 seconds. This is what stops the scroll. Extract it precisely from the transcript.

Then identify:

- **Hook type** — pick the closest match:
  - `Bold Claim` — makes a strong, surprising statement
  - `Curiosity Gap` — withholds info to create pull ("here's what nobody tells you")
  - `Contrarian` — challenges a widely held belief
  - `Tool Discovery` — "X is insane / I can't live without X"
  - `Result Lead` — opens with an outcome or number
  - `Pain Agitation` — names a frustration the viewer has
  - `Identity Call-out` — speaks directly to a specific person
  - `Raw Energy` — pure excitement, just hit record
  - `Cost Savings` — specific before/after with dollar amounts
  - `Forbidden/Secret` — implies exclusive access
  - `Replace + Proof` — names the thing being replaced
  - `Urgency/FOMO` — a window is closing
  - `Viewer Callout` — directly challenges the viewer
  - `Absurd Escalation` — exaggerates for impact

- **Why it works** — 1 sentence explanation of the psychological mechanism

---

## Step 4 — Save to database

Read `{dataDir}/hooks-database.md`. Count existing hooks to determine the next hook number.

Append the new entry:

```markdown
---

## Hook #[N]
**Source:** [full URL]
**Creator:** @[extract handle from URL]
**Date:** [today's date, YYYY-MM-DD]
**Type:** [hook type]

**Hook:**
> "[Exact opening lines from transcript]"

**Why it works:** [1-sentence explanation]

**Full opening (first ~10 seconds):**
"[First 3–5 sentences of transcript for more context]"
```

If the hooks database file doesn't exist yet, create it with this header first:

```markdown
# Hooks Database
> Personal swipe file of high-performing hooks from Instagram reels.
> Used by /script for inspiration when writing new scripts.
> Add more anytime with /hooks [url]
```

---

## Step 5 — Confirm

Tell the user:
- What the hook was (quote it)
- What type it is
- That it's saved (Hook #N in the database)
- Total hooks in database so far

**Do NOT show the full transcript** unless the user asks. Just the hook + confirmation.

---

## Step 6 — Clean up

```bash
rm -f /tmp/hook_reel.mp4 /tmp/hook_reel.txt
```
