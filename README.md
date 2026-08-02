# Digital Rental Contract Automation (n8n)

## Overview
An automated pipeline that turns a customer-facing intake form into a signed, personalized rental contract — delivered to both the business owner and the client via email, with zero manual paperwork.

## The Problem
Generating rental contracts manually meant retyping client details into a template, embedding their signature and ID images by hand, and emailing everything out one by one. Slow, repetitive, and easy to get wrong under time pressure — especially at high booking volume.

## The Solution
Built an end-to-end n8n workflow that:
1. **Captures a form submission** (via Tally) with client details: name, country, ID number, contact info, ID document photos, and an e-signature
2. **Copies a Google Docs contract template** and dynamically fills in the client's details via templated placeholders (`{{name}}`, `{{country}}`, `{{document_number}}`, etc.)
3. **Applies business-rule logic** to auto-fill fields like vehicle/asset registration number based on a selected option (dropdown → lookup via conditional expression)
4. **Embeds the client's uploaded signature** directly into the contract document via the Google Docs API's image-replace endpoint
5. **Emails the finished, personalized contract** to both the business owner (with ID documents attached) and the client (with a clean summary message)
6. **Logs every contract** to a CRM spreadsheet (client name, contact info, contract link, timestamps) for record-keeping

## Key Technical Challenges Solved
- **Dynamic document personalization** — used Google Docs' `batchUpdate` API to programmatically replace embedded images (not just text) inside a live document, based on data collected seconds earlier
- **Conditional business-rule mapping** — used nested ternary expressions to map a client-facing dropdown selection to the correct backend reference value (e.g. registration number) without hardcoding per-submission logic
- **Multi-recipient document routing** — split the contract + attachments so the business owner receives full documentation (ID front/back) while the client receives just their signed contract, in one automated pass
- **Conditional branching on form completeness** — used an `IF` node to route submissions differently depending on which optional documents were provided

## Result
- Fully automated, from form submission to signed contract delivery — no manual document editing or email sending
- Every contract automatically logged for CRM/record-keeping purposes
- Turns a multi-step manual process into a same-minute automated delivery

## Tech Stack
`n8n` · `Tally Forms` · `Google Docs API` · `Google Drive` · `Google Sheets` · `Gmail` · `Conditional logic / expressions`

## Workflow File
See [`workflow.json`](./workflow.json) for the exported n8n workflow.

---
*Note: This workflow was built for a real client project. All client personal data, document access tokens, credentials, and business-identifying details (file IDs, registration numbers, contact info) have been fully removed or replaced with placeholders for this public example. The original export contained live, time-limited signed URLs to a client's uploaded ID and signature — these were stripped entirely rather than just relabeled.*
