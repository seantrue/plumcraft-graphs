# AI Contributions — PlumCraft graphs

*About AI slop.* Fair enough: nearly everything in this repository was written by a machine. Think of it as instant ramen. It's fast, it's cheap, it's genuinely edible, and nobody should pretend it simmered on the stove all afternoon. The honest answer isn't a claim about quality. It's the record below: what the maintainer asked for and decided, what the model built, and what the model got wrong, including the defects that only showed up when each graph was run and its output looked at, rather than when it merely loaded. Bon appétit, but mind the sodium content.

Tracks what AI helpers contributed to PlumCraft graphs versus the human maintainer.

## Human maintainer

The project was started from the maintainer's request, quoted below. The maintainer wrote PlumCraft, the craft apps and their MCP servers that these graphs run on, and the craft packs that expose them as nodes. The maintainer set the direction that moved the runner out of this project and into PlumCraft. There was no code here before the AI work.

## Contributions

### 2026-11-05 — First commit of the collection (Claude Opus 5.5)

**What the maintainer asked for:** "CP" (commit and push). Then: "Make sure that no secrets or personal data are commited", and "Uncommit all the data files, commentary, and graphs related to [the home server's] estate statys".

**What the AI did:** committed the work recorded below as the repository's first commit: 22 graphs, the docs and gallery, the assets, `scripts/graphs` and the `claude-debug/` notes. A 23rd graph, built from the maintainer's home-server status data, is kept out of the repository with its data, snapshot script, gallery picture and the documentation written for it. All of it stays on disk, untracked by `.gitignore`. Nothing was rebuilt or re-run for this commit.

**Defects in AI-written work, and how each was caught:**

- **A snapshot script that leaked the names it hid.** The script that made the server data generic listed every private domain, project and host name in its replacement table. The rclone logs inside the data also carried remote bucket paths. Caught by a scan for personal data that the maintainer asked for, before the first push.

### 2026-11-02 — The collection: 22 graphs, documentation and findings (Claude Opus 5.5)

**What the maintainer asked for:** "Create a set of realistic graphs that use each of the artcraft engines, and another set that cross multuple engine boundaries. Put this in a new project (../plumcraft-graphs) with appropriate documentation. Being actually useful, amusing, or entertaining are important. Organize the project in a sensible way."

Partway through, after seeing that the AI had written a custom Rust runner, the maintainer said: "If this needs a custom runner, it suggests that there is something missing from plumcraft base". That redirected the work. The runner's job (run a graph's Write Files nodes on the engines it names), its `check` and its `tidy` moved into `plumcraft-editor`'s headless mode, in the PlumCraft repository on branch `headless-write-files` (uncommitted; about 330 lines across `apps/plumcraft-editor/src/headless.rs`, `lib.rs`, its tests and docs). The runner was then deleted from this project, and `scripts/graphs` is a 69-line loop over those commands.

**What the AI did:**

- **Graphs (22 files, 645 nodes, 658 wires; the JSON is about 16,000 lines, pretty-printed and generated):** one graph per craft (13) in `graphs/crafts/`, and nine crossings of two to five engines in `graphs/crossings/`. Each was built by a scratch Python builder (`claude-debug/`, untracked), checked against live catalogues, run, and its output looked at, usually several times.
- **Research:** dumped every engine's catalogue (16 engines, 570 distinct node types) over HTTP to design against, and probed behaviour that the catalogues don't show: which Open nodes accept pictures, what `gc.read` returns, HSV hue units, DesignCraft's object ids.
- **Documentation (~600 lines):** `README.md`, `docs/catalogue.md` (generated from the graphs and a real run), `docs/writing-graphs.md`, `docs/findings.md`, `AGENTS.md`, `CLAUDE.md`, `assets/CREDITS.md`, and 22 gallery images from real outputs.
- **Assets:** the lawnmower terms (Markdown and PDF) and the plum badge SVG were written for the project. The plum photo is the CC BY 2.0 one PlumCraft already credits.
- **Findings (`docs/findings.md`):** two PlumCraft core gaps (`INT` ids can't feed `ids` inputs; `TXT`/`MD` aren't `TEXT`), two pack gaps (seven Open nodes offer an `image` input they reject; `select` filters offered where unsupported), and nine craft-app quirks, each with its error text and the workaround used. These are for the maintainer to fix at source; none were fixed in the crafts.

**Defects in AI-written work, and how each was caught:**

- **Wrong target: a custom runner.** About 450 lines of Rust (`plumgraph`) worked around a gap that belonged in PlumCraft. Caught by the maintainer's review; replaced as described above.
- **Unwired nodes that passed the first checker.** A "delete the white background" node was never connected, because a string replace in the builder missed. Then four query nodes (word count, document info, session inspect, sequence inspect) and two others (a hatch, a paper texture) were built but fed nothing, so they never ran. Caught by adding two checks (wire-only inputs with no wire; nodes no output depends on), which now live in `plumcraft-editor check`.
- **The laser file would have cut a square round the coaster.** CadCraft's SVG carries a white background rectangle, and the recolour turned it into a red cut line. Caught by looking at the PDF; fixed by deleting the background in VectorCraft.
- **Three colour mistakes, all caught by looking at the output.** The parts preview came out one flat colour twice: first because hairline cut lines leak between regions, then because two labels landed on the same hue. The album cover's hue ramp ran from plum to apricot the long way round, through green. The AI's first diagnosis of that, that the array engine's hue was in turns rather than degrees, was wrong; a probe showed degrees.
- **Layout mistakes caught by looking at the output:** a blank first slide and an invisible theme in the deck; `####` columns, and a chart running off the page, in three GridCraft PDFs; an equation that landed mid-sentence on the poster; text positioned off-frame in the lower third and the ident (wrong anchor assumed); black storyboard frames (the composition background isn't exported).
- **Wrong parameters, caught by the craft's own error on the first run:** FigCraft `fills` format, DesignCraft `fillColor` (should be `fill`) and swatch name `Paper` (should be `[Paper]`), CadCraft and FigCraft `select` forms they don't support, a variadic input wired without its slot number, and three graphs that sent pictures to Open nodes that don't accept them.
- **An overclaim in a description:** the shelf-pin graph first said "print the PDF at 100 %". The PDF is fitted to an A4 page, not 1:1. Caught by `pdfinfo`; the description now says the DWG is true scale.
- **One near-destructive command:** a scratch cleanup, `rm …/peek/*` after a `cd`, was blocked by Claude Code's safety check before it ran. Nothing was deleted.

**Verification:** on 2026-11-02 all 22 graphs passed `plumcraft-editor check` and ran to completion through `plumcraft-editor run` (from the `headless-write-files` branch, debug build), with every craft app on the maintainer's machine. The broken-telephone graph was also opened and run in the windowed editor (46 nodes, 4 engines). Running is not the same as being right: the outputs were judged by eye from renders, and the audio only by duration, format and note count, never by ear. The new PlumCraft tests (5 headless tests, 27 editor unit tests) pass, with clippy and fmt clean on that crate. The full PlumCraft workspace test suite was not run.

**Division of labor:** the maintainer asked for the collection and redirected the runner into PlumCraft; the AI designed, built, debugged and documented the graphs and wrote the PlumCraft change.
