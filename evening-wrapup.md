---
name: evening-wrapup
description: Evening wrap-up and reflection for the Obsidian vault. Use when the user says "evening wrap-up", "wrap up my day", "end of day", "day review", "evening review", "how did my day go", "wrap up", or any similar phrase indicating they are finishing their day and want a reflective summary plus preview of tomorrow.
---

# Evening Wrap-up

Reflective end-of-day summary: what got done, what carries forward, habits check, evening reflection, and tomorrow's preview.

## Step 1: Gather Context (all in parallel)

1. **IMPORTANT** **Today's modified notes** — Find and read all notes modified today using this Bash command (use a heredoc to prevent bash from mangling PowerShell's `$_`):

   Replace `YYYY-MM-DD` with today's date and `YYYY-MM-DD+1` with tomorrow's date. Then read all returned files, skipping: `.trash/`, Japanese vocab notes (`02 - Projects/Japanese/単語`), and the daily notes already being read in steps 3–5.
2. **Google Calendar** — Run this Bash command to fetch today's and tomorrow's events across all three calendars:
   **Outlook** — `mcp__Outlook__get_todays_events` and `mcp__Outlook__get_events_for_date` for tomorrow
3. **Today's daily note** — `00 - Timestamps/YYYY/MM-MonthName/YYYY-MM-DD-DayName.md`
4. **Tomorrow's daily note** — same path pattern, next day. May not exist yet — that's fine.
5. **Prior 3 daily notes** — Use `mcp__obsidian-rag__get_recent_daily_notes(days=3)`. Returns full note content — no path construction needed.
6. **CLAUDE.md** — Use `mcp__obsidian-rag__get_note("03 - Resources/CLAUDE.md")` — for role, rules, and constraints. Reference before producing output.

Once today's note is loaded, proceed to Step 1b before continuing to Step 2.

No need to read PROGRESS.md or Recurring Tasks unless a specific issue warrants it.

## Step 1b: Extract Themes and Search (sequential after Step 1)

Look at today's note for reflection content in this priority order:
1. The `🌙 Evening Reflection` callout — if filled out, use this as the primary source
2. The `🌅 Morning Check-in` callout — use if evening reflection is empty
3. The day's completed time blocks and any Tasklist/Notes content — use as a fallback if both callouts are empty

Identify 2–4 distinct themes the user is engaging with — emotions, people, projects, situations, recurring concerns. Each becomes a search query.

Examples of how to derive queries:
- Evening reflection mentions feeling proud of finishing something → query: `"finished shipped completed proud"`
- Morning check-in mentioned processing something with parner → query: `"relationship"`
- Day was heavy with work but felt unproductive → query: `"work unproductive stuck"`
- User mentioned being tired or sleep affecting the day → query: `"tired sleep exhausted drained"`

If today's note has no reflective content at all (all callouts empty, no meaningful notes), skip this step entirely and omit the 🔁 Recurring Themes section from output.

Run the derived queries in parallel using `mcp__obsidian-rag__search_daily_notes` (date range: 30 days ago → today, n_results: 8 per query). Collect results for Step 2.

## Step 2: Extract and Analyze

### Accomplishments
From today's Time Blocking section:
- **Completed blocks:** `- [x]` items and their sub-tasks
- **Incomplete blocks:** `- [ ]` items — these may carry forward

### Habits
From today's Routines section (checklist items):
- Note which were checked (`- [x]`) and which weren't (`- [ ]`)
- Don't editorialize — just reflect the state. If medicine was missed, note it plainly.

### Evening Reflection
Check the `> [!quote]+ 🌙 Evening Reflection` callout in today's note:
- **If filled out:** Read it carefully. Engage with what the user wrote — reflect it back meaningfully, draw connections to the day's accomplishments or struggles, affirm insights, and gently add perspective if warranted. Don't just quote it back.
- **If empty or contains only the placeholder prompt:** Include the reflection prompt in the output to invite the user to fill it in.

### Evening Reflection Themes
From the search results in Step 1b, look for patterns across the past 30 days for each theme derived from today's reflection.

**IMPORTANT — verify before claiming patterns:** RAG search results return only file paths and similarity scores, not content. A high score does not confirm the theme appears in that note. Before asserting that a theme is recurring, read the content of at least 3–4 of the highest-scoring matched notes using the Read tool. Only report a theme as recurring if you can confirm it in the actual note text.

- Is this theme showing up repeatedly, or is today unusual?
- Is it trending better, worse, or staying flat?
- Are there connections between themes (e.g., tired days correlating with certain project struggles)?

Only surface themes that appear in 3+ *verified* notes. If no meaningful patterns emerge after reading, or if Step 1b was skipped, omit this section entirely.

### Carry-forwards
Tasks from today's Tasklist and incomplete time blocks that should move to tomorrow. Cross-reference with prior 3 days — flag anything that has been carrying forward repeatedly (2+ days).

### Tomorrow's Preview
From all calendars (Google + Outlook): list tomorrow's events. From tomorrow's daily note if it exists: note any pre-planned time blocks. If neither exists, note what's known from context (e.g., regular Kai days, commute days).

## Step 3: Output

Produce a warm, reflective briefing. Tone: grounded, honest, forward-looking — not a productivity lecture. Acknowledge what was hard. Celebrate what was done.

---

# 🌙 Evening Wrap-up — [Weekday], [Month] [D]

## ✅ What Got Done
[Bulleted list of completed time blocks and notable tasks. Include sub-tasks where meaningful. If light on completions, acknowledge the day honestly — some days are about other things.]

## 🔄 Habits
[State each habit from the Routines section and whether it was done. One line each. E.g. "💊 Medicine — done", "🏋🏽‍♂️ Exercise — skipped", "🗾 Japanese — done".]

## 🌙 Reflection
[If evening reflection was filled out: engage with it thoughtfully — 2-4 sentences that reflect it back, connect it to the day, and offer a brief insight or affirmation. If not filled out: include the prompt "How did the day go? What did I learn? What am I grateful for?" to invite the user to write.]

## ➡️ Carrying Forward
[List incomplete tasks and time blocks that didn't happen. Flag any items appearing for the 3rd+ day with a ⚠️. Be concrete — for each item, note whether it should move to tomorrow or be dropped/deferred further.]

## 📅 Tomorrow
[Bullet list of tomorrow's calendar events sorted by time. Format: `HH:MM – HH:MM — Event Name`. Then 1-2 sentences orienting the day: day type (commute? Kai? climbing?), known workload, anything to prepare tonight.]

## 🧭 Advisor

[This section is generated by the advisor agent. See Step 2b.]

**Modified files**
[Bullet list of all files modified today.]
---

## Step 2b: Dispatch Advisor Agent

After producing the briefing up through the Carrying Forward section, dispatch the **advisor agent** using the Agent tool:

```
Agent(
  subagent_type: "advisor",
  description: "Evening reflection and guidance",
  prompt: "<provide the advisor with: what got accomplished today, what didn't get done, habits completed/missed, the evening reflection text (if any), carry-forwards and how many days they've been carrying, tomorrow's calendar preview, recurring themes found, and energy/mood data. Then ask: 'Looking at how today went, what's your read? And what's the one thing Charles should keep in mind heading into tomorrow?'>"
)
```

Insert the advisor's response into the 🧭 Advisor section of the briefing. Do not editorialize or rewrite the advisor's output — present it as-is.
