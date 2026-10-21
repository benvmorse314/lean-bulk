# Lean Bulk - project brain

Ben's whole-food lean-bulk meal plan app. Third sibling of Yardage Book
(`E:\yardage-book`) and Ground Force (`G:\ground-force`). Personal project, NOT a
MikeTeeVee client job - keep it off the vault's 30 Projects rails.

## Facts

- **Working folder:** E:\lean-bulk
- **Live site:** https://benvmorse314.github.io/lean-bulk/ (GitHub Pages, main
  branch, root folder). Repo: github.com/benvmorse314/lean-bulk (PUBLIC - never
  commit body weight, address, order numbers or anything else personal; Ben's
  data lives only in his phone's localStorage)
- **Profile + preferences:** Claude memory `ben-nutrition-meal-plan` (targets,
  likes/dislikes, 2 meals + no breakfast, no oats, groceries via Walmart+).

## Architecture

Single self-contained `index.html` + PWA sidecars (`manifest.webmanifest`,
`sw.js`, `icons/`, `fonts/`). Same skeleton, palette and fonts as the siblings
(Big Shoulders Display, Archivo, IBM Plex Mono - self-hosted OFL woff2).
Accent is Melon (#E5566E) with Rind green (#3BC77E) for protein / positive, so
the three apps read as siblings, not twins.

- Rendering: innerHTML string templates per tab; event delegation via
  `data-act` on `document.body` (`handleTap`).
- Views: TODAY (cluster + day strip + checklist + thaw/prep/tomorrow), WEEK,
  RECIPES (`<details>`; a meal's recipe chip opens and flashes its card),
  WEIGH (entry, stats, SVG trend, verdict, data tools).
- Plan data is CODE: `RECIPES`, `MEALS`, `DAILY`, `PLAN` near the top of the
  script. A new week = new `PLAN.week` + `PLAN.days` (and any new meals /
  recipes), then bump the version. Calorie / protein figures are estimates for
  the listed portions.
- State: object `S` in localStorage **`lb-data-v1`**:
  `{v, meta:{rev, lastBackup}, days:{'YYYY-MM-DD':{done:{key:true}, w?:lb}}}`.
  Check keys: `m1`, `smoothie`, `m2`, `bowl`, `fruit`, `thaw`, `prep0..n`.
  Day records are keyed by DATE, so past weeks' checkmarks and weights survive
  a plan swap. `normalize()` validates every load/import; an unreadable blob is
  stashed to `lb-data-v1-corrupt` before anything can overwrite it.
- Verdict: week-1 vs week-2 average (3+ weigh-ins each): < +0.2 lb -> add ~200
  kcal, > +1.0 lb -> trim ~200, else hold.
- COPY FOR CLAUDE: plain-text log of weigh-ins + ticked meals, so Ben can paste
  it into a chat when planning the next week (Claude cannot read localStorage).

## Rules (same as the siblings)

- ASCII-only source; non-ASCII as \uXXXX in JS or HTML entities.
- Never wipe stored data; schema changes migrate forward in `normalize()`.
- **Every deploy bumps `VER` in sw.js AND `APP_VER` in index.html together**
  (`VER = 'lb-v' + APP_VER`). The worker deletes only `lb-` caches - the origin
  is shared with Yardage Book (`yb-`) and Ground Force.
- Test service-worker behavior in Claude in Chrome, never the built-in pane.
- NEVER tell Ben to delete the home-screen app to "reinstall" - that deletes
  his data. Updates arrive on their own (foreground check + reload).
