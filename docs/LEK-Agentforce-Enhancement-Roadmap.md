# LEK Agentforce Enhancement Roadmap

**Audience:** LEK Salesforce stakeholders, CRM product owners, and implementation partners  
**Prepared as:** Salesforce architecture recommendation  
**Baseline:** Agentforce implementation as of mid-2025  
**Target platform:** Agentforce 360 / Customer 360 as of September 2026 (Winter ’26 through Summer ’26, plus announced long-horizon capabilities)

---

## 1. Executive summary

LEK’s current Agentforce deployment is a **first-generation employee agent**: it can identify and query Salesforce records and **create Account, Contact, and Opportunity**. That was a sound mid-2025 starting point. The platform has since moved from “chat that writes records” to **governed, hybrid-reasoning agents** that research accounts, update CRM in the flow of work, draft outreach, run multi-step plans, and operate across Salesforce, Slack, email, and (optionally) voice.

The highest-value next step is **not** adding more create-object actions in isolation. It is to:

1. Complete the **sales record lifecycle** (read → summarize → update → task/email, not only create).
2. Ground the agent in **firm knowledge and unstructured content** (Data 360 / Agentforce Data Libraries / Intelligent Context).
3. Adopt **hybrid control** (Agent Script, confirmations, Testing Center, Command Center / Observability) so BD and delivery teams can trust the agent with writes.
4. Expand from a single generalist topic set into **purpose-built sales subagents**, then into **multi-agent orchestration** where justified.

Recommended outcome in 90 days: a production employee agent that can **find, brief, update, and follow up** on Accounts, Contacts, and Opportunities, with human confirmation on sensitive writes, measured in Command Center.

---

## 2. Current state (mid-2025 baseline)

| Capability | Typical mid-2025 Agentforce pattern | LEK today |
| --- | --- | --- |
| Agent type | Employee agent in Salesforce (Atlas / topic + action model) | In place |
| Record identification | Standard **Identify Record by Name** | In place |
| Query | Standard **Query Records** | In place |
| Writes | Custom or standard **Create** for Account, Contact, Opportunity | In place |
| Updates / deletes | Often not enabled to reduce risk | Assumed not in scope |
| Knowledge / RAG | Optional Einstein Knowledge or early Data Libraries | Not described — treat as gap |
| Email / activity | Draft email / log tasks often not wired | Not described — treat as gap |
| Testing / observability | Limited preview; Testing Center and Command Center still maturing in 2025 | Treat as gap |
| Deterministic control | Topic instructions + LLM tool choice | Pre–Agent Script |

**Architectural implication:** the agent is a **CRM data-entry and lookup copilot**. Users still do research, judgment, follow-up, and process enforcement outside the agent. That leaves most seller time (prep, next-best action, hygiene, outreach) unassisted.

---

## 3. What changed since mid-2025 (platform context)

Use this as the “why now” briefing for LEK leadership. Dates are Salesforce public announcements, not LEK license confirmation.

| Wave | When | What matters for LEK |
| --- | --- | --- |
| **Agentforce 3** | June 2025 | Command Center observability, Testing Center enhancements, 100+ additional prebuilt actions, **MCP** interoperability, Web Search in Data Libraries, faster Atlas responses |
| **Agentforce 360** | October 2025 | New **Agentforce Builder**, **Agent Script** (hybrid reasoning), **Agentforce Voice**, **Intelligent Context** for unstructured documents, Data 360 grounding |
| **Winter ’26** | October 2025 GA | **Agentforce Grid** (bulk prompt + action experiments), pipeline-management agent enhancements, Builder beta, observability |
| **Spring ’26** | February 2026 | **Sales Workspace**, **Account Management** (automated research, summaries, next steps), **Prospecting** from web/external signals, conversational / two-way email, Agentforce Builder as the primary build surface |
| **April 2026** | Platform rename | Topics are **subagents** (same idea, clearer multi-skill design) |
| **Summer ’26** | June 2026 | **Multi-agent orchestration**, Agentforce Self-Service (if client-facing later), Slack-first agentic work, Data 360 agentic setup (GA path) |
| **Job-ready / long-horizon** | 2026 (Hunter first; Optimizer GA targeted October 2026) | Agents that hold a **goal for days/weeks**, memory, durable execution, human checkpoints; Optimizer that tunes agents from production traces |

**Net shift:** mid-2025 agents were conversational tools over CRM. Current Agentforce is a **controlled execution layer** over Customer 360 + Data 360, with deterministic script, observability, and longer-running sales work.

---

## 4. Gap analysis vs LEK’s current agent

| Domain | Mid-2025 LEK agent | Current Agentforce capability | Business impact if adopted |
| --- | --- | --- | --- |
| CRM CRUD | Create Account / Contact / Opportunity | **Update Record**, Get Record Details, Summarize Record, Query with aggregates, Extract Fields and Values | Hygiene, stage moves, field completion without leaving chat |
| Sales research | Query what is already in Salesforce | **Sales Research** subagent: web search, related emails, notes, activities, conversation intelligence, account plan, Get Record Research | Meeting prep in minutes; consistent account briefs |
| Prospecting | User must find names, then create records | **Prospecting** + Data 360 / web signals; prioritized leads in CRM and Slack | Always-on BD pipeline support |
| Communication | Not in baseline | **Draft or Revise Email**, Conversational Email, recommended in-record actions | Faster, grounded outreach with CRM context |
| Activity | Not in baseline | Get Activities Timeline, create Tasks/Events via Flow actions | Follow-ups actually get logged |
| Knowledge | CRM fields only | Data Libraries, **Answer Questions with Knowledge**, Intelligent Context (PDF, tables, decks) | Playbooks, proposal templates, methodology Q&A |
| Control | LLM picks tools from topics | **Agent Script**: if/else, variables, action chaining, deterministic transitions | Fewer wrong-object creates; mandatory duplicate checks |
| Quality | Manual UAT | **Testing Center** (incl. tests from knowledge), session traces, **Command Center / Observability**, Agent Optimizer (near-term) | Safe expansion of write actions |
| Channels | Salesforce UI | Slack, mobile, Sales Workspace, optional Voice | Agents where partners actually work |
| Scale of work | Single turn | Action chaining, Grid for bulk, **long-horizon runtime** for multi-week BD goals | “Rescue at-risk pipeline by quarter-end” not just “create this opp” |
| Integration | Salesforce-only | MCP, MuleSoft Agent Fabric, A2A, External Services | Enrichment, marketing, finance, document systems |

---

## 5. Target architecture (recommended)

Keep a **single employee-facing Sales agent** as the user experience. Behind it, split work into **subagents** (formerly topics) with explicit actions and guardrails.

```
                    ┌─────────────────────────────────────┐
                    │  Channels: Salesforce, Slack, Mobile │
                    │  (optional: Voice, Conversational    │
                    │   Email, Sales Workspace)            │
                    └──────────────────┬──────────────────┘
                                       │
                    ┌──────────────────▼──────────────────┐
                    │  LEK Sales Employee Agent            │
                    │  Agent Script + hybrid reasoning     │
                    │  User perms + Einstein Agent User    │
                    └──────────────────┬──────────────────┘
           ┌───────────────┬───────────┼────────────┬──────────────┐
           ▼               ▼           ▼            ▼              ▼
    Record Mgmt      Sales Research  Knowledge    Outreach     Orchestration
    Identify         Web + CRM       Data Library Draft Email  Handoff /
    Query            Notes/Emails    Intelligent  Tasks        other agents
    Create           Account Plan    Context      Cadence
    Update           Summaries
           │               │           │            │              │
           └───────────────┴───────────┴────────────┴──────────────┘
                                       │
                    ┌──────────────────▼──────────────────┐
                    │ Customer 360 objects + Data 360      │
                    │ Flows / Prompt Builder / Apex        │
                    │ MCP / MuleSoft (external systems)    │
                    │ Testing Center + Observability       │
                    └─────────────────────────────────────┘
```

**Design principles**

1. **Same user permissions as the human** — the agent never bypasses sharing, FLS, or validation.
2. **Read freely, write with confirmation** — especially create/update on Opportunity amount, stage, close date, and Account ownership.
3. **Duplicate-before-create** — Identify + Query (and matching rules) must run before any Create action (Agent Script, not “hope the LLM remembers”).
4. **Ground every generative action** — summaries and emails must pull CRM + library context; no ungrounded commercial claims.
5. **One job per subagent** — do not overload a single “General CRM” topic with research, writes, and policy Q&A.

---

## 6. Recommended capability enhancements

### 6.1 Complete the Account–Contact–Opportunity lifecycle (priority 1)

LEK already creates the three core objects. Unlock the rest of the standard action catalog:

| Action | Use for LEK |
| --- | --- |
| **Get Record Details** | “What’s on this account?” without a SOQL-style conversation |
| **Summarize Record** (Prompt Template) | Opportunity briefing for partners; account relationship snapshot |
| **Update Record** | Stage, next step, close date, key fields — with confirmation |
| **Query Records with Aggregate** | “How much pipeline in FS for this quarter?” |
| **Extract Fields and Values** | Parse a partner’s pasted notes or email into fields |
| **Get Activities Timeline** | Last touch, stale relationships |
| Create **Task / Event** (Flow action) | “Set a follow-up Friday with Jane at Acme” |
| **Lead** identify / create / convert (if used) | Inbound BD before Opportunity |

**Opportunity-specific prompts to add**

- Summarize Opportunity (stage, stakeholders, risks, next step).
- Suggest next best action (grounded in activity recency and amount).
- Draft win/loss notes into a structured field set.

**Guardrails**

- Require user confirmation for: new Opportunity, stage change to Closed Won/Lost, amount above a threshold, Account owner change.
- Block delete and merge from the agent (keep in UI / data steward process).
- Enforce required fields via Flow before DML, not via prompt text alone.

### 6.2 Agentforce Account Management / Sales Research (priority 1)

Salesforce now ships an **Account Management** job with a **Sales Research** subagent. Enable and tailor rather than rebuilding:

- Search the Web (governed Data Library web search).
- Get Related Emails / Notes / Activities.
- Get Conversation Intelligence (if Einstein Conversation Insights is licensed).
- Get Account and Account Plan.
- Get Record Research + Record Research prompt templates.

**LEK-specific instructions** to add: industry taxonomy, competitor set, “do not invent fees or proposal language,” cite Salesforce record IDs in briefs.

**Outcome:** before a client meeting, a partner asks: “Brief me on Acme — last 90 days, open pipeline, who we know, and suggested agenda.” The agent synthesizes CRM + notes + optional web context instead of returning raw query rows.

### 6.3 Knowledge and unstructured content (priority 1)

Mid-2025 query actions only see structured CRM. Current Agentforce expects a **Data Library**:

- Knowledge articles: BD playbooks, industry one-pagers, CRM how-to.
- Uploaded files: proposal templates, credential slides, methodology PDFs.
- **Intelligent Context** (Data 360): tables, decks, multi-perspective extraction.
- **Answer Questions with Knowledge** with citations.

This is the difference between “create an Opportunity named Acme” and “draft an Opportunity description using our FS due-diligence scope language.”

### 6.4 Outreach and activity capture (priority 2)

| Capability | Recommendation |
| --- | --- |
| **Draft or Revise Email** | Ground in Contact + Opportunity; never auto-send in phase 1 |
| Conversational / two-way email | Phase 2 if marketing/service volume justifies it |
| Slack | Surface the same employee agent where deal rooms live |
| Logging | After draft, offer “log as Task/Email” via Flow |

### 6.5 Prospecting and pipeline (priority 2)

If LEK’s model includes proactive BD (not only inbound):

- Turn on **Prospecting** patterns: enrich Salesforce with allowed web/external signals, prioritized lead lists in CRM and Slack.
- Use **Sales Workspace** so reps see agent activity, priorities, and next actions in one hub.
- Later: **long-horizon** style goals (“re-engage stale pipeline this quarter”) with human approval gates — evaluate **Hunter** / long-horizon runtime when LEK licenses and GA status align.

### 6.6 Hybrid reasoning with Agent Script (priority 1, parallel to features)

Rebuild topic instructions into **subagents + Agent Script**:

- Variables: `matched_account_id`, `duplicate_found`, `user_confirmed_write`.
- Deterministic chain: Identify → Query duplicates → (if none) Create; else Update or stop.
- LLM tools only where judgment is needed (which Contact is the economic buyer).
- Action chaining: Get Record Details → Summarize → Draft Email.

This is the single biggest quality upgrade versus a mid-2025 “bag of actions.”

### 6.7 Multi-agent design (priority 3)

Do **not** split into many user-facing agents on day one. When volume grows:

- Employee Sales agent (BD / CRM).
- Knowledge / enablement subagent.
- Optional Service / internal ops agent.
- **Multi-agent orchestration** (Summer ’26) for handoffs with shared context (e.g. “this Opportunity needs a staffing request” → ops agent).

### 6.8 Enterprise connectivity (priority 3, as systems require)

Use **MCP** and **MuleSoft Agent Fabric** only for systems of record outside Salesforce (document repository, marketing automation, finance). Prefer native actions first. Every MCP tool inherits the same confirmation and logging rules.

### 6.9 Channels and UX

- Keep Salesforce record-page **recommended actions** (e.g. Summarize Opportunity on the Opportunity page).
- Add **Slack** for partners who live in deal channels.
- Mobile for on-the-road lookup and logging.
- Voice only if there is a clear call-center or hands-free use case (usually not first for consulting BD).

---

## 7. Suggested subagent catalog (phase 1–2)

| Subagent | Intent examples | Core actions |
| --- | --- | --- |
| **Record management** | Create/update Account, Contact, Opportunity; find duplicates | Identify, Query, Create (existing), Update, Extract Fields |
| **Sales research** | Brief this account/opportunity | Research pack: details, activities, notes, web, summarize |
| **Knowledge** | How do we staff FS due diligence? What’s the proposal outline? | Answer Questions with Knowledge |
| **Outreach** | Draft email to CFO; log follow-up | Draft/Revise Email, Task Flow |
| **Pipeline insights** | Pipeline by industry / partner | Query with aggregate, optional Grid for bulk summarization |

---

## 8. Governance, testing, and operations

These are no longer optional if write-scope expands.

| Control | Practice |
| --- | --- |
| **Einstein Trust Layer** | Retention, masking, audit; confirm LEK data residency |
| **Agent user + permission sets** | Least privilege; Prompt Template User / Run Flows as required |
| **Confirmations** | Standard action confirmations + Script flags for high-risk DML |
| **Testing Center** | Utterance packs: create with duplicate, update stage, “who is the buyer,” knowledge Q&A; generate tests from Data Library |
| **Command Center / Observability** | Topic/action success, latency, drop-off, cost (AWU) |
| **Agent Optimizer** | Plan adoption when GA (targeted October 2026) for trace-driven instruction fixes |
| **Change management** | Version subagents in a sandbox; never edit production instructions ad hoc |
| **Einstein Grid** | Use in sandbox to bulk-test research prompts across a sample of Accounts |

---

## 9. Phased roadmap

### Phase 0 — Discover (1–2 sprints)

- Inventory current topics, actions, Flows, prompt templates, and object permissions.
- Confirm licenses: Agentforce for Sales, Data 360, Slack, Conversation Insights, Account Management.
- Baseline metrics: weekly active users, successful creates, failed/hallucinated records, time-to-create Opportunity.

### Phase 1 — Trusted CRM copilot (first release)

- Add Get Details, Summarize, Update (confirmed), Activities, Task create.
- Duplicate-prevention Script around existing Create actions.
- Testing Center + Observability live.
- Record-page recommended actions.

**Exit criteria:** partners can brief and update an Opportunity without leaving the agent; zero unconfirmed Closed Won; duplicate create rate down.

### Phase 2 — Research and knowledge

- Account Management / Sales Research subagent.
- Data Library (playbooks + selected PDFs) + Intelligent Context if decks/tables matter.
- Draft email (no auto-send).
- Slack channel (if licensed).

**Exit criteria:** meeting prep time reduced; knowledge answers cited; email drafts accepted with light edit.

### Phase 3 — Growth and orchestration

- Prospecting / enrichment where legally and commercially approved.
- Sales Workspace for leadership visibility of agent work.
- Multi-agent handoff only if a second domain (staffing, finance, service) is real.
- Evaluate long-horizon BD goals and Optimizer.

---

## 10. What not to do

- Do not add Create for every object (Quote, Contract, custom project objects) until Update + confirmations work on the three core objects.
- Do not enable unattended send of client email from the agent in phase 1.
- Do not dump all unstructured files into a library without retention, confidentiality, and citation rules (consulting content is often client-confidential).
- Do not skip Agent Script and rely on longer topic instructions — that is the mid-2025 failure mode.
- Do not treat Einstein Copilot-era “suggest and human clicks” as the end state; current Agentforce is expected to **execute** under guardrails.

---

## 11. Success metrics

| Metric | Why it matters |
| --- | --- |
| Weekly active agent users (partners vs coordinators) | Adoption |
| Time from “new pursuit” to complete Opportunity (required fields) | Process speed |
| Duplicate Account/Contact/Opportunity rate | Data quality |
| % of Opportunities with next step + future Task | Hygiene |
| Research session → meeting held (proxy) | Prep value |
| Knowledge citation rate / thumbs-down | Grounding quality |
| Action success rate and escalation rate (Observability) | Reliability |
| Human edit distance on drafted emails | Generative quality |

---

## 12. Prerequisites and risks

| Item | Risk if ignored |
| --- | --- |
| Data quality (naming, duplicates, incomplete Opportunities) | Agent amplifies bad CRM |
| Matching rules / duplicate jobs | Creates proliferate |
| License mix (Agentforce Sales SKU, Data 360, Slack) | Design that cannot be turned on |
| Sharing model for sensitive engagements | Over-exposure via Query |
| Prompt injection via notes/emails | Research subagent must treat untrusted text as data, not instructions |
| Change control | Instruction drift in production |

---

## 13. Recommended decision for LEK

**Position to the client:** the mid-2025 agent proved CRM create/query. The 2026 platform makes it realistic to run a **governed sales coworker**: research, summarize, update, follow up, and answer from firm knowledge — still with humans approving commercial writes.

**Ask:** approve Phase 1 (lifecycle + Script + testing) immediately; parallel-track Data Library scoping for Phase 2; defer prospecting, voice, and multi-agent until Phase 1 quality is green.

---

## Appendix A — Current vs target action map

| Object / job | Mid-2025 | Target Phase 1 | Target Phase 2 |
| --- | --- | --- | --- |
| Account | Create, Identify, Query | + Details, Summarize, Update, Activities | + Research pack, web, account plan |
| Contact | Create, Identify, Query | + Details, Update, related to Account | + Email draft, stakeholder map |
| Opportunity | Create, Identify, Query | + Summarize, Update, next step Task | + Pipeline aggregates, at-risk briefing |
| Lead | — | If in data model: Identify/Create | Convert + prospecting |
| Task/Event | — | Create follow-up | Cadence support |
| Knowledge | — | — | Q&A with citations |
| Email | — | — | Draft/revise, log activity |
| Case | — | Only if internal ops needs it | Service agent later |

## Appendix B — Source notes (Salesforce public)

Architects should re-validate GA/SKU on LEK’s org (Setup → Agentforce, and contract). Public references used for this brief:

- Agentforce 3 (June 2025): Command Center, MCP, expanded actions, Testing Center  
- Agentforce 360 / what-is-new: Builder, Agent Script, Voice, Intelligent Context  
- Winter ’26: Grid, observability, pipeline enhancements  
- Spring ’26: Sales Workspace, Account Management, Prospecting  
- Summer ’26: Multi-agent orchestration  
- Developer docs: Agent Script, actions as Flow / Apex / Prompt Template; topics renamed subagents (April 2026)  
- Long-horizon runtime and Agent Optimizer: announced 2026; Optimizer GA called out for October 2026  

---

*This document is an architecture recommendation. Implementation should be estimated after org discovery (licenses, data model, existing topics/actions, and security model).*
