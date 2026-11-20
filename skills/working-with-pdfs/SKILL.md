---
name: working-with-pdfs
description: Use whenever a task requires reading, searching, summarizing, comparing, quoting, or extracting content or assets from any PDF.
---

# Working with .pdf files

## Purpose

Define how to work with `.pdf` files. **Never read a PDF directly** (no raw file reads, no naive text extraction). Always convert it with `docling` first, then work with the produced Markdown and extracted assets.

## Rules

1. **Docling first, always.** Before reading, analyzing, summarizing, or answering questions about any `.pdf` file, run the docling conversion below. If a `<pdf-name>-processed/` directory with a valid `pdf-name.md` already exists next to the PDF, skip conversion and use it.
2. **Output location.** All processed output goes into a directory named `<pdf-name>-processed` placed **next to the .pdf file** (where `<pdf-name>` is the PDF filename without the `.pdf` extension).
   - `report.pdf` → `report-processed/report.md`
3. **Read the Markdown, not the PDF.** After conversion, read `<pdf-name>-processed/<pdf-name>.md` (and referenced images when needed). Do not open the original `.pdf` unless conversion fails. You may also read created assets if needed.

## Commands (via uv)

Run docling as an ephemeral tool — no project dependencies and no global install needed. Requires Python 3.10+ and [uv](https://docs.astral.sh/uv/).

### Standard conversion

```bash
PDF="report.pdf"                     # path to the PDF file
OUT_DIR="${PDF%.*}-processed"       # strips ".pdf", appends "-processed": report-processed
MD="${OUT_DIR}/$(basename "${PDF%.*}").md"   # report-processed/report.md

uvx --from docling docling \
  --to md \
  --image-export-mode referenced \
  --table-mode accurate \
  --device cpu \
  --output "$OUT_DIR" \
  "$PDF"
```
