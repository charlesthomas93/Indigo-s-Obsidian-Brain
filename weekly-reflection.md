---
name: weekly-reflection
description: Weekly reflection and review for the Obsidian vault. Use when the user says "weekly reflection", "weekly review", "week review", "week in review", "how was my week", "reflect on my week", "weekly summary", "review my week", "what did I do this week", or any similar phrase indicating they want a comprehensive look back at the past 7 days covering themes, accomplishments, and an overview of how the week went.
---

# Weekly Reflection

Generates a thorough weekly review: all notes touched during the week, themes across daily notes, compiled accomplishments, habits trends, work balance analysis, and an honest narrative of how the week went. Output is a single structured reflection — insightful, grounded, and useful for spotting patterns the daily view misses.

## Step 1: Gather Context (all in parallel)

Run all of the following simultaneously:

1. **Past 7 daily notes** — Use `mcp__obsidian-rag__get_recent_daily_notes(days=7)`. Returns full content of each note.
2. **Today's daily note** — path: `00 - Timestamps/YYYY/MM-MonthName/YYYY-MM-DD-DayName.md` (derive from today's date). May overlap with the above — that's fine.
3. **All notes modified this week** — Find every file modified in the past 7 days:

   Read any non-daily-note files from this list that look substantive (project notes, resource docs, people notes). For each, extract: accomplishments mentioned, tasks added or completed, and any relevant context. Skip: daily notes (already gathered), images, attachments, and config files.
4. **PROGRESS.md** — Use `mcp__obsidian-rag__get_note("03 - Resources/PROGRESS.md")`
5. **CLAUDE.md** — Use `mcp__obsidian-rag__get_note("03 - Resources/CLAUDE.md")` — for role, rules, and constraints. Reference before producing output.
6. **Life Goals** — Read `03 - Resources/Life Goals.md` — the user's long-term goals across all domains.
7. **Recent therapy notes** — Find the 2 most recently modified files in `02 - Projects/Self Improvement/Therapy/` using:
   Read both files. Extract: what was worked on, tools or practices mentioned, any explicit "try this" instructions.
6. **Google Calendar (past 7 days)** — Run this Bash command to fetch the week's events:

   **Outlook (past 7 days)** — Call `mcp__Outlook__get_events_for_date` for each of the past 7 days.

## Step 2: Extract Themes and Search

Read through all 7 daily notes. Identify 3–6 recurring themes across the week — emotional patterns, relationships, projects, struggles, wins, energy trends. Each theme becomes a search query.

Examples of how to derive queries:
- Multiple days mention fatigue or low energy → query: `"tired exhausted low energy drained"`
- UOA work appeared in several notes → query: `"UOA game dev Uh Oh Airlines"`
- Recurring mentions of a person → query: `"[person name]"`
- Stress about a deadline → query: `"deadline pressure stress urgent"`

Run queries in parallel using `mcp__obsidian-rag__search_daily_notes` (date range: 30 days ago → today, n_results: 10 per query). This extends the view beyond the current week to spot longer-running patterns.

## Step 3: Analyze

### Accomplishments
Scan every daily note's Time Blocking section for `- [x]` items. Compile a complete list, grouped by project/tag (e.g., `#work`, personal). Include notable sub-tasks. This should be thorough — the user wants to see everything they got done.

### Incomplete / Carried Forward
Track `- [ ]` items that appeared across multiple days. Flag anything that carried forward 3+ days — these are stuck items that need attention.

### Habits Trends
From each day's Routines section, tally which habits were checked vs unchecked across the week. Present as a simple scorecard (e.g., "Medicine: 5/7 days", "Exercise: 3/7 days"). Note any streaks (positive or negative).

### Work Balance
From Time Blocking sections, estimate total hours on `#work` vs `#personal` for the week. Compare to the 50/50 target (Rule 4). Note which direction the balance tilted and whether it was appropriate given the week's circumstances.

### Themes
From the daily notes' Morning Check-in and Evening Reflection callouts, plus the search results from Step 2, identify the dominant emotional and situational threads of the week. Look for:
- Patterns that appeared 3+ times
- Trends (getting better, worse, or flat)
- Connections between themes (e.g., low energy days correlating with heavy work)
- Anything surprising or notable

### Notes Created / Modified
Compile the list of non-daily-note files that were created or meaningfully modified this week. Group by vault area (Projects, Resources, People, etc.). For each, briefly summarize what was added or changed — accomplishments, new tasks, decisions captured, etc. This shows the user what knowledge work happened beyond the daily routine.

### Calendar Retrospective
Summarize the week's calendar — how many meetings, any notable events, how much open time vs scheduled time. Note days that were particularly packed or particularly free.

## Step 4: Output

Produce the reflection in this format. Tone: honest, reflective, warm but not saccharine. Acknowledge hard days. Celebrate wins without overselling. The goal is a useful mirror — help the user see their week clearly.

---

# 📋 Weekly Reflection — Week of [Month] [D]–[D], [YYYY]

## 🏆 Accomplishments

[Thorough bulleted list of everything completed this week, grouped by area/tag. Include sub-tasks where meaningful. This is the "look at what you did" section — make it feel earned.]

### By the numbers
[Quick stats: total completed time blocks, tasks completed, any other quantifiable wins.]

---

## 📊 Balance & Habits

**Work balance:** [X hours work / Y hours personal — Z% / W%. Commentary on whether this hit the 50/50 target and why or why not.]

**Habits scorecard:**
[Table or list format. Each habit with days completed out of 7. Note streaks.]

---

## 🛋️ From Therapy

[Based on the last 2 session notes read in Step 1: what was the therapeutic focus this week? List any tools, practices, or insights from those sessions. Then check whether any of those themes showed up in this week's daily notes (morning check-ins, evening reflections). Format:

**This week's focus:** [1-2 sentences on what was worked on in therapy]
**Did it show up?** [Yes / Partially / No — with one example if yes. Be honest if there's a gap between what was worked on in therapy and what actually happened in daily life.]
**Carry into next week:** [One thing from the therapy notes worth keeping in mind.]

Skip this section entirely if no therapy session occurred this week.]

---

## 🧵 Themes

[3–6 bullet points identifying the week's dominant threads. For each: name the pattern, how many days it appeared, whether it's a new emergence or an ongoing trend, and any connections to other themes. Draw from morning check-ins, evening reflections, and search results. Be specific — "felt overwhelmed on Tuesday and Thursday, both heavy meeting days" is better than "some stress this week."]

---

## 🎯 Goals Progress

[Read `03 - Resources/Life Goals.md`. For each goal, write one bullet assessing this week's movement. Format: `- **[Goal]** — [what happened this week that served it, or "no movement this week"]. Status: 🟢 moving / 🟡 stalled / 🔴 neglected`

Be honest. A goal with no time blocks, no reflections, and no tasks this week is neglected — say so.]

---

## 🔄 Stuck Items

[Items that carried forward across 3+ days. For each: what it is, how many days it's been lingering, and a brief note on what might be blocking it. If nothing is stuck, say "Nothing stuck this week — clean slate."]

---

## 📝 Notes & Knowledge Work

[List of non-daily-note files created or modified this week, grouped by vault area. Brief note on what each represents. This captures the "intellectual output" beyond task completion.]

---

## 📅 Calendar Retrospective

[Summary of the week's schedule — meeting count, notable events, busiest day, most open day. Any observations about how the schedule affected energy or productivity.]

---

## 🤝 Social Connection

[Answer these two questions honestly based on the week's daily notes and reflections:

**Did you initiate at least one real conversation with someone new or a friend this week?** (Not a scheduled obligation — a genuine reach-out or in-person moment.) Name who, what happened, and how it felt. If no: say so plainly.

**Trend:** Is this better, worse, or the same as recent weeks? One sentence.]

---

## 🔮 Looking Ahead

[2–4 sentences. Based on the week's patterns and the Goals Progress section, what should the user be aware of heading into next week? Call out any goals that have been neglected and name one concrete action to move the most stalled goal. Any stuck items that need a decision? Balance corrections needed? Keep it forward-looking and actionable.]

---

## 💭 Closing Thought

[One honest, grounding sentence that captures the essence of the week. Not motivational-poster energy — something real that reflects what actually happened.]

---

## Step 5: Save Weekly Note

After presenting the full reflection in chat, write the complete output to a file in the vault.

**Derive the path:**
- Get today's ISO year and week number: `date +%G` and `date +%V`
- Path: `00 - Timestamps/[YEAR]/[YEAR]-W[##]-Weekly.md`
- Example: `00 - Timestamps/2026/2026-W15-Weekly.md`
- Use zero-padded week number (e.g., W05 not W5)

**File content:** the complete weekly reflection output exactly as presented in chat, with this frontmatter prepended:

```markdown
---
created: [YYYY-MM-DD]
week: [YEAR]-W[##]
tags: [[+Weekly Notes]]
---
```

**If the file already exists** (user ran the skill twice in the same week): overwrite it — the latest run is the authoritative version.

After writing, confirm: `✅ Weekly note saved → 00 - Timestamps/[YEAR]/[YEAR]-W[##]-Weekly.md`
