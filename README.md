# AI Native Law Firm Prototype

Share a legal issue and upload evidence, and the prototype will provide legal analysis, evidence analysis, draft legal document(s), and a docket/invoice.

This is a prototype only and does not constitute legal advice. Do not put sensitive information into the prototype.

# Video
https://www.loom.com/share/dc0b2c8317334a12b9b5467ab8adff5f
This video uses dummy data.

## Prototype
https://www.stackai.com/ai-webpage/6a9624fb4cb49584761c574e-4qjZ4r1AdyA3HWhyKY8OUX

## Interface
<img width="643" height="648" alt="Screenshot 2026-09-01 at 12 28 35 AM" src="https://github.com/user-attachments/assets/1aa6daea-c4c4-4cb0-89be-ad2efd68bb0d" />
<img width="647" height="634" alt="Screenshot 2026-08-31 at 11 36 00 PM" src="https://github.com/user-attachments/assets/224fa5a1-b11c-4270-ad73-6b786a4b7109" />

# Sample Output
<img width="648" height="629" alt="Screenshot 2026-09-01 at 12 24 56 AM" src="https://github.com/user-attachments/assets/91aea846-5817-4db2-8b2e-9a05b1cef9a1" />
<img width="650" height="627" alt="Screenshot 2026-09-01 at 12 25 33 AM" src="https://github.com/user-attachments/assets/bf7973cd-6030-4ba6-a3c3-80e33c5abbb7" />
<img width="643" height="646" alt="Screenshot 2026-09-01 at 12 25 58 AM" src="https://github.com/user-attachments/assets/76ba0990-731a-4b7c-beee-6cf05a875d38" />
<img width="637" height="660" alt="Screenshot 2026-09-01 at 12 26 22 AM" src="https://github.com/user-attachments/assets/b1ddc040-1b13-4ae0-9820-22d52851a319" />

# Back-end
<img width="1177" height="581" alt="Screenshot 2026-08-31 at 11 35 15 PM" src="https://github.com/user-attachments/assets/7c19c197-fbc7-4ba9-92cd-3a838a9a85d7" />

# Details

# AI-Native Law Firm

A prototype multi-agent workflow that turns a plain-language legal issue and a folder of evidence into a full first-pass case file: issue classification, theory of the case, evidence gap analysis, a drafted legal document, and a client docket/invoice.

> ⚠️ **This is a prototype only and does not constitute legal advice.** Do not put sensitive or real client information into the prototype.

## What it does

Given:
- A short description of a legal issue (e.g. *"My spousal sponsorship application to become a Canadian permanent resident got refused"*)
- A batch of supporting evidence files (emails, listings, receipts, activity logs, etc.)

...the pipeline produces four outputs:

1. **Case Classification & Theory of the Case** — classifies the legal issue (e.g. administrative/immigration law), lays out a theory of the case grounded in case law, and flags procedural issues.
2. **Evidence Analysis** — aligns the uploaded evidence against the theory of the case, sentence-cited to source documents, and flags what's missing.
3. **Legal Document Draft** — identifies the most urgent document needed (e.g. an affidavit) and drafts it.
4. **Client Bill** — a docket entry, itemized invoice, and client-ready email summarizing work completed and next steps.

## How it works

The workflow is a directed graph of agent and formatting nodes:

```
User Inputs Legal Issue ─────────────┐
User Batch Uploads Evidence ─────────┤
                                      ▼
                          ┌─────────────────────┐
                          │  Classification Agent │──▶ Case Classification & Theory of the Case
                          └─────────────────────┘
                                      │
                                      ▼
                          ┌─────────────────────┐
                          │   Evidence Checker    │──▶ Evidence Analysis
                          └─────────────────────┘
                                      │
                                      ▼
                          ┌─────────────────────┐
                          │    Legal Drafter      │──▶ Legal Document Draft
                          └─────────────────────┘
                                      │
                                      ▼
                          ┌─────────────────────┐
                          │ Client Docketing &    │──▶ Client Bill
                          │      Billing          │   (docket, invoice, client email)
                          └─────────────────────┘
```

Each agent node runs on **Claude 4.5 Haiku** and has access to **web search** plus an MCP tool (`find_legal_documents`) for retrieving Canadian case law.

## Example run

**Legal issue input:**
> My spousal sponsorship application to become a Canadian permanent resident got refused! Immigration said that my relationship wasn't real because there wasn't enough evidence, and that I lied on an old visa application. This is a huge mistake, I never lied, and my relationship is real! I'm scared that Immigration is going to force me to leave Canada and my partner!

**Evidence uploaded (7 files):**
- `891 Dundas listing.html`
- `message from client - Sofia.txt`
- `MyActivity.html`
- `amazon - your orders.html`
- `race entry - Lakeshore Harvest Run.html`
- (plus additional supporting files)

**Sample output highlights:**
- **Classification:** Administrative/immigration law — genuineness of relationship (s. 4 IRPR, s. 12(1) IRPA), misrepresentation inadmissibility (s. 40(1)(a) IRPA), and procedural fairness / incompetence of former counsel.
- **Theory of the case:** Relationship genuineness proven through "ordinary life" evidence (cohabitation timeline, shared purchases, shared bills, emergency contact designation) rather than paperwork, supported by case law such as *Haer v Canada (MCI)*, 2020 FC 530 and *Patel v Canada (MCI)*, 2012 FC 1389.
- **Evidence analysis:** Cross-references shipping addresses, running-route changes, shared expenses, and search history against the theory of the case; flags a factual dispute over a missing continuation sheet in the misrepresentation finding.
- **Legal document:** A draft **Affidavit of the Applicant** addressing procedural fairness, the misrepresentation dispute, and cohabitation.
- **Client bill:** Docket entry with hours logged per task, a market-rate invoice, and a client email outlining next steps (e.g. filing deadline for judicial review).

## Tech notes

- Built as an agentic pipeline where each node's output feeds the next.
- Formatting/output nodes (rendered as text/markdown panels) capture each agent's response for review and export (copy/download) independently of the underlying agent nodes.
- Model: Claude 4.5 Haiku for all agent nodes.
- Tools: web search (all agents) + A2AJ tool for Canadian case law lookups (https://a2aj.ca/data/)

## Disclaimer

This project is a proof of concept exploring AI-assisted legal workflows. Outputs are **not legal advice**, may contain inaccurate or fabricated case citations, and should be reviewed by a licensed lawyer before any real-world use. Do not upload real client data, personal information, or privileged materials into the prototype.
