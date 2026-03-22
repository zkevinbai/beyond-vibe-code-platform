# Beyond Vibe Code (BVC)

A single-page learning platform for software engineering fundamentals. The entire app is one static file: **HTML + CSS + JavaScript** in `index.html`—no build step, no npm install, no backend.

**Live site:** [https://zkevinbai.github.io/beyond-vibe-code-platform/](https://zkevinbai.github.io/beyond-vibe-code-platform/) (GitHub Pages)

## What this project is

BVC is a **curriculum browser**: five tracks (Foundation, Web Engineering, Systems, Professional Engineering, Capstone), **35 modules**, each with a sequence of lessons. Learners move through prose lessons, interactive visualizations, fill-in-the-blank exercises, multiple-choice quizzes, and optional in-browser coding drills. Progress (which modules are marked complete) is stored in the browser with `localStorage`.

For a deeper look at architecture, data shapes, and how rendering works, see [`SYSTEM_DESIGN.md`](SYSTEM_DESIGN.md).

## Run locally

Any static file server works. From this directory:

```bash
python3 -m http.server 8765
```

Then open `http://127.0.0.1:8765/` (GitHub Pages expects `index.html` at the site root).

You can also open `index.html` directly in a browser for a quick read-only view; some behaviors are nicer over HTTP (clipboard, consistent origins).

## Repository layout

| Path | Role |
|------|------|
| `index.html` | All UI, styles, curriculum data (`CURRICULUM`), and application logic in one file |
| `.gitignore` | Ignores OS junk (e.g. `.DS_Store`) |

## Editing content

1. **Curriculum** lives in the `CURRICULUM` constant near the top of the `<script>` block in `index.html`. Structure is: **tracks → modules → lessons**.
2. **Lesson types** (see `SYSTEM_DESIGN.md` for fields):
   - `lesson` — HTML string in `content` (can include `id` hooks for visualizations).
   - `fitb` — fill-in-the-blank with `fitb.prompt`, `fitb.code`, and `fitb.blanks`.
   - `quiz` — `questions[]` with stem, `opts`, correct `answer` index, and `explain`.
   - `editor` — `instructions`, `starterCode`; “Run” executes code in a sandboxed `new Function` (not a full Node runtime).
3. **New interactive visuals** need matching DOM in the lesson HTML *and* wiring in `initVizForLesson()` (currently keyed by **lesson title substrings**).

After edits, reload the page. For GitHub Pages, commit and push to `main`; the site updates in a minute or two.

## Progress storage

Completed module IDs are saved under the key **`bvc_completed`** in `localStorage` (JSON array of numeric module IDs). Clearing site data for the origin resets progress.

## License / usage

Add a license file if you want to specify terms; the repo currently ships as application source only.
