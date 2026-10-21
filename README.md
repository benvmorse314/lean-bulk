# Lean Bulk

Personal project - NOT a client/MikeTeeVee job. Sibling app to
[Yardage Book](https://benvmorse314.github.io/yardage-book/) and
[Ground Force](https://benvmorse314.github.io/ground-force/).

Whole-food meal plan for a gentle lean bulk. Single-file, self-contained web
app (vanilla HTML/CSS/JS, no build step, no dependencies) plus a thin PWA layer
so it installs to a phone home screen and works offline.

- **Working folder:** E:\lean-bulk
- **Live URL:** https://benvmorse314.github.io/lean-bulk/

## What it does

- **TODAY** - the day's two meals, afternoon smoothie, evening bowl and fruit as
  a checklist, with live calorie and protein bars, tonight's thaw, prep tasks.
- **WEEK** - all seven days at a glance, targets, what was bought.
- **RECIPES** - short how-tos; every meal links to its recipes.
- **WEIGH** - morning weigh-ins, 7-day average, trend chart, and a two-week
  add/trim/hold calorie verdict. COPY FOR CLAUDE exports the log as text.

## Dev

No build. Serve the folder with any static server and open index.html:

```
python -m http.server 8126
```

Deploy = push to `main`; GitHub Pages serves main/root.
All user data lives in localStorage (`lb-data-v1`) - schema changes must
migrate forward, never wipe.
