# School Comparison Skill

A Claude skill (`school-comparison.skill`) for researching and comparing the public elementary schools and kindergarten programs in one US county or school district. It can also turn the results into an interactive, classroom-themed dashboard.

## What it's for

The skill helps parents and families choosing a school zone, for example before buying or renting a home. It gets used when someone asks things like:

- "What are the best public elementary schools in Davidson County?"
- "How do the kindergarten programs in Metro Nashville compare?"
- "Show me the state letter grades and Niche ratings for my district."
- "Build me a dashboard comparing schools in my county."

## What it does

The skill works in three stages. Claude stops after whichever stage the user asked for.

### 1. Research
- Maps a city to its public school district and confirms the scope.
- Explains up front that kindergarten isn't state tested, so every rating covers the whole elementary school and mostly reflects grades 3–5.
- Collects two kinds of rating, because they disagree in useful ways:
  - **The state's official rating**, such as A–F letter grades, which often weight academic growth. It also records the district-wide count of schools at each grade.
  - **Third-party grades** from Niche (and optionally GreatSchools).
- Records these for each school: area, both grades, enrollment, student-teacher ratio, grade span, whether Pre-K is on site, how a seat is assigned (zoned, priority zone plus lottery, or charter), and a short note. Fields it can't find stay empty. It never guesses a grade.

### 2. Compare in chat
Claude writes a plain-language reply for parents. It includes:
- A table sorted from best to worst.
- A "what stands out" section: schools both systems rate highly, the biggest mismatches between the two systems, unusually high class ratios, and the state's lowest-rated schools.
- How enrollment works (magnets, lotteries, charters, zone changes), plus a pointer to the district's school finder.
- What the grades miss, and questions to ask on a kindergarten tour.

### 3. Interactive dashboard
Claude writes the research to a JSON file and runs a build script that validates it and produces a self-contained HTML page with a classroom theme:
- A chalkboard header, and a ruler showing the state's district-wide grade distribution.
- Index-card stats that recompute as you filter.
- Filters by search, area, admission type, Pre-K, and state grade.
- Crayon and pencil charts, and an enrollment-vs-ratio scatter plot drawn with apples.
- A sortable table where clicking a school opens a "report card" comparing it to the district median and to similar schools.
- Light and dark themes, keyboard access, and reduced-motion support.

## What's inside the `.skill` file

The `.skill` file is a zip archive:

| File | Purpose |
|---|---|
| `SKILL.md` | Main instructions: when to use the skill and how to run each stage |
| `references/research-method.md` | Search queries that worked, notes on state rating systems, and pitfalls |
| `references/data-schema.md` | The JSON format the dashboard expects |
| `references/example-davidson-county.json` | A complete worked example for Metro Nashville Public Schools (Davidson County, TN) |
| `scripts/build_dashboard.py` | Validates the data and fills the dashboard template |
| `assets/dashboard_template.html` | The classroom-themed dashboard template |

## Worked example

The skill was built from research on **Davidson County, Tennessee (Metro Nashville Public Schools)**. That research used Tennessee's 2024-25 letter grades (14 A, 23 B, 57 C, 39 D, and 10 F schools across the district) and Niche's 2026 grades for 25 elementary schools. One pattern it found: the two rating systems disagree most where academic growth is strong but class ratios are large.

## How to use it

1. Install the skill in your Claude account (or upload `school-comparison.skill`).
2. In a new chat, ask something like: *"Use my school comparison skill to compare elementary schools in Davidson County and build the dashboard."*
