# Case Study
## AI Voice Agent for High-Volume Outbound Lead Qualification
**Contact Center BPO · Pharmacy Campaign · 100,000+ Calls/Month**

---

| | | |
|---|---|---|
| **Industry:** Healthcare BPO / Contact Center | **Client Type:** Mid-Size BPO, 100+ Human Agents | **Confidentiality:** Client Identity Protected (NDA) |

---

## Background & Challenge

Our client operated a 100+ agent contact center delivering outbound pharmacy campaign services for a major healthcare enterprise. Their operation ran on ViciDial with Google Sheets as the CRM layer.

Despite a high daily dial volume of 400–500 calls per agent, the operation faced three compounding inefficiencies:

- **Agent idle time:** Human agents spent the majority of their time waiting for calls to connect — burning productive hours on dead air, voicemails, and dropped lines.
- **Inconsistent qualification:** Adherence to the multi-criteria eligibility screen (medication count, insurance type) varied by agent, leading to unqualified leads reaching closers.
- **Unsustainable infrastructure costs:** The team was locked to ElevenLabs TTS at ~$0.12/min and a third-party VoIP provider with no dialer server access — costs scaling faster than revenue.

The goal: automate front-line qualification, reduce cost per contact, and free human agents to focus exclusively on high-intent, connected conversations.

---

## Solution Architecture

### 1. AI Qualification Voice Agent — "The Intelligent Opener"

LeapsDev designed and deployed a custom outbound voice AI agent to own the first phase of each call — reaching prospects, running the qualification screen, and hot-transferring interested callers directly to human verifiers.

Qualification criteria the agent handled autonomously:

- Verified the prospect was taking 6 or more daily medications.
- Confirmed primary insurance was not TriCare or Humana (campaign ineligibility flags).
- Dynamically adapted conversation flow based on customer responses — not a rigid IVR, but a conversational AI capable of handling objections and re-engagement.
- Executed seamless warm transfers to a human verifier the moment a lead qualified — zero hold time, full context handoff.

### 2. Custom Self-Hosted TTS — Fine-Tuned on Agent Voices

At 100K+ calls/month, ElevenLabs API costs were a major operational drag. LeapsDev fine-tuned an open-source TTS model on the client's top-performing human agent voices using GPU-based LoRA training, then self-hosted it on the client's own infrastructure.

- Voice quality matching or exceeding ElevenLabs output.
- Sub-150ms TTS latency — critical for natural real-time conversation.
- Full data ownership, zero dependency on third-party API pricing or limits.
- TTS cost reduced from ~$0.12/min to ~$0.008/min — a **93% reduction**.

### 3. Self-Hosted Dialer with Custom SIP Trunks

The client's existing ViciDial was hosted by a third-party vendor with no root access, locking them to a single VoIP provider. LeapsDev deployed a fully self-hosted ViciDial instance with custom SIP trunk integrations across major providers.

- Complete dialer control for 100+ concurrent agent lines.
- Multi-provider SIP failover (Sinch, SignalWire) for call reliability at scale.
- PJSIP protocol for smooth AI-to-human call transfer routing.
- Saved over **$5,000/month** compared to third-party hosted dialer costs.

### 4. Post-Call Analytics Dashboard

A custom post-call sentiment analysis dashboard gave operations supervisors real-time visibility into call quality, qualification rates, and AI agent performance — enabling rapid prompt iteration and conversation flow optimization based on live campaign data.

---

## Technical Stack

| Component | Details |
|---|---|
| **Voice AI Platform** | Fully custom-built orchestration (no third-party voice AI platform dependency) |
| **LLM** | OpenAI GPT-4o — real-time conversational reasoning & qualification logic |
| **TTS** | Fine-tuned open-source model, GPU-trained (LoRA), self-hosted |
| **STT** | Deepgram Nova — sub-100ms transcription latency |
| **Dialer** | ViciDial (self-hosted) with PJSIP call transfer protocol |
| **SIP Providers** | Sinch, SignalWire (multi-provider failover) |
| **CRM Integration** | Google Sheets — real-time sync, lead status, duplicate prevention |
| **Analytics** | Custom post-call sentiment analysis dashboard |
| **Infrastructure** | AWS Serverless + dedicated GPU server for TTS inference with Autoscaling |

---

## Results & Impact

| Metric | Result |
|---|---|
| Calls Processed / Month | **100,000+** by AI agent |
| Qualification Cost Reduction | **30%** vs. human-only operations |
| TTS Cost Reduction | **70%** — from $0.30 to $0.015/min |
| Monthly Dialer Savings | **$5,000+** vs. hosted vendor |

- Human agents were fully removed from the qualification bottleneck — every transfer they received was a live, interested, pre-screened prospect.
- The AI agent operates on a connected-minutes billing model — the client only pays when the AI is in active conversation, whether 1 second or 10 minutes. No wasted spend on ring time or voicemails.
- Qualification consistency reached 100% — no agent-to-agent variation in eligibility criteria application.
- After a 3-month post-launch optimization phase, LeapsDev refined system prompts and conversation logic based on real call data, significantly improving qualification-to-transfer conversion rates.

---

## Challenges & How We Solved Them

| Challenge | Solution |
|---|---|
| No root access to existing dialer server | Deployed a parallel self-hosted ViciDial with full SIP integration and root control |
| High TTS API costs unsustainable at 100K+ calls/month | Fine-tuned and self-hosted TTS model, reducing per-minute cost by 93% |
| AI voice felt unnatural in early testing, reducing transfer rates | 3-month post-launch optimization: iterated prompts, conversation logic, and voice model based on live sentiment data |
| Duplicate call prevention across large lead lists | Phone number-based deduplication logic built into the Google Sheets CRM sync layer |

---

*Client identity and commercial details are protected under NDA in accordance with LeapsDev's white-label service model.*

**LeapsDev.com · sufyan-sufy@leapsdev.com**
