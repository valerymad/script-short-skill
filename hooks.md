---
name: hooks
description: "Generate scroll-stopping hooks for short-form video, OR save a hook you saw. GENERATE: returns 3 hook variants from a 120-formula library (12 psychology triggers) + your saved hooks — activate when the user asks 'give me hooks for [topic]', 'write 3 hooks for this Reel/TikTok/Short about X', 'I have a script about Y, give me hook options', 'write opening lines about Z', wants a confession/contrarian/story-style hook, pastes a script and wants alternative openers, or is brainstorming scroll-stoppers. SAVE: when the user gives /hooks <Instagram/TikTok/YouTube URL>, download → transcribe → extract → categorize → store in their personal hooks database. Works with or without /hooks; bilingual RU/EN."
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - AskUserQuestion
---

# /hooks — Save or Generate Hooks

## Mode detection (do this first)

Look at the argument after `/hooks`:

- **Starts with `http://` / `https://`** (instagram.com, tiktok.com, youtube.com,
  youtu.be, …) → **SAVE MODE**.
- **Anything else** (a topic, a pasted script, or empty) → **GENERATE MODE**.

---

# SAVE MODE — add a hook to your database

Save a high-performing hook from a real video to your personal swipe file. Every
saved hook makes `/script` better.

## S0 — Find config
Read `~/Documents/script-short-skill/config.json` → `dataDir`, `toolPaths.ytDlp`,
`toolPaths.transcribeScript`.
If it doesn't exist:
> "Run /script first to set up your skill (installs tools, creates your database)."
Then stop. (You can still use GENERATE MODE without setup — it falls back to the
shipped formula library.)

## S1 — Download (yt-dlp, with cookie fallback)
```bash
"$YT_DLP" "<URL>" -o /tmp/hook_reel.mp4 --merge-output-format mp4 -q \
  || "$YT_DLP" "<URL>" -o /tmp/hook_reel.mp4 --merge-output-format mp4 -q --cookies-from-browser safari \
  || echo "DOWNLOAD_FAILED"
```
If it fails (private / deleted / geo / age-gated), tell the user and stop.

## S2 — Transcribe (via the transcribe-media-local skill)
```bash
python3 ~/.claude/skills/transcribe-media-local/scripts/transcribe.py \
  /tmp/hook_reel.mp4 --output /tmp --format txt
```
Read `/tmp/hook_reel.txt`. (Do not pass `--model`/`--language`; the skill's
defaults — `small`, auto-language — handle RU/EN automatically. If the skill is
missing, fall back to `whisper /tmp/hook_reel.mp4 --model small --output_format txt --output_dir /tmp/ --fp16 False`.)

## S3 — Extract + categorize
The hook = the opening 1–3 sentences (~first 5–10s). Extract it precisely. Then
pick the closest category from the 12-category taxonomy (see
`hooks-formulas-en.md` / `hooks-formulas-ru.md`):

`01 Curiosity · 02 Contrarian · 03 Authority · 04 Emotional · 05 Listicle ·
06 Question · 07 Story · 08 Negation · 09 Specificity · 10 Confession ·
11 Urgency/FOMO · 12 Discovery/Result-Lead`

Also write a 1-sentence "why it works".

## S4 — Save to `{dataDir}/hooks-database.md`
Read the file, count existing hooks for the next number, append:
```markdown
## Hook #[N]
**Source:** [URL]
**Creator:** @[handle from URL if derivable, else "unknown"]
**Date:** [today YYYY-MM-DD]
**Views:** [if known, else n/a]
**Category:** [Cat NN — Name]

**Hook:**
> "[exact opening lines]"

**Why it works:** [1 sentence]

**Full opening (~10s):**
"[first 3–5 sentences]"
```
If the file doesn't exist, create it with a header first:
```markdown
# Hooks Database
> Personal swipe file of high-performing hooks from real videos.
> Used by /script and /hooks (generate) for inspiration.
> Add more anytime with /hooks [url]
```

## S5 — Confirm + clean up
Tell the user: the hook (quoted), its category, "saved as Hook #N", total hooks
now. Do NOT dump the full transcript unless asked.
```bash
rm -f /tmp/hook_reel.mp4 /tmp/hook_reel.txt
```

---

# GENERATE MODE — 3 hook variants from a topic/script

## G0 — Read the input
If the user pasted a script, read it. If they gave only a topic, ask ONE short
question about the audience or angle, then proceed. Detect language (RU/EN) from
the input (or ask).

## G1 — Load the library
Read the matching formula file: `hooks-formulas-ru.md` (RU) or
`hooks-formulas-en.md` (EN) from this skill folder. **Always read it.**
If `~/Documents/script-short-skill/config.json` → `dataDir/hooks-database.md` exists,
also read it (the user's proven hooks).

## G2 — Pick 3 hooks
Identify the emotional beat the topic fits. Pick 3 formulas, best practice: mix
one **stop-scroll** category (08 Negation / 09 Specificity / 06 Question) with one
**retention** category (07 Story / 10 Confession / 04 Emotional); third is free.
Prefer patterns proven in the personal `hooks-database.md` when relevant.

Fill the `[brackets]` with the topic's specific words, number, pain, or audience.
Keep each filled hook under 12 words, native to the platform (TikTok/IG = casual;
LinkedIn = punchy/declarative; YouTube = title-style; X = short/contrarian).

## G3 — Output
```
HOOK 1 — [Cat NN — Name · #ID · Platform] · best for [reason]
"<filled hook>"

HOOK 2 — [Cat NN — Name · #ID · Platform] · best for [reason]
"<filled hook>"

HOOK 3 — [Cat NN — Name · #ID · Platform] · best for [reason]
"<filled hook>"
```
Then one sentence: which to lead with and why. Offer:
**"Want the full script around one of these? Run /script."**
