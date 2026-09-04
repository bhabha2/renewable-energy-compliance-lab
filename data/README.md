# Data folder

This folder makes the lab data layout explicit for replication.

The reference implementation processed source documents from SharePoint and wrote extracted images back to SharePoint. Those tenant files are not bundled directly in this public repo. To reproduce the lab, place your own renewable-energy permitting, zoning, interconnection, and contracting documents in `source-documents/`, or upload them to a SharePoint document library and point the scripts at that library's drive ID.

## Folder layout

| Path | Purpose |
| --- | --- |
| `source-documents/` | Put PDF, DOCX, and PPTX files here if you want a local staging folder before uploading to SharePoint. |
| `extracted-images/` | Optional local staging folder for visual assets. The production flow writes extracted images to SharePoint instead. |
| `reference-corpus.csv` | The corpus used in the reference run, listed as filenames and document roles. |
| `sample-questions.json` | Lab prompts that exercise text retrieval, visual retrieval, and Foundry reasoning. |

## Reference run counts

| Asset | Count |
| --- | ---: |
| Source documents | 15 |
| Searchable text chunks | 1,371 |
| Indexed visual records | 1,908 |
| PDF rendered pages | 1,123 |
| Embedded PDF images | 779 |
| DOCX embedded images | 6 |
| Vision caption/OCR successes | 1,630 |

## Why source files are not committed

The source PDFs, Word documents, rendered pages, and extracted images came from a SharePoint library. Before publishing those files into a public GitHub repo, confirm that each document is public and that redistribution is allowed by its publisher/license.

For a workshop, the safest pattern is:

1. Keep documents in SharePoint.
2. Commit the manifest, prompts, and scripts.
3. Let each lab participant upload an approved corpus into their own SharePoint library.
4. Run the ingestion and extraction scripts against that library.

