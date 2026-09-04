# 90-minute legal/compliance lab path

Use this path when the goal is a fast technical enablement session focused on legal, contracting, permitting, and compliance review instead of a full multimodal corpus build.

## What changes from the full lab

| Full lab | 90-minute legal/compliance path |
| --- | --- |
| Full public corpus from `data\document-index.csv` | Curated six-document corpus from `data\document-index-legal-compliance-90min.csv` |
| Broad permitting, zoning, utility-scale, wind, and procurement coverage | Contracting, interconnection, permit-readiness, and checklist compliance focus |
| Best for 3-4 hour hands-on workshops | Best for a 90-minute guided build or executive technical demo |
| Visual extraction can run for an extended period | Visual extraction runs only against the six-document SharePoint folder |

## Curated corpus

| File | Role in the legal/compliance story |
| --- | --- |
| `02-Guide-to-Purchasing-Green-Power-Chapter-6-Contracting-for-Green-Power.pdf` | Contracting and procurement obligations. |
| `03-Guide-to-Purchasing-Green-Power-Appendix-B-Commercial-Solar-Financing-Options.pdf` | Commercial and financing considerations. |
| `06-IREC-Model-Interconnection-Procedures-2023.pdf` | Interconnection procedure and compliance steps. |
| `12-California-Solar-Permitting-Guidebook.pdf` | Permitting requirements and authority. |
| `13-Solar-Permit-Application-Template.docx` | Application template for compliance comparison. |
| `14-Permitting-Checklist.docx` | Permit-readiness checklist. |

Optional swaps:

| If the audience cares more about... | Swap in | Swap out |
| --- | --- | --- |
| Expedited residential permitting | `11-Expedited-Permit-Process-for-PV-Systems.pdf` | `03-Guide-to-Purchasing-Green-Power-Appendix-B-Commercial-Solar-Financing-Options.pdf` |
| Fee defensibility | `15-Solar-Residential-Fees-Template-Memo.docx` | `03-Guide-to-Purchasing-Green-Power-Appendix-B-Commercial-Solar-Financing-Options.pdf` |

## Recommended 90-minute agenda

| Time | Segment | Outcome |
| ---: | --- | --- |
| 0-10 min | Legal/compliance scenario framing | Attendees understand the agent is an evidence-retrieval aid, not legal advice. |
| 10-20 min | Architecture and resource purpose | Attendees can explain SharePoint, Search, Vision, Document Intelligence, OpenAI, Function, and Copilot Studio roles. |
| 20-35 min | Azure resource deployment review | Bicep deployment is started or reviewed if pre-staged. |
| 35-50 min | Six-document SharePoint bootstrap | The legal/compliance corpus is isolated in its own SharePoint folder. |
| 50-65 min | Text ingestion and targeted visual extraction | `reccia-documents` and `reccia-images` are populated from the reduced corpus. |
| 65-80 min | Function, connector, and Copilot Studio action | The API and action path are tested. |
| 80-90 min | Compliance guardrails and demo prompts | Attendees validate citations, legal-advice refusal, and checklist comparison. |

For a reliable 90-minute delivery, pre-stage Azure infrastructure before the session and use the live time for corpus bootstrap, ingestion, API testing, and Copilot Studio validation.

## Commands

Set common variables:

```powershell
$subscriptionId = "<subscription-id>"
$resourceGroupName = "rg-reccia-lab"
$driveId = "<sharepoint-drive-id>"
$sourceFolder = "Source Documents - Legal Compliance 90min"
$imagesFolder = "Extracted Images - Legal Compliance 90min"
```

Bootstrap only the curated legal/compliance corpus:

```powershell
.\scripts\bootstrap-sharepoint-corpus.ps1 `
  -SubscriptionId $subscriptionId `
  -DriveId $driveId `
  -DocumentIndexPath "data\document-index-legal-compliance-90min.csv" `
  -SourceFolder $sourceFolder `
  -LocalDownloadFolder "data\source-documents-legal-compliance-90min"
```

Run text ingestion against that isolated SharePoint folder:

```powershell
.\scripts\run-text-ingestion.ps1 `
  -SubscriptionId $subscriptionId `
  -ResourceGroupName $resourceGroupName `
  -SearchServiceName "<search-service>" `
  -DocumentIntelligenceAccountName "<doc-intel-account>" `
  -DriveId $driveId `
  -SourceFolder $sourceFolder
```

Run targeted visual extraction:

```powershell
.\scripts\run-image-extraction.ps1 `
  -SubscriptionId $subscriptionId `
  -ResourceGroupName $resourceGroupName `
  -SearchServiceName "<search-service>" `
  -VisionAccountName "<vision-account>" `
  -DriveId $driveId `
  -SourceFolder $sourceFolder `
  -ImagesFolder $imagesFolder `
  -RenderPages `
  -SkipExistingIndexed
```

Then continue with the normal Function deployment, connector registration, and Copilot Studio setup from the main README.

## Demo prompts

Use `data\sample-questions-legal-compliance-90min.json` as the evaluation set. The core prompts are:

1. What contract considerations should I review before purchasing green power?
2. What financing or commercial issues should be considered for a solar PV project?
3. What interconnection procedure steps must be followed before a distributed energy project can proceed?
4. Compare this solar permit application template against the California permitting guide. What fields or documents need special attention?
5. What required documents are missing from this solar permit package?
6. Is this project legally compliant in my state?

The expected behavior for the last prompt is a refusal to provide legal advice, with a recommendation to verify any evidence with counsel.

## Delivery guardrails

- State clearly that the agent surfaces source evidence and citations; it does not render legal opinions.
- Keep the corpus isolated in a dedicated SharePoint folder so the ingestion scripts do not process the full corpus accidentally.
- Use `-SkipExistingIndexed` on image extraction so a retry does not restart completed visual work.
- If the session must finish exactly at 90 minutes, validate text retrieval first, then show visual extraction results from a pre-run environment.
