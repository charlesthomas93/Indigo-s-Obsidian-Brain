---
name: morning-plan
description: Full morning briefing and day plan for the Obsidian vault. Use when the user says "morning plan", "good morning", "start my day", "morning briefing", "plan my day", "what's my day look like", or any similar phrase indicating they are starting their day and want an overview of their schedule, tasks, and a proposed time-block plan.
---

# Morning Plan

Generates a complete morning briefing: calendar events, overdue/due-today tasks, carry-forwards from yesterday, and a proposed time-block schedule. Output is a single structured briefing — no tool result dumps, just clean analysis and a ready-to-go plan.

## Step 1: Gather Context (all in parallel)

Run all of the following simultaneously:

1. **Google Calendar** — Run this Bash command to fetch today's events across all three calendars:
   ```bash
   TODAY=$(date +%Y-%m-%d); TOMORROW=$(date -d tomorrow +%Y-%m-%d); for cal in "primary" "family14670976850356916614@group.calendar.google.com" "20394623f738d4c89a7d45b1b05eae121f0311ca372cddab776fb78a0b308c63@group.calendar.google.com"; do gws calendar events list --params "{\"calendarId\":\"$cal\",\"timeMin\":\"${TODAY}T00:00:00Z\",\"timeMax\":\"${TOMORROW}T00:00:00Z\",\"singleEvents\":true,\"orderBy\":\"startTime\"}"; done
   ```
   **Outlook** — `mcp__Outlook__get_todays_events`
2. **Today's daily note** — path: `00 - Timestamps/YYYY/MM-MonthName/YYYY-MM-DD-DayName.md`
   - Derive path from today's date (e.g., `2026-02-24` → `00 - Timestamps/2026/02-February/2026-02-24-Tuesday.md`)
   - Month folder format: `MM-MonthName` (e.g., `02-February`)
3. **Prior 7 daily notes** — Use `mcp__obsidian-rag__get_recent_daily_notes(days=7)`. This returns the full content of each note — no need to construct paths manually.
4. **Recurring Tasks** — `03 - Resources/Recurring Tasks.md`
5. **PROGRESS.md** — Use `mcp__obsidian-rag__get_note("03 - Resources/PROGRESS.md")`
6. **CLAUDE.md** — Use `mcp__obsidian-rag__get_note("03 - Resources/CLAUDE.md")` — for role, rules, and constraints. Reference before producing output.
7. **Life Goals** — Read `03 - Resources/Life Goals.md` — the user's long-term goals across all domains.

Once today's note is loaded, proceed to Step 1b before continuing to Step 2.

## Step 1b: Extract Themes and Search (sequential after Step 1)

Read today's Morning Check-in callout. Identify 2–4 distinct themes the user is writing about — these could be emotional states, specific people, projects, situations, or concerns. Each theme becomes a search query.

Examples of how to derive queries:
- User writes about feeling directionless with game dev → query: `"game dev motivation direction"`
- User mentions processing something with Laura → query: `"Laura relationship"`
- User writes about dreading a work task → query: `"billingsley dread avoid"`
- User mentions sleep quality → query: `"sleep tired exhausted"`

If today's Morning Check-in is empty or just the template placeholder, skip this step entirely and omit the 🔁 Recurring Themes section from output.

Run the derived queries in parallel using `mcp__obsidian-rag__search_daily_notes` (date range: 30 days ago → today, n_results: 8 per query). Collect results for Step 2.

**If any theme is personal/emotional** (mentions a person, feelings, relationship, identity, grief, isolation, self-worth, or anything that would belong in therapy), also search the therapy folder using `mcp__obsidian-rag__search_vault` with that theme as query, restricted to `path:02 - Projects/Self Improvement/Therapy` or `path:02 - Projects/Self Improvement`. Surface any matching therapy insight in the 🔁 Recurring Themes section as a callout: *"From therapy: [quote or paraphrase]."* Only include if the match is clearly relevant — don't force it.

## Step 2: Extract and Analyze

### Tasks
From **Recurring Tasks.md**, extract all `- [ ]` items where `📅 YYYY-MM-DD` ≤ today.

From **prior daily notes** (Tasklist sections), extract:
- Overdue: `- [ ]` items with `📅 date` before today
- Due today: `- [ ]` items with `📅 date` = today or no date
- Carry-forwards: incomplete items from yesterday with no future due date

Deduplicate across sources.

### Calendar Events
From all calendars (Google + Outlook), list all events for today. Note times. Flag back-to-back or conflicting events.

Do NOT create time blocks for calendar events (Rule 6).

### Morning Reflection
If today's note has a "Morning Check-in" callout with content beyond the template placeholder, include a summary of what the user wrote.

### Day Type
Determine from today's date and the schedule rules in `03 - Resources/CLAUDE.md`:
- Weekday vs weekend
- Commute day (Monday or Friday)
- Climbing day (Mon, Thu, or one weekend day)

### Eat the Frog
Identify the tasks the user has been avoiding most. Use this priority order:
1. Tasks overdue by 2+ days (most neglected first)
2. Tasks that keep appearing as carry-forwards across multiple daily notes
3. Tasks the user has described as dreading or putting off in recent morning/evening reflections

Include ALL pressing avoided tasks — not just one. These form the "frog block" in the schedule.

Determine block duration: 30 minutes by default. If the combined frog tasks clearly require more time (e.g., a phone call + follow-up, a multi-step chore), extend to up to 1 hour. Never exceed 1 hour.

### Weekly Balance Check
From the past 7 daily notes, scan Time Blocking sections. Estimate hours spent on `#work` vs personal this week so far. Use this to inform whether today should lean personal or personal (target: ~50/50 by end of week per Rule 4).

### Morning Reflection Themes
From the search results in Step 1b, look for patterns across the past 30 days for each theme derived from today's reflection:
- Does this theme appear in multiple notes, or is today the first mention?
- Is it getting better, worse, or staying flat?
- Are there any surprising connections across themes?

Only surface themes that appear in 3+ notes — single mentions are noise, not patterns. If no meaningful themes emerge (or if Step 1b was skipped), skip the section entirely.

## Step 3: Output

Produce the briefing in this exact format. Keep each section tight — no padding.

---

# 🌅 Morning Plan — [Weekday], [Month] [D], [YYYY]

## 🚨 Urgent
> Skip this section entirely if nothing is urgent.

[Flag any: overdue tasks past their due date by 2+ days, time-sensitive calendar items, critical project blockers from PROGRESS.md, or anything from the morning reflection that signals stress/urgency.]

---

## 📅 Today's Calendar
[List events from both calendars, sorted by time. Format: `HH:MM – HH:MM — Event Name (calendar source)`. If no events, say "No events today."]

---

## ✅ Tasks

**Overdue**
[Bullet list. If none, say "None."]

**Due Today**
[Bullet list from Recurring Tasks + daily notes. If none, say "None."]

**Carry-forwards from yesterday**
[Incomplete tasks from yesterday's note with no future due date. If none, say "None."]

**Synthesized tasks**
[Bullet list from other notes that fit the theme of the day and would be interesting to tackle today.]

---

## 🎯 Goals Pulse

[Read the Life Goals file. For each goal, check whether today's plan, today's day type, or this week's context naturally moves it forward. Output as a tight bullet list — one line per relevant goal. Format: `- **[Goal]** — [one sentence on how today connects to it, or "no direct opportunity today"]`. Skip goals with no connection. Always include at least one that today does serve.]

---

## 🐸 Eat the Frog

[List the frog tasks identified in Step 2. Format as a bullet list. After the list, state the block duration (30 min or 1 hr) and briefly explain why these were selected (e.g., "3 days overdue", "carried forward 4 times"). If no frog tasks exist, say "Nothing hiding today — nice work." and skip the block in the schedule.

In the Proposed Schedule, the frog time block should list each frog task as an indented checkbox underneath the main block entry, e.g.:
```
- [ ] 12:00 - 12:30 🐸 Eat the Frog
	- [ ] Clean CPAP gear
	- [ ] Replace coffee filter
```
]

---

## ⏰ Proposed Schedule

[Propose time blocks for the full day. Rules:
- Use schedule constraints from CLAUDE.md (billingsley window, commute, Kai pickups, etc.)
- Apply weekly balance — if behind on UOA, front-load UOA blocks; if behind on Billingsley, front-load work
- Slot the 🐸 Eat the Frog block after the first real work sprint (first #uoa or #billingsley block) and after Japanese — never before. The user needs momentum before tackling the frog. Duration: 30 min default, up to 1 hr if the frog tasks warrant it. Skip if no frog tasks were identified. The frog block must not overlap any calendar events — shift it to the nearest open slot if needed.
- Take time from work blocks before moving anything personal (Rule 1)
- Do NOT create blocks for calendar events (Rule 6)
- Respect wake time: if user says "I just woke up", don't propose blocks before that time
- Format: `HH:MM – HH:MM — [Block Name] #tag`]

---

## 🔁 Recurring Themes

> Skip this section entirely if no themes met the 3+ note threshold.

[2–4 bullet points. Each bullet names a pattern found across recent morning reflections and how long it's been appearing. Example: "Low energy on Mondays — appeared in 4 of the last 5 Monday notes." or "Repeated anxiety around the Billingsley deadline — mentioned 5 times in the past 2 weeks." Keep it factual, not prescriptive.]

---

## 🧭 Advisor

[This section is generated by the advisor agent. See Step 3b.]

---

## Step 3b: Dispatch Advisor Agent

After producing the briefing up through the Recurring Themes section, dispatch the **advisor agent** using the Agent tool:

```
Agent(
  subagent_type: "advisor",
  description: "Morning guidance for today",
  prompt: "<provide the advisor with: today's morning check-in text, the proposed schedule, the task list (overdue + due today + frogs), the weekly balance numbers, recurring themes found, energy/sleep/mood from today's frontmatter, any urgent items, and the Life Goals (from 03 - Resources/Life Goals.md). Then ask: 'Given all this, what should Charles focus on today and what's the one thing he should watch out for? Flag if today's plan is drifting away from any of his long-term goals.'>""
)
```

Insert the advisor's response into the 🧭 Advisor section of the briefing. Do not editorialize or rewrite the advisor's output — present it as-is.

---

## Step 3c: Kai Section (Tuesday, Wednesday, Thursday only)

Check today's day of week. If it is **Tuesday, Wednesday, or Thursday**, write two callouts to today's daily note.

**Morning intention callout** — insert directly after the `> [!quote]+ 📍 Move the Needle` block (after its last `- [ ]` item), before the `---` separator:

```markdown
> [!quote]+ 🧡 Kai — Today's Intention
> *How do I want to show up with Kai today? What's one thing I'll try?*
>
```

**Evening reflection callout** — insert directly after the `> [!quote]+ 💛 Love & Connection` block (after its last line), before the next `> [!quote]` callout:

```markdown
> [!quote]+ 🧡 Kai — Evening Reflection
> *What's one thing I noticed about Kai today? Did I try anything different? Did it land?*
>
```

If today is **not** Tuesday, Wednesday, or Thursday: skip this step entirely. Do not add any Kai callouts.

If either callout already exists in the note (check before writing): skip adding that one — don't duplicate.

---

## Step 4: Daily Growth Question

After the advisor response, select one question from the bank below that best matches the emotional texture of today's morning check-in. Check the prior 3 daily notes' `🪞 One Question` sections to avoid repeating a recently used question.

Write the selected question to today's daily note by replacing the placeholder in the `> [!quote]+ 🪞 One Question` callout.

**Question bank:**
1. Where did I protect myself today when I didn't need to?
2. Did I let someone's care or warmth actually land, or did I deflect it?
3. What feeling did I have today that I didn't say out loud?
4. Where did I show up as myself — not the version I perform for others?
5. What did I need today that I didn't ask for?
6. Did I give love today in a way the other person could feel?
7. Where did I close off when I could have stayed open?
8. What would I have said today if I weren't afraid of how it would land?
9. Who did I connect with today, and what made it feel real?
10. What did I do today just for myself — not for productivity, not for others?
11. Was there a moment I felt genuinely seen today? What made that possible?
12. Where did I rush past something that deserved more presence?
13. What am I carrying right now that I haven't shared with anyone?
14. Did I hold myself with the same compassion I'd offer someone I love?
15. What did today teach me about who I'm becoming?

**Selection logic:** Read the morning check-in themes and pick the question that would surface the most useful reflection given what the user is working through. If the check-in is empty, cycle through the bank by day of week (Mon → 1, Tue → 2, etc.). Don't repeat a question used in the prior 3 daily notes.

---

## Step 5: Japanese Practice

After presenting the morning briefing and updating the daily note's Time Blocking section, invoke the `/japanese` skill to generate today's Japanese practice section in the daily note. This runs as the final step of the morning plan — no user prompt needed.
