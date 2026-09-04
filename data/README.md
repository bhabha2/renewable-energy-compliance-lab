# Lab data

This folder makes the lab data layout explicit for replication.

The reference implementation processed source documents from SharePoint and wrote extracted images back to SharePoint. `document-index.csv` records where each public source document came from so the lab can download and upload the corpus into a new SharePoint library as part of the exercise.

## Folder layout

| Path | Purpose |
| --- | --- |
| `source-documents/` | Local cache populated by `scripts\bootstrap-sharepoint-corpus.ps1`; ignored by git except for `.gitkeep`. |
| `extracted-images/` | Optional local cache for visual assets; ignored by git except for `.gitkeep`. |
| `document-index.csv` | Source reference for the legal/requirements documents, including publisher/source URL and download status. |
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

## Source file note

The source PDFs, Word documents, rendered pages, and extracted images are generated or cached during the lab. Commit the source index and scripts, not the generated binary corpus.
