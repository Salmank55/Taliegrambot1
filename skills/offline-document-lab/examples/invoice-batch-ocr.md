# Example: Invoice batch OCR

Use this as a local, evidence-preserving batch plan. It does not treat OCR output as automatically correct.

## Job objective

Extract invoice number, date, supplier, line items, subtotal, tax, and total from a folder of mixed text PDFs and scanned PDFs. Produce searchable text plus a review report without uploading documents.

## Intake manifest

| Field | Example |
| --- | --- |
| Job ID | `invoice-2026-001` |
| Source | `originals/invoices/` |
| Output | `outputs/searchable/` and `outputs/tables/` |
| Review | Totals, dates, identifiers, and low-text pages |
| Retention | Delete temporary renders after output verification |

## Processing path

1. Copy files to `originals/` and compute a checksum when practical.
2. Detect whether each PDF already contains text.
3. Extract native text first; preserve page markers.
4. Render only image-only or incomplete pages for local OCR.
5. Export a table with original number strings before numeric conversion.
6. Compare visible totals with extracted totals on every invoice.
7. Mark unreadable or ambiguous fields for manual review.
8. Keep the original PDF, extracted text, table, and manifest linked by source name.

## QA matrix

| Check | Pass condition |
| --- | --- |
| Completeness | Every source file appears in success, review, or failed status |
| Fidelity | Invoice numbers, dates, and totals match the visible source |
| Page traceability | Each extracted value has a page reference |
| OCR quality | Pages with sparse or suspicious text are flagged |
| Privacy | No remote OCR call and no raw invoice body in logs |
| Visual output | Searchable PDF has no blank, clipped, or reordered page |

## Delivery bundle

Provide the manifest, extracted machine-readable table, searchable derivatives, review queue, and a short report of tools, transformations, failures, and known OCR errors. Never overwrite the originals.
