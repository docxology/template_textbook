# Development and rendering

Use uv, Python 3.14, Pandoc with a matching `pandoc-crossref`, and XeLaTeX
with `soul` and the packages named in the [preamble](manuscript/preamble.md). Install
Linux Libertine, Linux Biolinum, and DejaVu Sans Mono for the configured
typography. Node 24, Mermaid CLI 12.2.0 (`mmdc`), and a Chromium-compatible browser
render the standalone and inline diagrams. Configure Puppeteer's browser
path if your installation does not discover it. Run the pinned renderer
without `NODE_PATH` or `NODE_OPTIONS` overrides.

Run this from a clean checkout named `template_textbook`. Use an unused path
for the external renderer clone.

```bash
TEXTBOOK_ROOT="$(pwd -P)"
TEMPLATE_ROOT="$TEXTBOOK_ROOT/../textbook-render-template"
git clone https://github.com/docxology/template.git "$TEMPLATE_ROOT"
git -C "$TEMPLATE_ROOT" switch --detach 87704feae0a4490ef7dad9c931d268e8c30273c2

export UV_PYTHON=3.14
uv sync --locked --extra dev
export TEMPLATE_TEXTBOOK_TEMPLATE_ROOT="$TEMPLATE_ROOT"
export PYTHONPATH="$TEMPLATE_ROOT${PYTHONPATH:+:$PYTHONPATH}"
uv run --no-sync python scripts/scaffold_chapter.py
uv run --no-sync python scripts/generate_figures.py
uv run --no-sync python scripts/generate_diagrams.py
uv run --no-sync python scripts/analysis.py
uv run --no-sync python scripts/audit_textbook_quality.py
uv run --no-sync python -m pytest tests/ --cov=src --cov-fail-under=90

# Route the pinned renderer's inline Mermaid assets to the project output.
ln -s ../output docs/output

cd "$TEMPLATE_ROOT"
uv sync --locked --no-dev --python 3.14
mkdir -p projects/working
ln -s "$TEXTBOOK_ROOT" projects/working/template_textbook
uv run --no-sync python scripts/pipeline/stage_03_render.py --project working/template_textbook --skip-manuscript-hydration
uv run --no-sync python scripts/pipeline/stage_04_validate.py --project working/template_textbook
```

The scaffold command leaves existing chapters, labs, and question banks in
place. The structural audit accepts the exemplar's marked stubs. A filled fork
can use `scripts/audit_textbook_quality.py --require-complete` to require zero
reported stubs. The modelling script writes `output/data/worked_model_summary.json`.
Figures and diagrams go to `output/figures/`.

The temporary `docs/output` link routes inline Mermaid artifacts to the
project output and can be removed after validation.

The project's `output/` receives the combined PDF, HTML, EPUB, and DOCX in
the `pdf/`, `web/`, `epub/`, and `docx/` subdirectories. The
`reports/` directory contains the validation and provenance receipts.
The [publication proofs and retained build](../rendered/README.md) are historical
artifacts.
