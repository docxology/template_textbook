# Appendices

End-matter reference material for the textbook. These files are ordinary
markdown chapters that carry an `{#sec:...}` label and follow the same
crossref/citation/glossary conventions as the main chapters.

## What lives here

| File | Label | Purpose |
| --- | --- | --- |
| [`appendix_authoring_guide.md`](appendix_authoring_guide.md) | `sec:appendix_authoring_guide` | How to fill the stubs and grow the book — **read first** |
| [`appendix_notation.md`](appendix_notation.md) | `sec:appendix_notation` | Symbol table for the worked models |
| [`appendix_math_review.md`](appendix_math_review.md) | `sec:appendix_math_review` | Just-enough math, tied to `src/textbook/models.py` |
| [`appendix_formalisms.md`](appendix_formalisms.md) | `sec:appendix_formalisms` | Definitions, theorems, and algorithms |
| [`appendix_format_gallery.md`](appendix_format_gallery.md) | `sec:appendix_format_gallery` | Examples of supported manuscript formats |
| [Master Glossary](../glossary.md#master-glossary) | `master-glossary` | Authoritative term definitions |
| [`appendix_index.md`](appendix_index.md) | `sec:appendix_index` | Index of key terms (placeholder) |

## Ordering is data-driven

The order and display titles come from `appendices.reference` in
[`../config.yaml`](../config.yaml).

To add, remove, or reorder an appendix, edit `appendices.reference` in
`config.yaml` — never renumber by hand. Toggle whole groups with
`include_reference`, `include_labs`, and `include_questions`.

See the sibling READMEs for the [labs convention](../labs/README.md) and the
[question-bank convention](../questions/README.md).
