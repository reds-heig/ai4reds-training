---
name: one-page-teaching-ui
description: Build a single self-contained offline HTML page that teaches a paper (paired with its code) or an unfamiliar repo, in the institute's reference visual style. Use when asked to "build a UI that teaches X" from local sources, or to turn a paper + code repo into a one-page walkthrough.
---

# One-page teaching UI

Use this when the user wants to learn a local paper, a paper plus its code
repo, or an unfamiliar repo, and the deliverable is one HTML page. The page
teaches: it pairs every key claim with its evidence (figure, equation, code)
and a short note on how the two connect. The audience is a competent adult
who knows nothing about the topic yet.

Hard rules: work offline (no CDN, no external fetches at render time), never
modify the source material, write only to the requested output path, answer
only with verified information — every fact gets a named source — and follow
the Visual spec below. Do not invent a theme.

## Goals

- **Teach, don't display.** Each key figure and key equation gets the matching
  code from the repo (or, for a repo-only task, the matching docs/commands) and
  a note explaining the connection. A figure with no "so what" is incomplete.
- **Verifiable offline.** Figures are inlined (base64 data URI, or raw SVG for
  small ones). The page renders with no network. Citation links in the prose
  may name URLs, but nothing loads them.
- **Zero invention.** Every fact is traceable to a local source: paper section,
  table, figure caption, or repo file (with line numbers for code). Where the
  paper and the repo disagree, state the difference on the page instead of
  papering over it.
- **Read-only sources.** The paper, the repos, and pre-saved assets are input.
  The only write is the requested output file(s) under the requested directory.
- **One page, digestible.** Layout and look follow the Visual spec. For a
  paper: motivation → roadmap → one section per technique (figure + equation
  + code + connection note) → training/usage recipe → results (including
  negative results) → how to run → sources footer. For an unfamiliar repo:
  what it is → how it works (use the project's own diagram if it has one) →
  what it can do → how to use it (real commands) → what it returns → limits
  and rules.

## Visual spec (fixed: match `demo/rea/index.html`)

The page must look like the REA intro page. Use its design system, values
and all. Do not pick a new palette, a new font, or a new layout.

Palette — define as CSS variables on `:root`:
- `--bg:#0b0e14` (page), `--bg-soft:#11151f`, `--bg-card:#151b27`,
  `--line:#232b3b`.
- `--text:#e6ebf4`, `--muted:#93a0b5`.
- `--accent:#e8483f`, `--accent-soft:#ff6b5e`.
- Secondary colors: `--green:#4ecb71`, `--blue:#58a6ff`, `--amber:#e3b341`.
- Monospace: `"SFMono-Regular","JetBrains Mono",Consolas,"Liberation
  Mono",Menlo,monospace`.
- Accent appears only on: section labels, dots, step badges, the kicker,
  the primary button, and links that the page highlights. The rest of the
  page stays neutral dark.

Header — centered, on `--bg-soft`, a red radial glow top-left, 1px
`--line` border below, 72px top / 56px bottom padding:
1. Kicker: monospace, 13px, uppercase, letter-spacing .18em,
   `--accent-soft` (for example `WORKSHOP · INTRO LEVEL`).
2. `h1`: `clamp(34px, 6vw, 56px)`, letter-spacing -.02em. The name is
   bold; the rest of the title may use font-weight 300 in `--muted`.
3. Tagline: one sentence, 18px, `--muted`, max 640px, centered.
4. Badge row: monospace 12px pills, 1px `--line` border, `--bg-card`
   background, 8px gap.
5. Link row: 1–3 buttons, monospace 13px, 8px radius. The first button is
   filled `--accent`.

Body — max-width 1000px, centered, 24px side padding:
- One `section` per topic: 64px vertical padding, 1px `--line` border below.
- Section label: monospace 12px, uppercase, letter-spacing .16em,
  `--accent-soft`, numbered (for example `1 · WHAT IT IS`).
- `h2`: 30px. Lead paragraph: 17px, `--muted`, max 720px.
- Cards: `--bg-card`, 1px `--line` border, 12px radius, 20px padding.
  Title 16px with a 9px colored dot (red, green, blue or amber). Card text
  14.5px in `--muted`; emphasized words in `--text`.
- Card grid: `auto-fit`, `minmax(250px, 1fr)` or `minmax(320px, 1fr)`,
  16px gap. Two or three columns only when every column has content.
- Steps: a numbered `01` badge (monospace, `--accent-soft` on a 12% red
  background) above each step card.
- Callouts: 3px left border in `--green` on a 6% green background.
- Code: 1px `--line` border, 10px radius, background `#0d1119`, monospace
  13.5px. A command gets a title line and a copy button at the top right.
- Prefer cards over tables.

Figures — this is where pages break:
- Every figure sits in a `.fig` box with 30px top margin.
- SVG: `width:100%; height:auto; display:block`.
- Screenshots (raster): `max-width:100%; height:auto; display:block`. Never
  wider than the container, never cropped by the layout. A very wide or
  very tall screenshot is cropped to the informative part before it is
  embedded, and the caption names the crop.
- Every figure gets a caption: 12.5px, `#5b6878`, what it shows plus the
  source.
- A figure sits next to the text it explains. Never stack figures at the
  end of the page.

Placement — no exceptions:
- Spacing scale: 64px between sections, 30px before a figure or a grid,
  16px between cards, 12–14px between blocks inside a card. Do not invent
  new spacings.
- One block, one job: a title, a lead, a card, a figure, a code block, a
  note. If a block does two things, split it.
- Footer: centered, `--muted`, 14px; a monospace link row; a 12px
  fine-print source line.

## Routine

1. **Read the target fully before writing.** For an HTML paper, extract the
   text (strip tags) and read the whole body plus the appendices that hold
   ablation detail. For a repo, read the README, the manifest
   (`package.json`, setup scripts), the entry points, and the docs the README
   points at. Note the list of figures and where they live on disk.
2. **Map figure files to figure numbers by caption, not by filename.** Pre-saved
   figure files are often named differently from the paper's figure numbers
   (here: `6_head_sweep_pdf.svg` is Figure 5, `5_layer_share_setting_pdf.png`
   is Figure 6). Confirm against the `<figure>`/`<object>` tags in the paper
   HTML.
3. **List the teaching units first.** Key figures (with their exact captions),
   key equations (the paper's are often inline), key claims and tables, and
   how to use it. This list is the page's table of contents.
4. **For each unit, find the matching code.** Grep the repo for the concept
   (flag names, class names, config keys), read the exact function, and record
   `file + line range`. Verify line numbers with `sed -n` before quoting. If a
   claim has no code in the repo (e.g. an experiment that wasn't released),
   say so explicitly on the page.
5. **Generate the HTML with a script; never hand-paste big assets.** A small
   Python script that (a) base64-inlines the figures, (b) extracts code
   verbatim by line range from the source files and HTML-escapes it with
   line numbers, and (c) fills a template with the cards. The script keeps the
   code quotes byte-faithful and makes rebuilds cheap when you fix a note.
6. **Keep code snippets short and pointed.** One function, one loop, one flag
   block — the minimal span that *is* the equation. A 330-line file is never
   the right snippet.
7. **Validate, then report.** Run the checks below. Then report: the output
   path(s), the section names of the page (and of the skill, if one was asked
   for), and any problems hit.

## Checks (all must pass before "done")

- **Well-formed HTML.** Parse with a strict tag-stack parser (Python
  `html.parser`): no unclosed tags, no mismatches.
- **Figures present and inlined.** Count data-URI/inline images; it must equal
  the number of figures promised. No `src` pointing outside the page.
- **Code is byte-faithful.** Re-extract each rendered snippet from the output,
  unescape it, and diff against the source file at the displayed line numbers.
  (Watch for a trailing-newline artifact in your diff regex — that is a check
  bug, not a content bug.)
- **Line numbers are 1-based and contiguous** in every numbered block, and
  match the range named in the block title.
- **Every fact has a source line.** Each card ends with a `Sources:` line
  (paper §/table/figure, repo file + lines, docs page). No orphan claims.
- **In-page links resolve.** Every `href="#x"` has a matching `id="x"`.
- **Sources untouched.** `git status --porcelain` on every source repo is
  empty (or unchanged from before); the pre-saved assets directory is
  unmodified.
- **Output confined.** The only new files are under the requested output
  directory.
- **Matches the visual spec.** Screenshot the page at 1280x900 (headless
  browser if one is available). Check against the spec: centered header with
  kicker, `h1`, tagline and badges; numbered section labels; cards on
  `#151b27` with 12px radius; `#e8483f` only where the spec allows it. No
  horizontal scroll at 1280px. Every figure fits its container and has a
  caption. Nothing overlaps, and every card keeps its padding. If no browser
  is available, check the CSS instead: the `:root` palette above is present,
  every `img` has `max-width:100%`, and no container has a fixed width above
  1000px.

## Pitfalls (learned the hard way)

- **0-based vs 1-based line numbers.** If you build snippets from
  `range(start - 1, end)`, the *content* is right but the printed numbers are
  off by one. Print `i + 1`. The diff check above catches this.
- **Template collisions.** Triple-quoted strings inside a Python f-string
  template terminate the template. Build repeated blocks (command cards, code
  blocks) as variables *before* the big f-string, then interpolate.
- **Apostrophes in prose strings.** ASCII `'` inside a single-quoted Python
  string breaks the script; use the typographic `'` in rendered prose.
- **Figure filenames ≠ figure numbers.** Always confirm by caption.
- **Config flags ≠ paper terminology.** A released `config.json` may use
  `tie_word_embeddings` while the training code implements the paper's feature
  under its own flag (here: `share_embedding`, a CLI flag wired in
  `pretrain.py`, default `True`). Read both sides, then state the difference.
- **Script defaults can contradict the paper's main model.** Read the shipped
  scripts end to end (`pretrain.sh`, `eval.sh`) — here `eval.sh` explicitly
  passes `--layer_sharing False` while the args parser defaults it to `True`.
- **Learning-rate and hardware claims need the README's scaling note.**
  The paper's 2e-3 LR was for 32 GPUs; the shipped single-node script uses
  5e-4 and the README says scale linearly. Show the arithmetic, don't hide it.
- **Negative results are teaching material.** Include the KD experiment that
  didn't help, and the "repo does not contain the fine-tuning/quantization
  code" boundary — it tells the learner exactly what the code can prove.
- **Literal `%` in prose breaks Python `%` formatting.** Benchmark text is
  full of percent signs. Build the template with markers and `replace`, not
  with `%` formatting.
- **HTML inside a data-URI breaks the attribute.** If an inlined asset
  contains an HTML snippet (an `<img>` tag inside an SVG), the outer `src`
  attribute ends early and the image renders broken. Embed the raw SVG
  markup, or base64-encode the file bytes. Check: no `<` character inside
  any attribute value.
- **Screenshots at native size break the layout.** A 1500px screenshot in a
  1000px column overflows or dominates the page. Crop to the informative
  part first, then constrain to the container width.
