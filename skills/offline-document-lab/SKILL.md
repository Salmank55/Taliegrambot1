---
name: offline-document-lab
description: Extract, clean, inspect, transform, and quality-check PDFs, scans, images, office files, and tabular documents using local tools. Use for offline OCR, document conversion, batch extraction, searchable archives, redaction preparation, and evidence-preserving document workflows.
---

# Offline Document Lab

Process documents locally while preserving the original, tracking transformations, and separating extracted evidence from interpretation. Prefer a transparent imperfect result over a polished file whose OCR or table values were silently guessed.

## Establish an evidence-preserving intake

Create a job directory with four areas: `originals/`, `working/`, `outputs/`, and `logs/`. Copy or reference the source without modifying it. Record filename, byte size, file type, page or row count when available, checksum when practical, and the intended output.

Classify each input before choosing a tool:

| Input | First action | Main risk |
| --- | --- | --- |
| Text PDF | Extract text and inspect layout | Reading order or missing columns |
| Scanned PDF | Render pages and OCR locally | Character substitutions and missed regions |
| Image | Inspect dimensions, orientation, and quality | Tiny text, skew, glare, or compression |
| Office document | Extract text and structure, then render a sample | Hidden formatting and floating objects |
| Spreadsheet/CSV | Detect encoding, delimiter, headers, and types | Silent type coercion and shifted columns |
| Mixed bundle | Process per file type and join by manifest | Inconsistent naming and duplicated content |

Keep a manifest that maps every output to its source and processing step. Never overwrite an original with an OCR, crop, conversion, or redacted derivative.

## Choose the least destructive extraction path

1. Inspect metadata and page dimensions.
2. Attempt native text extraction when text is present.
3. Render only the pages or regions that need visual inspection.
4. Apply OCR only to image-only or incomplete regions.
5. Preserve page, paragraph, table, row, or cell coordinates where the tool provides them.
6. Normalize whitespace and encoding without changing substantive characters.
7. Store uncertain or low-quality segments in a review file.
8. Export the requested format and keep a machine-readable intermediate when useful.

Do not infer missing words from context without marking the inference. If a scan is unreadable, report the page and region instead of inventing content.

## Make OCR measurable

Before batch OCR, inspect a representative page for skew, contrast, orientation, handwriting, multi-column layout, stamps, tables, and tiny type. Test settings on that page and compare the result visually with the source. Keep the source image, OCR text, and any searchable PDF as separate artifacts.

Use page-level confidence or review flags when the chosen OCR engine exposes them. At minimum, flag pages with very little extracted text, unusually high symbol frequency, broken word patterns, or suspiciously repeated characters. Human-review pages containing names, numbers, dates, totals, addresses, or legal clauses.

## Treat tables as structured evidence

Detect headers, merged cells, repeated page headers, footnotes, and continuation rows before exporting a table. Preserve empty cells and original number strings until validation is complete. Do not convert currency, dates, percentages, or identifiers into numeric types without documenting the rule and checking for leading zeros.

For each extracted table, record the page range, column mapping, unit or currency, row count before and after cleaning, and cells that required manual review. Compare totals or row counts against the visible source when the document provides a control value.

## Create safe transformations

Use deterministic transformations for rotation, cropping, format conversion, page ordering, filename normalization, and whitespace cleanup. Make redaction irreversible only on a deliberate output copy, and visually inspect the final pages for hidden text, metadata, annotations, or unredacted duplicates. Never claim a document is legally redacted or compliant without the required domain review.

When merging files, preserve source order and add a manifest. When splitting files, use stable names containing the source stem and page range. When producing a summary, keep it separate from the extracted evidence and label model-generated interpretation as interpretation.

## Work offline and protect privacy

Prefer local utilities and already-installed libraries. Before using a new package, check whether it is available locally and whether the task can be completed with a simpler tool. Never upload a document, use a remote OCR endpoint, or enable telemetry without explicit authorization. Avoid logging raw document content; log filenames, step names, counts, and errors instead.

If temporary files contain personal or confidential data, place them in the job directory, set an explicit retention period, and delete them only after the user confirms the outputs are usable. Do not commit originals, extracted personal data, or credentials to a repository.

## Validate every deliverable

Run both machine checks and visual spot checks:

| Check | What to verify |
| --- | --- |
| Completeness | Expected pages, files, rows, and sections are present |
| Fidelity | Names, dates, numbers, totals, and symbols match the source |
| Order | Pages, columns, and reading order are correct |
| Encoding | No replacement characters or unexpected mojibake |
| Searchability | Text can be found where a searchable output is promised |
| Layout | A sample render has no clipping, overlap, or blank-page surprise |
| Traceability | Every output maps to an input and recorded transformation |
| Privacy | No unintended uploads, secrets, or source files in the output bundle |

For batch work, report success, review-needed, and failed items separately. Never report a 100% success rate when files were skipped.

## Report limitations clearly

Deliver a concise processing report containing the input manifest, tools and versions if known, transformations, output paths, review flags, known extraction errors, and a suggested manual review order. Include representative before/after samples only when they do not expose sensitive content.

## Useful references

- PDF Association, “PDF technology and resources”: https://www.pdfa.org/
- Library of Congress, “PDF/A”: https://www.loc.gov/preservation/digital/formats/fdd/fdd000318.shtml
- Tesseract OCR documentation: https://tesseract-ocr.github.io/tessdoc/
- Python, “csv — CSV File Reading and Writing”: https://docs.python.org/3/library/csv.html

## Practical example

Apply this skill to the included **Invoice batch OCR** example in `examples/invoice-batch-ocr.md`. Start a new task by copying `templates/document-job-manifest.csv` and use `assets/demo.gif` as a quick visual summary of the workflow.
