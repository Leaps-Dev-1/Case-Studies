# Case Study
## AI-Powered Student Inquiry Management System — WhatsApp & Website Chatbot
**Student Accommodation · UAE Market · Bilingual AI (English & Arabic) · 24/7 Lead Qualification**

---

| | | |
|---|---|---|
| **Industry:** Student Accommodation / Real Estate (UAE) | **Client Type:** International student housing operator, multi-property | **Confidentiality:** Client Identity Protected (NDA) |

---

## Background & Challenge

Our client operated a student accommodation property in Dubai serving university students from institutions across the UAE — including DIAC, Business Bay, and Knowledge Park. Inquiries came in around the clock from prospective students asking about room types, pricing, availability, booking processes, and property amenities.

The business faced a classic first-responder problem in a competitive market:

- **Delayed responses were costing bookings.** Students shopping for accommodation contact multiple properties simultaneously — the first to respond wins. Without 24/7 coverage, after-hours and weekend inquiries were going unanswered for hours or days.
- **Language barriers were creating friction.** A significant portion of the student base communicated primarily in Arabic. Human staff handled this inconsistently, and non-Arabic-speaking team members couldn't qualify these leads at all.
- **Manual data entry and inconsistent information.** Staff were logging inquiry details by hand, often incompletely. Different agents gave different answers to the same questions, creating inconsistency that eroded trust.
- **No lead qualification or prioritization system.** All inquiries received the same level of attention regardless of intent — hot leads ready to book were treated the same as early-stage browsers, wasting sales team time.

The goal: deploy an AI agent that responds to every student inquiry in under 10 seconds, handles bilingual conversations fluently, qualifies and scores every lead automatically, and logs everything to a live CRM — with zero manual effort for routine inquiries.

---

## Solution Architecture

LeapsDev designed and deployed a dual-channel AI inquiry management system — a WhatsApp AI agent and an embedded website chatbot — both operating on the same underlying intelligence, qualification logic, and CRM pipeline.

### 1. WhatsApp AI Agent

The primary contact channel for students in the UAE market. The agent operated on the property's WhatsApp Business number, responding to every inbound message in under 10 seconds.

**Multi-format input handling:**
- Text messages — natural language understanding in English and Arabic.
- Voice notes — auto-transcribed via Whisper, then responded to intelligently in the student's language.
- Images — room photos, floor plans, ID documents processed and referenced in conversation.
- Documents — PDFs, contracts, and booking forms handled within the chat flow.

**What the agent handled:**
- Room types, availability, and pricing across all unit categories (Twin, Studio, Single Standard, Single Large).
- Property amenities — pool, gym, study room, gaming zone, shuttle service.
- Location details and proximity to target universities.
- Booking process, payment terms, semester dates, and move-in timelines.
- Policies — cancellation, refund, refer-a-friend program, special requests.
- Objection handling — price, location, amenity comparisons.

### 2. Website Chatbot

An embedded chatbot widget deployed on the property's website pages, offering identical intelligence to the WhatsApp agent:

- Proactive engagement trigger — appeared with contextual prompts like "Need help finding a room?" to capture passive visitors.
- Seamless WhatsApp handoff — if a student preferred to continue on WhatsApp, the agent collected their number and transitioned the conversation without losing context.
- Same CRM logging and qualification logic as the WhatsApp channel.

### 3. Intelligent Lead Qualification & Scoring

Every conversation was evaluated in real time against a qualification framework:

**Data collected per inquiry:**
- University name, preferred move-in date, budget range, room type preference, language, referral source.

**Disposition tagging:**
- Ready to Book — high intent, ready to proceed
- Needs Follow-up — interested but requires more time or information
- Just Browsing — early-stage, gathering options
- Not Qualified — budget mismatch, wrong location, not serious

**Priority scoring:** Hot / Warm / Cold — derived from conversation signals (urgency language, specific questions about booking, budget alignment).

Returning students were recognized via WhatsApp number — the agent referenced previous conversations naturally, updated their CRM record rather than duplicating it, and picked up exactly where the last conversation left off.

### 4. Google Sheets CRM Integration — Auto-Logged

Every conversation produced a structured CRM record with zero manual input:

- WhatsApp number (unique student ID), full name, university, room preference, budget range, move-in date, language, disposition tag, priority score.
- AI-generated conversation summary (3–5 sentences) capturing key intent signals and discussion points.
- Timestamps for first contact, last message, and response time.
- Staff notes field for manual additions after human handoff.

When a conversation required human attention — complex situations, pricing negotiations, or urgent requests — the system triggered an alert to staff with full conversation history attached, enabling seamless continuation without the student repeating themselves.

### 5. Automation Orchestration — n8n

All workflows connecting the AI agent, WhatsApp API, website chatbot, and Google Sheets CRM were built and orchestrated in n8n — handling message routing, CRM record creation/updates, language detection, human handoff alerts, and conversation memory management.

---

## Technical Stack

| Component | Details |
|---|---|
| **WhatsApp Platform** | AiSensy — WhatsApp Business API management |
| **LLM** | GPT-4 — bilingual conversation, qualification logic, response generation |
| **Voice Note Transcription** | OpenAI Whisper — Arabic and English voice note processing |
| **Automation Orchestration** | n8n — workflow automation, CRM sync, alert routing |
| **CRM** | Google Sheets — auto-logged lead records, staff-accessible |
| **Website Chatbot** | Embedded widget — proactive engagement, WhatsApp handoff |
| **Languages** | English & Arabic (UAE market dialect-aware) |
| **Infrastructure** | VPS hosting — $15/month |

---

## Results & Expected Impact

| Metric | Result |
|---|---|
| Response Time | **Under 10 seconds** for 95%+ of inquiries |
| Availability | **24/7/365** — after-hours, weekends, holidays fully covered |
| Lead Capture | **100%** of conversations logged to CRM automatically |
| Qualification Rate | **85%+** of inquiries properly categorized and scored |
| Languages Supported | **2** — English and Arabic, UAE dialect-aware |
| Monthly Operational Cost | **$80–150** (scales with conversation volume) |

- The first-responder gap was eliminated — every student inquiry received an intelligent response within seconds regardless of time zone, day of week, or staff availability.
- Arabic-speaking students received native-quality responses without any language barrier — a significant competitive advantage in the UAE student housing market.
- Sales staff received a pre-qualified, prioritized lead queue every day — hot leads flagged for immediate follow-up, browsers deprioritized. No more treating every inquiry the same.
- CRM records were complete, consistent, and required zero manual data entry — the quality of lead data improved immediately from day one.

---

## Challenges & How We Solved Them

| Challenge | Solution |
|---|---|
| After-hours and weekend inquiries going unanswered | 24/7 AI agent with under 10-second response time — zero coverage gaps |
| Arabic-speaking students not receiving quality support | Native bilingual agent trained on UAE Arabic dialect — language auto-detected, response served in student's language |
| Inconsistent information across different staff members | Single AI agent trained on structured knowledge base — same accurate answer every time, every channel |
| Manual CRM entry creating incomplete and duplicate records | Fully automated Google Sheets logging — structured record created per conversation, returning students updated not duplicated |
| No lead prioritization — sales team overloaded with low-intent inquiries | Hot/Warm/Cold scoring and disposition tagging on every conversation — staff work only the highest-intent leads |
| Students preferring WhatsApp over website | Seamless website-to-WhatsApp handoff — agent collects number and continues conversation in preferred channel without losing context |

---

*Client identity and commercial details are protected under NDA in accordance with LeapsDev's white-label service model.*

**LeapsDev.com · sufyan-sufy@leapsdev.com**
