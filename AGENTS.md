# AGENTS.md: guide for AI agents and contributors

A collection of PlumCraft graphs: one per craft app in `graphs/crafts/<craft>/`, and graphs that cross engines in `graphs/crossings/`. The graphs are the product. There is no code to build: `scripts/graphs` wraps `plumcraft-editor check`, `run` and `tidy`.

## Rules

1. **Every graph must run.** "It loads" isn't enough: run it and look at what it wrote. Several mistakes here passed `check` and ran cleanly, and were only caught by looking at the output: a red cut line round a coaster, a chart off the page, a hue ramp through green.
2. **Fix gaps at source.** When a graph needs a workaround, the gap belongs in PlumCraft or the craft, which are the maintainer's repositories. Record it in `docs/findings.md` with the error text, and propose the fix there. Don't build machinery here to route round it: this project once had its own runner, and the right fix was in `plumcraft-editor`.
3. **Follow the conventions** in `docs/writing-graphs.md`: an `engines` list, `meta.title` and `meta.about`, Write Files into `out`, relative asset paths, Tidy.
4. **Credit what you add to `assets/`** in `assets/CREDITS.md`. No image searches on Wikimedia.

## Before you finish

```sh
scripts/graphs check                 # every graph against the live catalogues
scripts/graphs run                   # every graph; read out/REPORT.md, then look at the files
```

Then regenerate what depends on the outputs: `docs/images/` and `docs/catalogue.md`. The scripts that make them are scratch, in `claude-debug/`; see `claude-debug/NOTES.md`. Update `AI.md` (the `ai-md` skill) whenever you write a commit message. Commit only when asked.

## Scratch

Probes, scripts and test data go in `claude-debug/`. Only its `.md` notes are tracked. Don't delete what's there.
