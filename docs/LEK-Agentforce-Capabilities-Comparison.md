# LEK Agentforce: Differences, Current Capabilities & Advantages

**Client:** LEK (LEAK)  
**Baseline:** Agentforce as implemented mid-2025  
**Comparison point:** Current Agentforce 360 capabilities (as of September 2026)  
**Purpose:** Clear side-by-side view of what LEK has today, what the platform can do now, and why upgrading matters

---

## 1. One-line difference

| Mid-2025 LEK Agentforce | Current Agentforce |
| --- | --- |
| Chat that **finds and creates** Account, Contact, and Opportunity | Governed AI coworker that **researches, briefs, updates, follows up, and answers from firm knowledge** — with control, testing, and observability |

---

## 2. What LEK has today (mid-2025)

| Capability | Status | What it means in practice |
| --- | --- | --- |
| Identify Salesforce records | Yes | Resolve “Acme” to the right Account / Contact / Opportunity |
| Query Salesforce records | Yes | Ask questions about data already in CRM |
| Create Account | Yes | Spin up a new company record from conversation |
| Create Contact | Yes | Capture a person against an account |
| Create Opportunity | Yes | Open a new pursuit / deal |
| Update records | No (not in scope) | Stage, amount, next step still done in the UI |
| Summarize / research accounts | No | Partners still prep meetings manually |
| Knowledge / playbooks | No | Agent cannot answer from firm methodology content |
| Draft email / log tasks | No | Outreach and follow-ups stay outside the agent |
| Deterministic process control (Agent Script) | No | Behavior relies on LLM + topic instructions |
| Testing Center / Command Center | Limited / not adopted | Hard to measure quality and expand writes safely |

**Bottom line today:** strong **data-entry and lookup** assistant. Most of the sales cycle (prep, hygiene, outreach, knowledge) is still human-only.

---

## 3. Current Agentforce capabilities (available to enhance LEK)

Grouped by what matters for a consulting / BD Salesforce org.

### A. CRM record work (beyond create)

| Capability | Description |
| --- | --- |
| Get Record Details | Pull full context for a named record |
| Summarize Record | Natural-language brief of Account / Contact / Opportunity |
| Update Record | Change fields (stage, close date, next step) with confirmation |
| Query Records with Aggregate | Pipeline totals, counts, rollups in conversation |
| Extract Fields and Values | Turn pasted notes/email into structured field updates |
| Get Activities Timeline | Last touches, stale relationships |
| Create Task / Event (via Flow) | Log follow-ups from chat |

### B. Sales research & account intelligence

| Capability | Description |
| --- | --- |
| Sales Research subagent | Package of research actions for Account / Lead |
| Web search (Data Library) | Governed external context for prep |
| Related emails, notes, activities | Synthesize what the firm already knows |
| Account plan / record research prompts | Always-on account intelligence in CRM & Slack |
| Conversation intelligence (if licensed) | Insights from calls/meetings |

### C. Knowledge & unstructured content

| Capability | Description |
| --- | --- |
| Agentforce Data Libraries | Ground answers in Knowledge articles and files |
| Answer Questions with Knowledge | Q&A with citations |
| Intelligent Context (Data 360) | Structure PDFs, decks, tables for agent use |

### D. Outreach & channels

| Capability | Description |
| --- | --- |
| Draft or Revise Email | CRM-grounded drafts (send stays with the human) |
| Recommended in-record actions | One-click “Summarize Opportunity” on the record page |
| Slack / mobile / Sales Workspace | Agent where partners already work |
| Conversational email / Voice | Optional later for volume or hands-free use cases |

### E. Control, quality & operations

| Capability | Description |
| --- | --- |
| Agent Script (hybrid reasoning) | If/else, variables, mandatory duplicate checks, action chaining |
| Subagents (formerly topics) | Separate jobs: record mgmt, research, knowledge, outreach |
| Testing Center | Automated utterance and knowledge-based tests |
| Command Center / Observability | Usage, success rates, traces, cost |
| MCP / MuleSoft | Connect external systems when Salesforce alone is not enough |
| Multi-agent orchestration | Specialized agents hand off with shared context |
| Long-horizon runtime | Multi-day/week BD goals with human checkpoints (evaluate as GA/SKU allow) |

---

## 4. Side-by-side differences

| Area | Mid-2025 LEK | Current Agentforce | Difference |
| --- | --- | --- | --- |
| **Primary job** | Create & find records | Execute sales work under guardrails | From data entry → coworker |
| **Account / Contact / Opportunity** | Create only | Create + read details + summarize + update | Full lifecycle |
| **Meeting prep** | Manual / query fields | Research brief (CRM + notes + optional web) | Hours → minutes |
| **Pipeline hygiene** | User updates UI | Conversational update + tasks | Higher CRM completeness |
| **Firm knowledge** | Not used | Data Libraries + citations | Consistent methodology answers |
| **Email** | Not in agent | Draft/revise grounded in CRM | Faster, on-brand outreach |
| **Process rules** | Prompt instructions | Agent Script (deterministic) | Fewer bad creates / wrong updates |
| **Duplicate risk** | Soft guidance | Hard Identify → Query → Create chain | Cleaner data |
| **Trust to expand** | Manual UAT | Testing Center + Observability | Safe to add write actions |
| **Where users work** | Salesforce chat | Salesforce + Slack + mobile + Sales Workspace | Higher adoption |
| **Scope of work** | Single turn | Multi-step plans; later multi-week goals | Bigger outcomes per request |
| **Integrations** | Salesforce only | MCP / MuleSoft when needed | Enterprise systems in the loop |

---

## 5. Advantages of moving to current capabilities

### For partners / BD

| Advantage | Why it matters |
| --- | --- |
| Faster meeting prep | Research + summarize replace hunting across records and notes |
| Less CRM admin | Update stage, next step, and tasks in conversation |
| Better outreach | Email drafts grounded in Opportunity and Contact context |
| Answers from firm IP | Playbooks and proposal language available in the flow of work |
| Work in Slack / mobile | Agent meets users where they already collaborate |

### For CRM & data quality

| Advantage | Why it matters |
| --- | --- |
| Duplicate prevention | Scripted identify/query before create |
| Completer Opportunities | Updates and required-field Flows, not create-and-forget |
| Auditable actions | Observability and Trust Layer visibility |
| Controlled writes | Confirmations on stage, amount, Closed Won/Lost |

### For IT / Salesforce platform owners

| Advantage | Why it matters |
| --- | --- |
| Hybrid control (Agent Script) | Predictable behavior without abandoning generative AI |
| Prebuilt Sales Research / Account Management | Less custom build vs mid-2025 greenfield |
| Testing Center | Regression before production changes |
| Command Center | Adoption, failure patterns, and ROI visibility |
| Extensibility | MCP/MuleSoft only when native actions are not enough |

### For leadership

| Advantage | Why it matters |
| --- | --- |
| Higher agent ROI | Same license covers research, hygiene, and outreach — not only create |
| Measurable impact | Prep time, duplicate rate, pipeline hygiene, action success |
| Scalable BD support | Prospecting and long-horizon goals when ready — without re-platforming |
| Risk managed growth | Expand autonomy only where tests and confirmations allow |

---

## 6. Capability maturity map

```
Mid-2025 LEK (today)
│  Identify · Query · Create Account/Contact/Opportunity
│
├─► Phase 1 — Trusted CRM coworker
│     + Details · Summarize · Update (confirmed) · Tasks · Agent Script · Testing
│
├─► Phase 2 — Research & knowledge
│     + Sales Research · Data Library · Draft email · Slack
│
└─► Phase 3 — Growth
      + Prospecting · Sales Workspace · Multi-agent · Long-horizon BD goals
```

| Maturity | What users can say | Advantage unlocked |
| --- | --- | --- |
| **Today** | “Create Opportunity Acme – FS diligence” | Speed of record creation |
| **Phase 1** | “Move Acme to Proposal, set follow-up Friday, summarize risks” | Hygiene + briefing without leaving chat |
| **Phase 2** | “Brief me on Acme for Thursday; draft email to the CFO using our FS scope language” | Prep + grounded communication |
| **Phase 3** | “Re-engage stale pipeline this quarter; show prioritized prospects in Slack” | Always-on BD leverage |

---

## 7. Advantages vs cost of staying on mid-2025 scope

| If LEK stays as-is | If LEK adopts current capabilities |
| --- | --- |
| Agent useful mainly for coordinators creating records | Agent useful for partners preparing and advancing deals |
| CRM still incomplete after create | Conversational updates improve pipeline quality |
| Prep and email stay outside Salesforce AI | Prep and drafts happen inside governed Agentforce |
| Hard to prove AI ROI beyond “records created” | Metrics: prep time, hygiene, email accept rate, action success |
| Custom instructions fragile as use grows | Agent Script + Testing Center scale safely |
| New Salesforce Sales AI features unused | Prebuilt Account Management / Research adopted, not rebuilt |

---

## 8. Summary table for stakeholders

| Question | Answer |
| --- | --- |
| **What do we have?** | Identify, Query, Create Account / Contact / Opportunity |
| **What is different now?** | Full lifecycle, research, knowledge, email, Script, testing, multi-channel |
| **What is the main advantage?** | Move from “AI that types into CRM” to “AI that helps win and run pursuits under control” |
| **What should we do first?** | Add summarize/update/tasks + Agent Script + Testing/Observability on the three objects you already create |
| **What can wait?** | Prospecting, voice, multi-agent orchestration, long-horizon goals |

---

## Related document

For phased roadmap, architecture, governance, and metrics, see [LEK Agentforce Enhancement Roadmap](./LEK-Agentforce-Enhancement-Roadmap.md).

---

*License and GA status should be confirmed in the LEK Salesforce org before implementation planning.*
