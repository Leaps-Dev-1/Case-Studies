# Case Study
## Inbound AI Voice Agent for Automotive Dealership — Hallucination Resolution & Live Inventory RAG
**Automotive / Car Dealership · Inbound Sales Calls · 500+ Daily Calls · Vapi.ai + Custom RAG**

---

| | | |
|---|---|---|
| **Industry:** Automotive Retail (Canadian Dealership) | **Client Type:** Established car dealership, 500+ inbound calls/day | **Confidentiality:** Client Identity Protected (NDA) |

---

## Background & Challenge

Our client operated a busy Canadian automotive dealership handling 500+ inbound sales calls daily. Callers were asking about vehicle availability, pricing, financing options, trade-ins, warranty coverage, test drive bookings, and certified pre-owned details — a high-knowledge, high-stakes conversation where accuracy and tone directly impacted sales outcomes.

The dealership needed an AI voice agent to handle this inbound volume and was already committed to the Vapi.ai platform. LeapsDev was brought in to build, deploy, and stabilize the agent.

Two compounding problems made this project technically demanding:

**Problem 1 — AI Hallucinations & Prompt Failure**
The agent wasn't behaving like a sales representative. It was reading instructions out loud instead of having natural conversations. No available LLM configuration on Vapi was fitting the use case cleanly — the model would surface system prompt language, break conversation flow, or fabricate information about vehicles, pricing, and financing terms that didn't exist. Every week produced new failure patterns.

**Problem 2 — Stale Knowledge Base**
The dealership's inventory and promotions changed daily — new vehicles in stock, price updates, active promotions expiring and launching, new certified pre-owned listings. A static knowledge base became outdated almost immediately after deployment. The AI was confidently answering questions about inventory and promotions that were no longer accurate, creating a serious credibility problem on live customer calls.

---

## Solution

### 1. System Prompt Engineering — 3-Month Iterative Refinement

There was no quick fix. The hallucination and instruction-reading problem required sustained, evidence-based prompt engineering over a 3-month period — weekly iterations driven by real call data.

The core approach:
- Rebuilt the system prompt architecture from scratch, separating behavioral instructions from knowledge content so the model could process one without surfacing the other.
- Progressively tested each LLM model available on Vapi against the dealership's actual call scenarios — vehicle inquiries, financing questions, trade-in handling, CPO warranty questions, test drive booking — to identify the best-fit configuration.
- Tuned conversation flow logic so the agent handled ambiguous caller intent gracefully — guiding undecided callers toward the right vehicle type based on stated needs (space, budget, fuel efficiency) rather than defaulting to generic responses.
- Embedded mystery shopper readiness into the agent behavior: professional greeting, accurate product knowledge, transparent pricing disclosure (all-in pricing as required by Canadian law), clear call-to-action on every call, and appointment/test drive offer before closing.
- Each weekly enhancement was scoped around specific failure patterns from the prior week — not guesswork, but structured iteration.

By month three, the agent was conducting complete inbound sales conversations: handling vehicle inquiries across the full Nissan lineup (Rogue, Altima, Sentra, Pathfinder, Kicks, Frontier, LEAF, etc.), answering financing and leasing questions, quoting CPO warranty terms accurately, and offering test drive bookings — without reading instructions or fabricating information.

### 2. Custom RAG Database with FastAPI Tool Calling

To solve the stale inventory problem, LeapsDev built a custom Retrieval-Augmented Generation (RAG) system integrated directly into the Vapi agent via FastAPI tool calling.

**How it worked:**
- A custom FastAPI backend was deployed to serve as the agent's live knowledge retrieval layer.
- The dealership's daily inventory updates, active promotions, vehicle details, and pricing information were ingested into the RAG database on a continuous basis — keeping the agent's knowledge current with the website.
- When a caller asked about a specific vehicle, promotion, or price, the agent called the FastAPI tool at inference time — retrieving the current, accurate record rather than relying on a static system prompt or outdated embedded knowledge.
- This meant the agent could answer questions like "Is the 2025 Rogue still on promotion?" or "Do you have a white Sentra in stock?" with live data — not a cached snapshot from three weeks ago.

The RAG integration also covered the dealership's full knowledge base: financing tiers, special programs (College Graduate, Military/RCMP, First-Time Buyer, Loyalty), CPO and Certified Select warranty terms, service department hours, OMVIC fee disclosures, trade-in processes, and CARFAX report availability — all grounded in structured, retrievable data rather than embedded prompt text.

---

## What the Agent Handled

The fully stabilized agent managed the complete inbound call flow for a Canadian automotive dealership:

- New vehicle inquiries across the full Nissan lineup — sedans, SUVs, trucks, EVs
- Used and Certified Pre-Owned vehicle questions — accident history, mileage, ownership count, CARFAX availability, CPO warranty terms (72 months/120,000 km)
- Financing and leasing — credit tiers, APR estimates, down payment guidance, special programs, online application referral
- Trade-in process — online appraisal explanation, in-person appraisal offer
- Test drive scheduling — same-day availability offered, appointment booking
- Service department queries — hours, walk-in policy, appointment scheduling, shuttle availability
- Canadian compliance handling — all-in pricing disclosure, OMVIC fee explanation, Ontario documentation fee transparency
- Mystery shopper protocol — professional tone, product knowledge accuracy, no misleading statements, follow-up offer on every call

---

## Technical Stack

| Component | Details |
|---|---|
| **Voice AI Platform** | Vapi.ai — inbound call handling, agent deployment |
| **LLM** | Iteratively selected and tuned across Vapi-available models |
| **RAG Backend** | Custom FastAPI server — live inventory and knowledge retrieval |
| **Knowledge Base** | Dealership inventory, promotions, FAQs, financing programs, CPO terms |
| **Tool Calling** | FastAPI endpoints called at inference time for real-time data retrieval |
| **Call Volume** | 500+ inbound calls/day |

---

## Results & Impact

| Metric | Result |
|---|---|
| Hallucinations Eliminated | **Yes** — after 3-month iterative prompt engineering process |
| Knowledge Accuracy | **Live** — daily inventory and promotion updates reflected in real time |
| Inbound Calls Handled | **500+/day** — fully automated |
| Stabilization Timeline | **3 months** of weekly prompt refinement cycles |
| Mystery Shopper Compliance | **Built-in** — professional tone, transparency, CTA on every call |

- The agent went from reading instructions out loud to conducting complete, natural inbound sales conversations — a fundamental behavioral transformation achieved through structured, data-driven prompt iteration rather than platform switching.
- Real-time RAG eliminated the inventory accuracy problem entirely — callers received current information on every call regardless of what changed on the dealership website the night before.
- The solution stayed within the client's committed Vapi.ai platform — no migration required, no sunk-cost write-off.

---

## Challenges & How We Solved Them

| Challenge | Solution |
|---|---|
| Agent reading system prompt instructions aloud instead of conversing naturally | Rebuilt prompt architecture to separate behavioral instructions from knowledge content; iteratively refined over 3 months based on real call failure patterns |
| No LLM configuration fitting the automotive dealership use case cleanly | Systematic testing of available Vapi LLM options against real call scenarios — selected and tuned the best-fit model through evidence-based iteration |
| Daily inventory changes making static knowledge base inaccurate within hours | Built a custom FastAPI RAG backend with tool calling — agent retrieves live inventory and promotion data at inference time |
| High call volume (500+/day) with complex, multi-topic caller inquiries | Full knowledge base structured into retrievable records — financing, CPO warranty, service hours, trade-in, special programs — all accessible via tool call |
| Canadian compliance requirements (all-in pricing, OMVIC fee disclosure) | Compliance handling embedded into conversation flow — agent proactively discloses fees and pricing terms as required by Ontario law |

---

*Client identity and commercial details are protected under NDA in accordance with LeapsDev's white-label service model.*

**LeapsDev.com · sufyan-sufy@leapsdev.com**
