# Case Study
## Automated AI Quality Assurance Bot — Post-Call Analysis & Custom CRM Dashboard
**Insurance BPO · Pay-Per-Call Campaigns · Ringba · Replacing 20+ Manual QA Specialists**

---

| | | |
|---|---|---|
| **Industry:** Insurance BPO / Pay-Per-Call | **Client Type:** Mid-Size Contact Center, Multi-Campaign Operations | **Confidentiality:** Client Identity Protected (NDA) |

---

## Background & Challenge

Our client operated a high-volume pay-per-call insurance contact center running multiple campaigns simultaneously through Ringba — including SSDI, Medicare Plus Inbound, ACA, Final Expense, and others. Every call had commercial value and needed to be reviewed for quality, compliance, and accurate disposition classification.

Their QA process was costly and unscalable:

- **20+ manual QA specialists** were employed to listen to call recordings, classify dispositions, and flag compliance issues — a process that was slow, expensive, and inconsistent between reviewers.
- **Ringba's built-in QA feature** was tried and abandoned — it wasn't granular or customizable enough for the client's multi-campaign, multi-disposition taxonomy.
- **No standardized reasoning.** When a QA specialist marked a call "Not Interested" vs "Not Qualified," there was no documented evidence trail — just a human's subjective judgment.
- **No centralized analytics.** Disposition data, sentiment trends, and campaign performance were scattered with no live dashboard for management to monitor in real time.

The core ask: replace the 20+ QA headcount with an AI system that processes every call automatically, classifies it with evidence-backed reasoning, and surfaces the results in a custom CRM dashboard — with predictable cost and zero human variability.

---

## Solution Architecture

LeapsDev designed and deployed a fully automated, end-to-end post-call QA pipeline — from call recording ingestion through Ringba to final disposition logging in a custom Airtable CRM — orchestrated on a self-hosted n8n instance running on AWS serverless autoscaling infrastructure.

### Pipeline Flow

```
Ringba API → Batch Call Log Fetch
        ↓
Recording URL Validation (Has Audio?)
   ├── No Audio → Log as "No Voice", skip to next record
   └── Has Audio → Download Recording
                        ↓
              Deepgram Nova STT Transcription
                        ↓
              AI Speaker Labeling Agent (LA: / CX: tagging)
                        ↓
              Airtable Record Created (transcript + metadata)
                        ↓
              AI Disposition Analysis Agent (GPT-4o-mini)
              + RAG Tool Calling (Custom MCP Server)
                        ↓
              Structured Output: Disposition / Sub-Disposition
              / Reason / Summary / Sentiment / Confidence
                        ↓
              Airtable Record Updated → Live QC Dashboard
```

---

### 1. Call Ingestion — Ringba API Integration

The pipeline fetched batch call logs directly from Ringba on a scheduled basis, extracting all required metadata per call:

- System Call ID, Publisher ID, System Name, Campaign Name
- Inbound/Dialed phone numbers, call timestamp, duration in seconds
- Recording URL — used to route the call to audio processing or flag it as no-voice

Calls with no valid recording URL were automatically logged as "No Voice" records and skipped — no wasted processing cost.

### 2. Speech-to-Text — Deepgram Nova

Every call recording was downloaded and passed to Deepgram Nova for transcription:

- Sub-100ms transcription latency maintained across high batch volumes.
- Raw transcripts passed to a **speaker-labeling AI agent** that identified and separated Licensed Agent (LA) and Customer (CX) turns using contextual reasoning — preserving every word verbatim while adding structural clarity.
- The labeled transcript was stored in Airtable alongside the recording link and all call metadata.

### 3. AI Disposition Analysis — GPT-4o-mini + RAG Tool Calling

The core intelligence layer: a custom-prompted GPT-4o-mini agent that analyzed each labeled transcript and assigned a disposition, sub-disposition, reason, and sentiment — with evidence from the transcript required for every classification.

**14 Primary Dispositions supported:**

| # | Disposition | Example Sub-Dispositions |
|---|---|---|
| 1 | Sales | CX Sale (confirmed enrollment/transfer) |
| 2 | DNC | Wants to stop calls, Too much calls, Rude Caller, Put me on DNC |
| 3 | Unresponsive | CX ununderstandable |
| 4 | DWSPI | Won't give info, Won't give SSN, Won't give bank info |
| 5 | Tech Issues | CX Audio Problems |
| 6 | Voicemail | Straight up voicemail |
| 7 | Not Interested | CX Hangup, CX Not Interested, CX Don't Want, CX Don't Need, No Time, Happy with plan |
| 8 | Not Qualified | Has Medicare, Age NQ, Can't afford, Condition Not Met, Has Job, Already has coverage |
| 9 | Target Hung Up | LA Hangup Call |
| 10 | Callback | LA callback scheduled |
| 11 | IVR | No key pressed, Incomplete key pressed |
| 12 | Misdialed | LA called wrong number/person |
| 13 | Language Barrier | CX Spanish / Other Language |
| 14 | Subsidy/Incentivised | CX already has coverage, looking for subsidy |

**Campaign-aware classification:** The AI applied campaign-specific qualification logic per call — different criteria for SSDI, Medicare Plus Inbound, ACA, Final Expense, MVA, and U65. The same transcript would be classified differently depending on which campaign the call belonged to.

**RAG Tool Calling via Custom MCP Server:** A custom Model Context Protocol (MCP) server was built and integrated as a RAG tool — providing the AI agent with real disposition examples and feedback samples at inference time. This grounded every classification decision in verified, campaign-specific precedents rather than relying solely on the LLM's general reasoning.

**Structured output per call (logged to Airtable):**
- `disposition` — primary classification
- `sub_disposition` — specific sub-category
- `reason` — exact quote from transcript triggering the classification
- `summary` — 1–2 sentence call summary
- `sentiment` — Positive / Neutral / Negative
- `confidence_level` — High / Medium / Low
- `key_moments` — critical conversation turning points
- `objections_raised` — list of customer objections
- `objections_overcome` — Yes / No / Partial

### 4. Custom CRM & Live QA Dashboard — Airtable

All pipeline outputs were consolidated into a custom Airtable CRM built specifically for the client's QA operation. Every call record contained the full data set:

**Fields tracked per record:** Sub-Disposition, System Name, Inbound Phone Number, Dialed Number, Campaign Name, Status, Disposition, Reason, Summary, Transcript, Call Recording (link), Call Timestamp, Duration Seconds, System Call ID, System Publisher ID, Call Outcome Category.

**Dashboard capabilities:**
- Real-time metrics overview with KPIs and alert notifications.
- Executive summary and manager operational views.
- Date filters: Today, Yesterday, Last 7 days, Last Week, Last Month, Custom Period.
- Global search across all fields with advanced multi-criteria filtering.
- Disposition and sub-disposition breakdown by campaign, publisher, and time period.
- Daily, weekly, and monthly QA reports exportable to CSV/Excel.
- Role-based access: Super Admin and QA Manager permissions configured from launch.

### 5. Infrastructure — Self-Hosted n8n on AWS Serverless

The entire orchestration layer ran on a self-hosted n8n instance deployed serverlessly on AWS:

- **Auto-scaling** ensured the pipeline handled variable call volumes without manual infrastructure management or idle server costs.
- **Error handling** built into every node: failed transcriptions, empty audio files, and AI agent errors were caught, routed, and logged rather than silently dropped.
- **Retry logic** on AI agent nodes (up to 5 retries with 3-second wait) prevented transient API failures from losing call records.
- **Batch loop processing** iterated through all call records efficiently — each call processed in sequence with full error isolation.

---

## Campaign-Specific Logic Implemented

### SSDI (Social Security Disability Insurance)
**Qualified:** Age 49–65, US resident, not currently collecting SS benefits, not working due to disability, worked 5 of last 10 years, no attorney representation.
**Disqualified:** Under 49 or over 65, already receiving benefits, insufficient disability, has SSDI attorney.

### Medicare Plus Inbound
**Qualified:** Age 65+, US resident, has Medicare Part A and Part B, not already enrolled in Medicare Advantage (or open to switching).
**Disqualified:** Under 65 with no qualifying disability, Medicaid-only, already satisfied with current plan.

---

## Technical Stack

| Component | Details |
|---|---|
| **Call Tracking / Recording Source** | Ringba — pay-per-call campaign management and recording storage |
| **Automation Orchestration** | n8n (self-hosted, AWS serverless, autoscaling) |
| **Speech-to-Text** | Deepgram Nova — sub-100ms transcription latency |
| **LLM** | GPT-4o-mini — disposition analysis, speaker labeling, call summarization |
| **RAG / Tool Calling** | Custom MCP Server — campaign-specific disposition examples for grounded AI decisions |
| **CRM & Dashboard** | Custom Airtable — live QC reports, filtering, role-based access, CSV export |
| **Infrastructure** | AWS serverless (autoscaling) — no idle compute costs, handles variable volume |

---

## Results & Impact

| Metric | Result |
|---|---|
| QA Specialists Replaced | **20+** manual reviewers → fully automated |
| Call Processing Volume | Over **50,00,000+** calls/mo fully automated |
| Operational Cost Reduction | **60%+** in QA department costs |
| Disposition Accuracy | Evidence-backed — every classification tied to a transcript quote |
| Consistency | **100%** — zero human reviewer variability across campaigns |
| Campaigns Supported | **Multi-campaign** — SSDI, Medicare, ACA, FE, MVA, U65 |
| Data Retained & Searchable | **Up to 1 year** — full historical QC record |

- Every call received a disposition with a documented reason — no more subjective human judgment, no more inconsistency between reviewers.
- Ringba's internal QA feature had been tried and rejected; the custom pipeline delivered the granularity, campaign-awareness, and RAG-grounded accuracy that off-the-shelf tools couldn't provide.
- Management gained a live operational view of campaign performance, sentiment trends, and disposition breakdowns — previously impossible without manual reporting aggregation.
- The pipeline cost is fully predictable — per-API-call pricing on Deepgram and GPT-4o-mini, with no headcount overhead, no sick days, and no training cycles.

---

## Challenges & How We Solved Them

| Challenge | Solution |
|---|---|
| Different campaigns have different qualification rules — one classification system can't fit all | Campaign-aware AI prompt — agent reads campaign name from record and applies the correct qualification criteria before classifying |
| AI making ambiguous disposition decisions between similar outcomes (e.g. Not Interested vs NQ) | 12 explicit classification priority rules built into system prompt — with decision logic covering every edge case (Voicemail vs Not Interested, IVR vs Unresponsive, etc.) |
| AI needed examples, not just rules, to make accurate decisions | Custom MCP Server as a RAG tool — real disposition samples injected at inference time to anchor decisions in verified precedents |
| Calls with no audio content wasting processing budget | Recording URL validation gate — empty/no-audio calls bypassed transcription and AI processing, logged as "No Voice" automatically |
| High call volume with variable peaks requiring reliable infrastructure | Serverless AWS autoscaling — n8n orchestration layer scales up and down with demand, no manual intervention required |
| Client needed a QA system that couldn't produce unpredictable edge cases | Structured JSON output enforced — AI required to always output a valid disposition from the 14-item taxonomy, no "unknown" or fallback classifications permitted |

---

*Client identity and commercial details are protected under NDA in accordance with LeapsDev's white-label service model.*

**LeapsDev.com · sufyan-sufy@leapsdev.com**
