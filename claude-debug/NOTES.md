# Scratch notes

Untracked helpers used to build this collection. Only this file is tracked.

- `gb.py`: a tiny graph builder (`G(title, about, engines).n(...).w("a.port", "b.port").write(...)`). It keeps the positions of nodes that already exist in the target file, so a rebuilt graph keeps its Tidy layout.
- `crafts_a.py`, `crafts_b.py`, `crafts_c.py`: build the 13 graphs in `graphs/crafts/`.
- `crossings_a.py` … `crossings_d.py`: build the 9 graphs in `graphs/crossings/`.
- `make_gallery.sh`: regenerates `docs/images/` from a run's `out/`.
- `make_catalogue.py [OUTDIR]`: regenerates `docs/catalogue.md` from the graphs' `meta` and a run's files.
- `probe*.py`, `p_*.json`: one-off probes of craft behaviour (what `gc.read` returns, which Open nodes accept images, HSV hue units). The findings went to `docs/findings.md`.
- `dump_schemas.py`, `sigs.py` (in PlumCraft's `claude-debug/graphs-project/`): every engine's catalogue as JSON and compact signatures, used to design the graphs.

The JSON files under `graphs/` are the source of truth. Edit them in the editor, or rebuild them from the builder scripts (which overwrites any hand edits).
