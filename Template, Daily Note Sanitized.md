<%*
const dateStr = (tp.file.title.match(/^\d{4}-\d{2}-\d{2}/)?.[0]) ?? "";
const date = window.moment(dateStr, "YYYY-MM-DD"); // parse only the date part

const year = date.format("YYYY");
const monthNum = date.format("MM");
const monthName = date.format("MMMM");
const dayNum = date.format("DD");
const weekday = date.format("dddd");

const yesterday = date.clone().subtract(1, "day");
const tomorrow  = date.clone().add(1, "day");

const yesterdayPath = `00 - Timestamps/${yesterday.format("YYYY")}/${yesterday.format("MM")}-${yesterday.format("MMMM")}/${yesterday.format("YYYY-MM-DD")}-${yesterday.format("dddd")}`;
const tomorrowPath  = `00 - Timestamps/${tomorrow.format("YYYY")}/${tomorrow.format("MM")}-${tomorrow.format("MMMM")}/${tomorrow.format("YYYY-MM-DD")}-${tomorrow.format("dddd")}`;

// Build output once
tR =
`---
created: ${tp.file.creation_date("YYYY-MM-DD")}
queryDate: ${dateStr}
---
tags:: [[+Daily Notes]]
# ${weekday}, [[${monthNum}-${monthName} | ${monthName}]] ${dayNum}, ${year}
<< [[${yesterdayPath}|Yesterday]] | [[${tomorrowPath}|Tomorrow]] >>
`;
%>

`BUTTON[New-task]` `BUTTON[new-communication]`

[[#🔄 Midday Reset]]
# 🌅 Morning

> [!quote]+ 🌅 Morning Check-in
> *What's on my mind? What do I want from today? How will I work towards my goals? Today, what does thriving (not surviving) look like?*
>
> **Primary Goal**: `INPUT[text(class(meta-bind-textbox))]`
>  **Secondary Goal**: `INPUT[text(class(meta-bind-textbox))]`

> [!quote]+ 📍 Move the Needle
- [ ] **Task 1:** 
- [ ] **Task 2:** 
- [ ] **Task 3:** 

---
## ☀️ Routines
> [!multi-column]
> > [!NOTE]+ [[Habits.canvas|Habits]]
> > - [ ] 💊 Take medicine
> > - [ ] 🏋🏽‍♂️ Exercise
> > - [ ] 🗾 Japanese
> > - [ ] 🪥 Brush Teeth
>
> > [!NOTE]+ Trackers
> > 😃 Mood: `INPUT[number:Mood]`
> > 💤 Sleep: `INPUT[number:Sleep]`
> > ⚡ Energy: `INPUT[number:Energy]`

# 🗾 Japanese Practice

## 読解 — Reading

## 作文 — Writing Prompt

**Your response:**

## Speaking
**Your response:** 

## Vocab Review
```dataviewjs
// Seeded random based on filename (date) - same day = same words
const seed = dv.current().file.name.split('').reduce((a, c) => a + c.charCodeAt(0), 0);
const seededRandom = (i) => {
	const x = Math.sin(seed + i) * 10000;
	return x - Math.floor(x);
};

const pages = dv.pages('"02 - Projects/Japanese/単語"').array();
const shuffled = pages
	.map((p, i) => ({ p, sort: seededRandom(i) }))
	.sort((a, b) => a.sort - b.sort)
	.slice(0, 5)
	.map(x => x.p);

dv.table(
	["Word", "読み方", "意味"],
	shuffled.map(p => [p.file.link, p.Pronunciation, p.Definition])
);
```

---

---
# 📅 Schedule
> [!multi-column]
> > [!note] + Agenda
> > ```gEvent
> > type: schedule
> > offset: 0
> > timespan: 1
> > showAllDay: true
> > navigation: false
> > ```
>
> > [!note] + Calendar
> > ```custom-frames
> > frame: Google Calendar
> > style: height: 350px;
> > urlSuffix: day
> > ```
```meta-bind-button
label: New task
icon: plus
hidden: true
class: ""
tooltip: ""
id: "New-task"
style: default
actions:
  - type: js
    file: 01 - Assets/tasks.js
    args: {}
```
```meta-bind-button
label: New task
icon: plus
hidden: true
class: ""
tooltip: ""
id: "new-communication"
style: default
actions:
  - type: js
    file: 01 - Assets/New Communications.js
    args: {}
```
# ✅ Tasks
> [!multi-column]
> > [!due-today] + ## Daily `BUTTON[New-task]`
> >```tasks
> >not done
> > heading includes Tasklist
> > path includes 00 - Timestamps
> > (happens today) OR (no happens date)
> >```
>
> > [!due-today] + ## 🗣️ Communications `BUTTON[new-communication]`
> > >```tasks
> > >not done
> > heading includes Communications
> > path includes 00 - Timestamps
> > (happens today) OR (no happens date)
> >```
# 🧠 Focus
> [!multi-column]
> >[!Overdue]
> >```tasks
> >not done
> >happens before {{query.file.property('queryDate')}}
> >sort by happens
> >sort by description
> >hide task count
> >```
>
> >[!danger]+ High Priority
> >```tasks
> >happens {{query.file.property('queryDate')}}
> >priority is above medium
> >not done
> >sort by happens
> >sort by description
> >hide task count
> >sort by description
> >```

 >[!aggregated]+ Aggregated
>```tasks
>not done
>priority below high
>happens {{query.file.property('queryDate')}}
>sort by happens
>sort by description
>hide task count
>```

---
# 🔮 Upcoming
>[!later]+ Due Tomorrow
>```tasks
>not done
>happens on <%* try { tR += moment().add(1, 'day').format('YYYY-MM-DD'); } catch(e) { tR += "ERROR"; } %>
>sort by description
>hide task count
>description does not include Take medicine
>```

>[!coming-soon]- Future Log
>```tasks
>not done
>(happens after today) AND (happens before in 1 month)
>sort by happens
>group by function task.happens.format("YYYY[%%]-MM[%%] MMM [- Week] WW")
>```

---

> [!multi-column]
>> **Current State**
>> - Mood:
>> - Energy:
>> - Focus:
>> - Thriving: `INPUT[toggle:Thriving]`
>
>> **Body Check**
>> Eat    `INPUT[toggle:Eat]`
>> Drink `INPUT[toggle:Drink]`
>>Move `INPUT[toggle:Move]`
>>Rest    `INPUT[toggle:Rest]`
>>Hold yourself    `INPUT[toggle:Hold]`
# 🔄 Midday Reset
> [!quote]+ 🧭 Check-in
> *Pause. Notice. Adjust.*
> *How are things going? What have I actually done so far? Am I on track or avoiding something? Am I staying present?*

**Next Goal:**

**Adjustment (if needed)**
- One shift:

---
# 🔗 Quick Links
> [!multi-column]
> > [!note]+ Project 2
> > [[03 - Resources/PROGRESS|📊 PROGRESS]]
> > [[Project 2 Dashboard]]
> > [[02 - Projects/Uh Oh Airlines/kbb_Bugs - Uh Oh Airlines|🐛 Bugs]]
> > **Recent Patches:**
> > ```dataview
> > LIST WITHOUT ID file.link
> > FROM "02 - Projects/Project 2/Patch Notes"
> > SORT file.ctime DESC
> > LIMIT 3
> > ```
>
> > [!note]+ Other
> > [[02 - Projects/Japanese/単語|🗾 Vocab Folder]]
> > [[Japanese|Japanese Dashboard]]

---
# 🌙 Evening

> [!quote]+ 🌙 Evening Reflection
> *How did the day go? What did I learn? What am I grateful for?*


> [!quote]+ 🪞 One Question
> *[question filled in each morning by /daily]*
>

> [!quote]+ 🌡️ Day Score
> 🎯 `INPUT[number:DayScore]`
> *I gave today a [score] because...*
>

---
# ✨ Today's Activity

>[!Success]+ Done Today
>```tasks
>done on {{query.file.property('queryDate')}}
>```

>[!info]- 📄 Notes Created Today
>```dataview
>List FROM "" WHERE file.cday = date("{{date:YYYY-MM-DD}}") SORT file.ctime asc
>```

>[!info]- ✏️ Notes Modified Today
>```dataview
>List FROM "" WHERE file.mday = date("{{date:YYYY-MM-DD}}") SORT file.mtime asc
>```

---
## Tasklist
## Communications
## Time Blocking
<%*
const day = tp.date.now("ddd");
const tasks = new Set();

// Core (every day)
[
  "- [ ] 08:00 - 08:30 Breakfast/Day planning",
  "- [ ] 08:30 - 09:30 Japanese",
  "- [ ] 12:00 - 13:00 Lunch",
  "- [ ] 14:30 - 15:00 Tasklist/Break",
  "- [ ] 16:00 - 16:30 List cleanup/Review",
].forEach(t => tasks.add(t));

// Work / special blocks
if (day === "Fri") {
  tasks.add("- [ ] 09:30 - 11:00 #billingsley meetings");
  tasks.add("- [ ] 11:00 - 12:00 Work");
} else if (day === "Sun") {
  tasks.add("- [ ] 09:30 - 11:00 Work");
  tasks.add("- [ ] 11:00 - 12:00 Wash Hair");
} else {
  tasks.add("- [ ] 09:30 - 12:00 Work");
}

// Clean on non-commute days (matches your original: Sun + other days, not Mon/Fri)
if (!["Mon", "Fri"].includes(day)) {
  tasks.add("- [ ] 14:00 - 14:30 Clean");
}

// Commute days
if (["Mon", "Fri"].includes(day)) {
  tasks.add("- [ ] 07:15 - 08:00 Drive to work");
  tasks.add("- [ ] 16:30 - 18:00 Drive home");
}

if (["Mon","Tue","Wed","Thu","Fri"].includes(day)) {
  tasks.add("- [ ] 13:00 - 14:30 Work");
  tasks.add("- [ ] 15:00 - 16:00 Work");
}

tR += Array.from(tasks).join("\n");
-%>
