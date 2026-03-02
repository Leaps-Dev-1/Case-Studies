# Case Study
## Multi-Agent AI Automation System for End-to-End Lead Management
**Home Services / Flooring Business · Jobber CRM · Voice + WhatsApp + Email AI Agents**

---

| | | |
|---|---|---|
| **Industry:** Home Services (Flooring) | **Client Type:** SMB running 150–200 inbound ad leads/day | **Confidentiality:** Client Identity Protected (NDA) |

---

## Background & Challenge

Our client ran a flooring services business generating 150–200 inbound leads daily through Google and Meta Ads. Every lead filled out a form — and then waited. Manual follow-up was slow, inconsistent, and leaking revenue at every stage.

The core problems:

- **Speed-to-lead failure:** Human agents couldn't respond within the critical first few minutes of a form submission — the window where conversion rates are highest.
- **Inconsistent follow-ups:** Leads that didn't answer the first call were frequently lost. No structured retry logic meant missed opportunities piled up silently.
- **No unified system:** Calls, emails, and WhatsApp were handled in silos. There was no single source of truth, and Jobber CRM wasn't being used to its potential.
- **Manual quotation process:** Sales reps were manually building quotes — slow, error-prone, and impossible to personalize at scale.

The goal: build a fully automated, 24/7 lead management system that engages every lead instantly, nurtures them intelligently across multiple channels, and closes the loop inside Jobber — with zero manual intervention for routine tasks.

---

## Solution Architecture

LeapsDev designed and deployed a **Multi-Agent AI Automation Ecosystem** — five interconnected AI agents, each owning a specific stage of the lead journey, all orchestrated through n8n and synchronized in real time with Jobber CRM.

### 1. AI Voice Qualifier Agent — "The Lead Qualifier"

Triggered automatically within 1–60 minutes of a form submission (configurable), this agent handles the first and most critical touchpoint: the qualification call.

- Called the lead while interest was still hot — no human delay.
- Verified project details: flooring type, area size, location, timeline, and budget alignment.
- Collected all required data: name, email, WhatsApp number, special requirements.
- Logged a complete call transcript, AI summary, and sentiment score directly into Jobber.
- On a qualified lead: instantly triggered the automated quotation system.
- On a no-answer: handed off to the Callback Agent with full context.

### 2. Callback AI Agent — "The Persistent Follow-Up"

No lead was abandoned after a single missed call. The Callback Agent managed a structured retry sequence:

- **Attempt 2:** 4 hours after the first missed call.
- **Attempt 3:** 20–24 hours after the second missed call.
- All attempts timestamped and logged in Jobber automatically.
- After 3 failed voice attempts: automatically triggered the WhatsApp AI Agent, updated lead status in Jobber, and continued the engagement through a different channel.

### 3. WhatsApp AI Agent — "The Multi-Format Communicator"

The most capable channel agent in the system — handling rich, multi-format conversations across three trigger scenarios:

- **Post-voice failure:** Reached out after 3 unanswered calls with a personalized intro referencing the lead's specific project.
- **Post-email no-response:** Followed up with quotation reminder and booking link after 48–72 hours of email silence.
- **Direct inbound WhatsApp inquiries:** Responded instantly using the business knowledge base, collected lead details, and created a new Jobber entry automatically.

Input formats the agent handled:
- Text messages (natural language understanding)
- Voice notes (auto-transcribed, intelligently replied to)
- Images (room photos, measurement photos, flooring samples — parsed and referenced in responses)
- Documents (PDFs, floor plans, existing competitor quotes)

Output formats: text, voice messages, branded quotation documents.

### 4. Email Marketing AI Agent — "The Quotation Specialist"

Once a lead qualified, this agent took over the written channel — generating personalized quotations and managing multi-step email follow-up sequences.

**Quotation generation process:**
1. Pulled lead data from Jobber (name, flooring type, area size, location, requirements).
2. Calculated pricing dynamically using pre-configured brackets (material, labor, complexity, location).
3. Generated a branded PDF quotation with itemized pricing, timeline, payment options, and warranty terms.
4. Uploaded the PDF to Jobber and updated lead status to "Quotation Sent."
5. Dispatched a personalized email with the quote attached and a "Book Your Free Site Visit" CTA.

**Automated follow-up sequences:**
- **24–48 hrs no response:** Gentle reminder with value-add tips and appointment booking link.
- **Customer replies with questions:** Context-aware AI response using FAQs, business data, and Jobber conversation history — simultaneously notified on WhatsApp.
- **72–96 hrs still no response:** Final outreach with concurrent WhatsApp message triggered.

### 5. Orchestration & Automation Layer

Built on **n8n**, this layer was the central nervous system connecting all four agents — managing triggers, routing logic, data flow, and Jobber synchronization in real time.

Key workflows orchestrated:
- Ad form submission → Jobber entry → Voice Agent trigger (with configurable delay).
- Call outcome routing: qualified → quotation system, no answer → callback queue, not interested → archive with reason logged.
- Quotation ready → Email Agent → follow-up timer sequences.
- Multi-channel coordination: WhatsApp and Email agents synchronized to avoid duplicate outreach.
- Appointment booking → auto-created in Jobber + synced to Google Calendar + confirmation and reminders sent.

---

## Technical Stack

| Component | Details |
|---|---|
| **Voice AI Platform** | Vapi.ai — voice agent deployment and call management |
| **LLM** | Custom GPT model — qualification logic, quotation context, email responses |
| **WhatsApp** | WhatsApp Business API — text, voice note, image, and document handling |
| **Automation Orchestration** | n8n — all workflow triggers, routing logic, and agent coordination |
| **CRM** | Jobber — lead status, call logs, transcripts, quotations, appointments |
| **Calendar** | Google Calendar — dual-entry appointment sync |
| **Quotation Engine** | Dynamic PDF generator — personalized pricing and branded templates |
| **Email** | AI Email Agent via SMTP/API — sequences, quotation dispatch, follow-ups |
| **VoIP** | Twilio / Telnyx — outbound calling infrastructure |
| **Sentiment Analysis** | Built-in sentiment scoring per call (Positive / Neutral / Negative / Confused / Angry) |
| **Analytics** | Google Data Studio dashboard — sentiment trends, conversion funnel, agent performance |

---

## Lead Journey Flow

```
Ad Form Submission (Google / Meta Ads)
        ↓
Jobber CRM Entry — Status: "New - Awaiting Call"
        ↓ (1 min – 1 hr delay)
AI Voice Qualifier Agent — Call #1
   ├── Answered & Qualified → Quotation Generation → Email Agent → Follow-Up Sequences
   ├── Answered & Not Qualified → Archive lead, log reason in Jobber
   └── No Answer → Callback Agent (Attempt #2 at 4hrs, Attempt #3 at 20-24hrs)
                        └── After 3 fails → WhatsApp AI Agent triggered
                                ↓
                    Quotation / Booking / Nurture via WhatsApp
                                ↓
                    Appointment Booked → Jobber + Google Calendar synced
                    Confirmation + Reminders sent automatically
```

---

## Results & Expected Impact

| Metric | Result |
|---|---|
| Speed-to-Lead | **Under 60 minutes** — AI calls every lead automatically |
| Follow-Up Coverage | **100%** — no lead dropped after first no-answer |
| Channels Covered | **3** — Voice, Email, WhatsApp (coordinated, not siloed) |
| Quotation Turnaround | **Automated** — generated and delivered without human input |
| Operational Cost vs. Cloud Platforms | **50–60% lower** per-minute cost on custom-coded option |
| Lead-to-Booking Conversion Improvement | **3–5x** projected vs. manual process |

- Every inbound lead received an immediate, personalized response — regardless of time of day or human availability.
- The multi-channel approach ensured no lead was lost to a single point of failure — if voice failed, email engaged; if email failed, WhatsApp picked it up.
- Human staff were removed from routine follow-up entirely, freeing them for site visits, consultations, and closing.
- Jobber became a live, real-time operations hub — every interaction, transcript, sentiment score, and status update logged automatically with no manual data entry.

---

## Challenges & How We Solved Them

| Challenge | Solution |
|---|---|
| Leads going cold between form fill and first contact | Configurable 1–60 min AI call trigger — first contact happens while intent is still active |
| No-answer leads falling through the cracks | Structured 3-attempt callback logic with automatic WhatsApp escalation after all voice attempts |
| Manual quotation process too slow to scale | Automated PDF quotation engine pulling live Jobber data — quote generated and sent without human input |
| Siloed communication channels creating inconsistent experience | n8n orchestration layer coordinating all agents — WhatsApp and Email synced to avoid duplicate outreach |
| WhatsApp leads sharing photos/voice notes with no structured handling | WhatsApp agent built to parse images, transcribe voice notes, and respond intelligently in the same session |
| Jobber not being used as a real-time system of record | Full bidirectional Jobber integration — every call outcome, transcript, status, and appointment logged automatically |

---

*Client identity and commercial details are protected under NDA in accordance with LeapsDev's white-label service model.*

**LeapsDev.com · sufyan-sufy@leapsdev.com**
