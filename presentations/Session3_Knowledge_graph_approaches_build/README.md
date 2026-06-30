# Session 3 — Knowledge graph approaches for linking datasets to uncover new biological insights

ECCB 2026, Tutorial 12. Slides for the 15:30–16:10 session, presented by Dr. Carissa Bleker.

## Contents

```
Session3_Knowledge_graph_approaches.md     source slides (Marp markdown)
Session3_Knowledge_graph_approaches.pdf    rendered, presentation-ready PDF
Session3_Knowledge_graph_approaches.html   rendered, interactive HTML version
images/                                    diagrams used in the slides
```

## Files

** `.md` ** -- the editable source. Edit this, then re-render to update the `.pdf`/`.html` (see below).

** `.pdf` ** -- a single self-contained file of the presentation.

** `.html` ** -- presenter-mode features (click-through fragments, speaker notes, slide overview) in a browser. *Not* a single file — it loads images from the `images/` folder.

## Editing the slides

The `.md` file is plain [Marp](https://marp.app/) markdown. Slides are separated by `---`, with a small YAML header at the top.

Images are referenced with relative paths (e.g. `![w:600](images/neo4j.png)`), so new images need to go in `images/` alongside the markdown.

## Re-rendering after edits

Rendering requires [Marp CLI](https://github.com/marp-team/marp-cli) and a Chromium/Chrome/Edge/Firefox install (Marp uses it headlessly to produce PDF/PPTX output; not needed for editing the markdown itself).

For PDF:
```bash
npx @marp-team/marp-cli@latest Session3_Knowledge_graph_approaches.md --pdf --allow-local-files
```

For HTML:
```bash
npx @marp-team/marp-cli@latest Session3_Knowledge_graph_approaches.md --html --allow-local-files
```

If Marp can't find a browser automatically, point it at one explicitly:

```bash
npx @marp-team/marp-cli@latest Session3_Knowledge_graph_approaches.md --pdf --allow-local-files \
  --browser chrome --browser-path /path/to/chrome
```

## Attribution

Some diagrams and slides are adapted from the [BioCypher Workshop](https://github.com/ssciwr/slides-biocypher) materials (Heidelberg, June 2026), licensed [CC-BY 4.0](https://ssciwr.github.io/slides-biocypher/LICENSE). Examples on those slides have been changed to plant-science-specific ones for this tutorial.
