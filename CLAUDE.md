# Training Health dashboard

A template for a Claude **Data** artifact (dashboard page on claude.ai) that combines Strava training,
Oura Ring recovery (through the Freddy - Oura connector) and a training plan. See README.md for setup.

## Layout
- `index.html`: the page. Script 1 = loaders + calcs (every number), script 2 = `window.HB`
  (thresholds `T`, status rules, chart helpers), script 3 = one `dash.draw` per `data-draw` element.
  Text that doesn't depend on data lives in the HTML; scripts only fill `data-draw` elements.
- `datasets/*.json`: source definitions (`datasets/<id>` docs). `_dash_meta.json` is `dash/meta`.
- `scheduled-task.md`: daily prompt that saves Freddy's last 7 days into the `oura_history` file.
- `samples/`: example plan CSV and empty history. `test/`: offline harness with synthetic data.

## Working rules
- Run the tests after any change to `index.html`:
  `npm install --prefix test d3@7.9.0 playwright`, then `python3 test/build.py && node test/run.js`
  (expect `NO ERRORS`; also try scenarios `race_passed`, `no_focus`, `freddy_error`, `no_oura`).
- Keep thresholds in `T` only; any text that mentions one is drawn from `T`.
- Freddy answers `query_metrics` in plain text; the `freddy_oura` loader and `scheduled-task.md`
  must parse it the same way. Change both together.
- This repo is public. Never commit personal data: no real activities, plans, Oura history,
  dashboard links or names. Files named `my-*` are git-ignored for local copies.
