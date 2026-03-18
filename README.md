# AWHL Tournament Brackets

HTML bracket and standings pages for the Anchorage Women's Hockey League (AWHL) end-of-season tournament. These are designed to be pasted into **Sports Engine HTML code blocks** so teams and fans can follow scores and standings online instead of checking paper brackets at the rink.

---

## What's in this repo

Each skill level gets **two pages**: a division header and a full tournament tracker.

| File | Purpose |
|------|---------|
| `XX-XX-rec-tournament-brackets.html` | **Rec division header** — shows which teams are in each division (American / National). This goes on the tournament landing page. |
| `XX-XX-rec-tournament-tracker.html` | **Rec tournament tracker** — full game schedule, division standings tables, and playoff bracket boxes. This is the main working page you update throughout the week. |
| `XX-XX-int-brackets.html` | **Intermediate division header** — same idea as Rec, for the Intermediate level. |
| `XX-XX-int-tournament-tracker.html` | **Intermediate tournament tracker** — full schedule, standings, and playoff bracket for Intermediate. |

The `XX-XX` prefix is the season (e.g., `25-26` for the 2025–26 season). Rename files each year.

---

## How it works with Sports Engine

1. In Sports Engine, navigate to the page where you want the bracket (or create a new page).
2. Add an **HTML code block** (sometimes called "Custom HTML" or "Embed Code" depending on your Sports Engine layout editor).
3. Copy the **entire contents** of the relevant `.html` file and paste it into that code block.
4. Save / publish the page.

That's it; the HTML includes all its own CSS styling inline, so there are no external files or scripts to worry about. It's 100% static HTML and CSS.

---

## How to set up a new season

### Step 1: Copy last season's files

Duplicate all four files and rename them with the new season prefix:

```
25-26-rec-tournament-brackets.html    -->  26-27-rec-tournament-brackets.html
25-26-rec-tournament-tracker.html     -->  26-27-rec-tournament-tracker.html
25-26-int-brackets.html               -->  26-27-int-brackets.html
25-26-int-tournament-tracker.html     -->  26-27-int-tournament-tracker.html
```

### Step 2: Update the division header files

These are the simpler files. Open each one and change:

- **Title text**: Update the year in the `<tr class="title-row">` (e.g., "2027 AWHL Rec Tournament Divisions").
- **Team names**: In the `<tr class="team-row">`, replace each team name. Teams are separated by `<br>` tags within each division cell.

Example — the Rec brackets file has a structure like this:

```html
<tr class="team-row">
  <td>Team A<br>Team B<br>Team C</td>          <!-- American division -->
  <td>Team D<br>Team E<br>Team F<br>Team G</td> <!-- National division -->
</tr>
```

Just swap in the new season's team names.

### Step 3: Update the tournament tracker files

These are the bigger files. Here's what to change:

**a) Update the HTML comment block at the top.**
The large comment at the very top of each file is a cheat sheet for whoever is updating scores during the tournament. Update it with the new season's division assignments, team names, and playoff format.

**b) Update the title and season year.**
Find the `<h2>` tag near the top and change the year:

```html
<h2>2027 AWHL Rec Tournament</h2>
```

**c) Update the game schedule.**
The schedule is a `<table class="sched-table">` with one row per game. For each game row, update:
- Game number
- Day and date
- Time
- Rink code (UAA, BB1, BB2, DA1, DA2, O'Malley-Red, etc.)
- Home team name
- Away team name

Make sure the score cells are **empty** for the new season:

```html
<tr>
  <td>1</td>
  <td>Tue 3/17</td>
  <td>7:15 PM</td>
  <td>UAA</td>
  <td>Team A</td>
  <td class="score"></td>    <!-- leave empty -->
  <td>Team B</td>
  <td class="score"></td>    <!-- leave empty -->
</tr>
```

Both the Rec and Intermediate trackers use `<td class="score"></td>` for score cells. This gives scores bold, slightly larger styling so they stand out at a glance.

**d) Add or remove game rows as needed.**
If the schedule has more or fewer games than last year, just copy/paste or delete `<tr>...</tr>` rows. Keep the playoff games (marked with `class="playoff-row"`) at the bottom.

**e) Update the standings tables.**
Each division has a `<table class="stand-table">`. Replace the team names in the first `<td>` of each row and make sure all point/goal cells are empty:

```html
<tr><td>New Team Name</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
```

Add or remove rows if a division has a different number of teams than last year. If a division plays fewer games, you may need to remove a `G4` column (or add one). Adjust the `<th>` header row to match.

**f) Update the playoff bracket boxes.**
Change the game numbers and times/rinks in each `<div class="playoff-box">`. Reset the team names back to placeholders like "1st Place American" and make sure score divs are empty.

**g) Check the playoff format.**
The current format is:
- **Semi-Final #1:** 1st Place American vs 2nd Place National
- **Semi-Final #2:** 1st Place National vs 2nd Place American
- **Consolation:** 3rd vs 4th from the division with more teams
- **Championship:** Winner SF #1 vs Winner SF #2

If the playoff format changes, update the bracket boxes and the playoff rows in the schedule table accordingly.

### Step 4: Adjust team count if divisions change

The Rec and Intermediate levels may not have the same number of teams each year. Things to watch for:

- **Standings columns:** A division with 3 teams playing a double round-robin needs G1–G4 columns. A 4-team round-robin only needs G1–G3. Make sure the column count matches the number of games each team plays.
- **Schedule rows:** More teams = more games. Count carefully.
- **Consolation game:** This typically comes from the division with 4+ teams (3rd vs 4th place). If both divisions are small, you may not need a consolation game.

---

## How to update scores during the tournament

This is the part you'll be doing at the rink on your phone or laptop. You can edit the HTML directly in Sports Engine's code block editor — you don't need to go through GitHub for live updates. That said, it's best if you copy the code out of the webpage, into an editor like vsCode, make changes, then copy/paste back into the webpage. Otherwise you risk breaking formatting in that tiny text box the webstie gives you.

### Updating game scores

Find the row for the game that just finished. Put the score between the `<td class="score">` tags:

**Before:**
```html
<td>Ice Services</td><td class="score"></td><td>Pioneer Peak</td><td class="score"></td>
```

**After (Ice Services won 4-2):**
```html
<td>Ice Services</td><td class="score">4</td><td>Pioneer Peak</td><td class="score">2</td>
```

### Updating standings

After each game, update the standings table for that division:

1. **Game points:** Put `2` (win), `1` (tie), or `0` (loss) in the appropriate G1/G2/G3/G4 column for each team that played.
2. **PTS:** Update the total points (sum of all game points).
3. **GF:** Goals For — total goals the team has scored across all their games.
4. **GA:** Goals Against — total goals scored against them.
5. **Place:** Once all division games are done, fill in 1st, 2nd, 3rd, etc.

### Filling in the playoff bracket

Once division play is complete:

1. Replace placeholder names ("1st Place American", "2nd Place National", etc.) with the actual team names — both in the schedule table playoff rows AND in the playoff bracket boxes at the bottom.
2. After each playoff game, add the scores to the bracket boxes.
3. **Winner styling:** Add `winner` to the winning team's CSS class to highlight them:

```html
<!-- Before -->
<div class="matchup-team">Ice Services</div>

<!-- After (they won) -->
<div class="matchup-team winner">Ice Services</div>
```

This turns the winner's box dark navy with white text (or gold-bordered for the championship).

---

## CSS class reference

### Wrapper classes (keep these unique per skill level to avoid style conflicts)
| Class | Used in |
|-------|---------|
| `.awhl-header` | Division header pages (both Rec and Int) |
| `.awhl-bracket` | Rec tournament tracker |
| `.awhl-int` | Intermediate tournament tracker |

### Key style classes
| Class | What it does |
|-------|-------------|
| `.sched-table` | Game schedule table — fixed column widths tuned to fit 8 columns without scrolling |
| `.stand-table` | Division standings table |
| `.score` | Bold styling on score cells (used in both Rec and Intermediate trackers) |
| `.playoff-row` | Blue-tinted background on playoff game rows in the schedule |
| `.playoff-box` | Bordered box for each playoff matchup |
| `.champ-box` | Gold border for the championship game |
| `.consolation-box` | Grey border for the consolation game |
| `.matchup-team` | Team name box inside a playoff matchup |
| `.matchup-team.winner` | Dark navy highlight for the winning team |
| `.matchup-score` | Score display inside a playoff matchup |
| `.section-note` | Small italic text below section headers (e.g., "Win = 2 pts") |

### Mobile responsiveness
All pages include `@media` queries that kick in at **540px wide**. On small screens:
- Font sizes shrink
- The **Rink** column hides automatically in the schedule table
- Playoff boxes compact slightly

---

## Color scheme

The pages use a consistent navy/dark blue palette:

| Color | Hex | Usage |
|-------|-----|-------|
| Dark navy | `#1a1a2e` | Page title headers (`h2`) |
| Navy | `#16213e` | Section headers (`h3`), footer rows |
| Blue | `#0f3460` | Table headers, playoff box borders, winner highlight |
| Gold | `#d4af37` | Championship game border |
| Light blue tint | `#eef2ff` | Playoff rows in schedule |
| Light grey | `#f5f5f5` | Consolation game background |

If you want to change the color scheme, search-and-replace these hex values across all files.

---

## Tips and gotchas

- **Edit in Sports Engine for live updates.** During the tournament, edit the HTML code block by copy/pasting out of the website and into an editor like vsCode or Notepad++. You don't need to push to GitHub, rebuild, or redeploy anything. Commit your final version to GitHub after the tournament for archival.
- **Careful with copy-paste.** Sports Engine's HTML editor sometimes strips or modifies tags. If the formatting looks broken after pasting, check that the `<style>` block at the top survived intact.
- **Don't add `<html>`, `<head>`, or `<body>` tags.** These are HTML fragments meant to be embedded inside an existing page. Sports Engine provides the outer page wrapper.
- **Test on mobile.** Most people checking scores at the rink will be on their phones. The CSS is built to handle this, but always preview after pasting.
- **Each file is self-contained.** All CSS is in a `<style>` block at the top of each file. There are no external stylesheets or JavaScript dependencies.
- **No JavaScript.** Everything is plain HTML and CSS. Scores are updated by hand-editing the HTML. This keeps it simple and means it works everywhere Sports Engine renders HTML blocks.
- **Watch your `<br>` tags** in the division header files. Each team name is separated by `<br>` — don't accidentally delete one or you'll merge two team names together.
- **Keep old seasons.** Don't delete previous years' files. They're a useful reference for next year's setup and a nice historical record.

---

## Typical tournament week workflow

1. **Before the tournament:** Set up new season files following the steps above. Paste into Sports Engine pages. Test on desktop and mobile.
2. **During the tournament (Tue–Sat):** After each game, update scores in the schedule and standings directly in Sports Engine's HTML editor. No need to touch GitHub.
3. **After division play (Saturday night):** Calculate final standings, fill in playoff bracket team names.
4. **Playoff day (Sunday):** Update scores and add `winner` classes as games finish.
5. **After the tournament:** Copy the final HTML from Sports Engine back into the repo files and commit, so you have a clean archive of the completed bracket.

---

## File structure for future seasons

```
awhl-tournament-brackets/
├── README.md
├── 25-26-rec-tournament-brackets.html
├── 25-26-rec-tournament-tracker.html
├── 25-26-int-brackets.html
├── 25-26-int-tournament-tracker.html
├── 26-27-rec-tournament-brackets.html      (next season)
├── 26-27-rec-tournament-tracker.html
├── 26-27-int-brackets.html
├── 26-27-int-tournament-tracker.html
└── ...
```
