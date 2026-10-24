# MLOps Product Requirements Document (PRD) Template

### With an optional LLM / agent integration module

**Template version:** 1.0 · **Issued:** 2026-10-24 · **Owner:** WhoIsJaneTurner, LLC

---

## 0. How to use this template

This is a reusable PRD skeleton for standing up — or extending — a production machine-learning platform for a client engagement. It is written so a product owner, ML lead and platform engineer can fill it in together and come out with a document that is specific enough to build from and testable enough to sign off against.

It is organised in two parts:

- **Part A — Core MLOps PRD (Sections 1–14).** Fill this in for every engagement. It covers the problem, the data, the platform, the model lifecycle and the governance that any ML system in production needs.
- **Part B — LLM integration module (Sections L1–L9).** An *optional* add-on. Complete it **only** when the system exposes a large-language-model or agent surface (an assistant, a copilot, RAG over your content, a tool-using agent, an LLM "interface" over existing models). If there is no LLM in scope, delete Part B and note the decision in Section 13.

**Conventions used below:**

- `⟨angle brackets⟩` mark a value the author must replace.
- **[FILL]** marks a section that must be completed before sign-off; **[OPTIONAL]** may be deleted if not applicable.
- `REQ-nn` identifiers are stable requirement anchors — keep the numbers so tests, tickets and the verification contract can reference them.
- A **basis tag** on key choices — *Sourced* (backed by a published standard), *Practice* (consistent with published practice), *Own design* (a house decision), *Starting value* (a number to tune against the client's own data) — mirrors the convention used across the firm's reference architectures, so a reader can tell a requirement grounded in a standard from a default we chose.

**Normative references — the frameworks this template operationalises.** This PRD does not restate those documents; it points to them and asks the author to record where the engagement diverges.

| Ref | Framework | Role in this PRD |
|---|---|---|
| **MLOPS-REF** | *MLOps reference architecture with an LLM interface* (v1.2) | Platform layers, environment taxonomy, stores and lineage, model lifecycle and promotion (Sections 6–8). |
| **RESP-AGENT** | *Responsible agent deployment and evaluation* (v0.4, draft) | Risk tiers, launch gates, operational controls, evaluation rubric and LLM-judge calibration (Part B). |
| **VERIFY** | *The verification contract* (v1.0) | What is guaranteed to be checked before a change merges (Section 9). |
| **ORCH** | *A well-built agent orchestrator* (v1.0) | Orchestration and fleet control where agents write code or run jobs (Sections 8, L7). |

---

## Document control

| Field | Value |
|---|---|
| Project / product name | ⟨name⟩ |
| Client | ⟨client⟩ |
| PRD author(s) | ⟨author⟩ |
| Document status | Draft / In review / Approved |
| Version | ⟨0.1⟩ |
| Last updated | ⟨YYYY-MM-DD⟩ |
| Approvers (name, role, date) | ⟨…⟩ |
| LLM module in scope? | ☐ Yes (complete Part B) ☐ No (delete Part B) |

**Change log**

| Version | Date | Author | Change |
|---|---|---|---|
| 0.1 | ⟨…⟩ | ⟨…⟩ | Initial draft |

---

# Part A — Core MLOps PRD

## 1. Problem statement and objectives **[FILL]**

**1.1 Problem.** `REQ-01` — In one paragraph, what decision or workflow is this system meant to improve, for whom, and what does "better" mean in the client's terms? Avoid naming a technique here; name the outcome.

**1.2 Current state.** How is this done today (manual process, legacy model, spreadsheet, nothing)? What is the measurable pain — cost, latency, error rate, missed revenue, compliance exposure?

**1.3 Objectives and non-goals.**

| ID | Objective (outcome, not feature) | Measure | Target | Basis |
|---|---|---|---|---|
| OBJ-1 | ⟨…⟩ | ⟨…⟩ | ⟨…⟩ | ⟨Starting value⟩ |

`REQ-02` — **Non-goals.** List explicitly what this system will *not* do. A short non-goals list prevents scope creep and sets up the "excluded zone" of the verification contract (Section 9).

**1.4 Business case summary.** Expected value, rough cost envelope (Section 14), and the decision this PRD is asking for.

---

## 2. Stakeholders, roles and decision rights **[FILL]**

`REQ-03` — Name the people, not just the teams. A production ML system fails most often at the handoffs, so make decision rights explicit.

| Role | Name | Responsibility | Decision rights |
|---|---|---|---|
| Product owner | ⟨…⟩ | Priorities, acceptance | Approves scope |
| ML / data science lead | ⟨…⟩ | Modelling, evaluation | Proposes promotion |
| **Model approver / analyst** | ⟨…⟩ | Reviews metrics, signs promotion | **Approves production promotion** (Section 8) |
| Platform / MLOps engineer | ⟨…⟩ | Pipelines, environments, stores | Owns platform code |
| Data owner / steward | ⟨…⟩ | Source data, access, retention | Approves data use |
| Risk / legal / compliance | ⟨…⟩ | Governance sign-off | Approves launch against policy |
| Business sponsor | ⟨…⟩ | Funding, value | Funds / cancels |

`REQ-04` — **The approval that cannot be delegated to the system.** Model promotion and any high-consequence action are signed, identity-bound human actions — never an action the pipeline or an agent can take or relay on a human's behalf *(Sourced: MLOPS-REF, RESP-AGENT)*. Record here who holds that identity and through which control (operator UI, signed approval in the interface).

---

## 3. Scope and context **[FILL]**

**3.1 In scope / out of scope.** `REQ-05` — Bullet the capabilities in and out of this release.

**3.2 Users and consumers.** Who or what calls the system — a UI, a batch job, another service, an agent? Expected volume and concurrency.

**3.3 System context diagram.** `REQ-06` — A simple diagram of upstream data sources, the platform, and downstream consumers. Show which boundaries carry PII or regulated data.

**3.4 Assumptions, dependencies and constraints.** External systems, data availability, licensing, client infrastructure (cloud/on-prem/hybrid), and any hard constraints (data residency, air-gap, latency ceiling).

---

## 4. Success metrics, SLOs and acceptance criteria **[FILL]**

`REQ-07` — Separate three kinds of metric and do not blend them into one score:

| Kind | Example | Target | Measured where |
|---|---|---|---|
| **Business / outcome** | Reduction in manual review hours | ⟨…⟩ | ⟨…⟩ |
| **Model quality** | Precision / recall / calibration / holdout performance | ⟨…⟩ *(Starting value)* | Eval store, on the model version |
| **Operational / SLO** | p95 latency, availability, throughput, cost per 1k requests | ⟨…⟩ | Monitoring |

**4.1 Acceptance criteria.** `REQ-08` — The system is accepted when: ⟨list testable conditions tied to the IDs above⟩.

**4.2 Guardrail metrics.** Metrics that must *not* regress even if the headline metric improves (fairness across groups, error on a protected slice, cost ceiling).

---

## 5. Data requirements and lineage **[FILL]**

**5.1 Sources.** `REQ-09` — For each source: owner, system of record, refresh cadence, access path, sensitivity classification, licence/consent basis.

**5.2 Input (pre-training / pre-run) checks.** `REQ-10` — Schema, row counts and distributions are checked against stated expectations **before** a training or scoring run; a failure stops the run and is recorded on the job *(Sourced: MLOPS-REF)*. List the expectations and the stop conditions.

**5.3 Lineage and stores.** `REQ-11` — Every model version carries its metadata and the feature definitions it was built from, so each environment's read path resolves the same results *(MLOPS-REF)*. Specify:

- **Feature catalog** — feature definitions attached to model versions.
- **Metadata store** — travels with the model version.
- **Data retention & deletion** — how long, and how a deletion request propagates.

**5.4 Data rule across environments.** `REQ-12` — Dev and staging may read production input data (or a sample) but never write to production data or production outputs. Registering a model and its eval evidence is the one write they share, by design *(Own design: MLOPS-REF)*. Confirm or amend for this client.

---

## 6. Platform and environment requirements **[FILL]**

The reference architecture classifies every platform element by **how it relates to environments**, not by a blanket "one copy per env". Use this taxonomy — it is the cheapest correct default for a small-to-mid team *(MLOPS-REF)*.

| Class | What belongs here | Environment behaviour | This engagement |
|---|---|---|---|
| **Built once, versioned** | ML framework, agent framework, platform client, shared libraries, (LLM knowledge / playbooks) | Published/reviewed once; each environment pins the version it uses | ⟨…⟩ |
| **Deployed per environment** | Orchestration engine + compute, model monitoring, **job store** | Same infra definition, different config | ⟨…⟩ |
| **Shared across environments** | Model registry (model + metadata + feature stores), tool registry, action log, eval store, LLM interface | One instance; every call/record names its environment | ⟨…⟩ |
| **Product-line code** | Client/product application code and models | Versioned like a library; models move through registry *stages*, not env copies | ⟨…⟩ |

**6.1 Environment rules.** `REQ-13`

- Two cheap environments, one real: **dev** (changing the platform itself), **staging** (prod at smaller scale), **prod** (full capacity).
- **Environment comes from config, never from code** — one setting selects every per-environment resource.
- Models train into the **shared** registry from whichever environment ran the training — uploaded once, never copied between registries.

**6.2 Store inventory.** `REQ-14` — Confirm each store, its scope and its owner:

| Store | Scope | Purpose |
|---|---|---|
| Model store | Shared | A model version is one object everywhere; stages are aliases on it |
| Metadata store | Shared | Travels with the model version |
| Feature catalog | Shared | Feature metadata attached to model versions |
| Tool registry | Shared | Tool definitions shared; access rules set per environment |
| Action log | Shared | One record of agent/operator actions; each entry names its environment |
| Eval store | Shared | Eval runs & feedback attach to a model/agent version |
| Job store | **Per environment** | Runs belong to the environment that executed them |

**6.3 Non-functional requirements.** `REQ-15` — Availability target, latency budget, throughput, scaling model, DR/RTO/RPO, residency.

---

## 7. Model / system requirements **[FILL]**

**7.1 Approach.** `REQ-16` — Model family / technique under consideration and why; interpretability needs; baseline to beat.

**7.2 Training & retraining.** `REQ-17` — Trigger (scheduled, drift-based, on demand), compute, reproducibility (seed, data snapshot, config), expected cadence.

**7.3 Serving.** `REQ-18` — Batch, real-time or streaming; interface contract (schema in/out); versioned API.

**7.4 Fallback & degradation.** `REQ-19` — What the system returns when the model is unavailable, low-confidence or out of distribution.

---

## 8. Model lifecycle, promotion and approval **[FILL]**

Promotion lives in the **model registry**, which owns exactly one thing: which version of each model holds each **stage**. This is the control plane for "what is live" *(MLOPS-REF)*.

**8.1 Stages.** `REQ-20`

| Stage | Meaning |
|---|---|
| **Candidate** | Registered, eval evidence attached, gates evaluated — waiting for the approver |
| **Production** | An approver signed off on the metrics; scoring resolves this alias |
| **Previous** | Retained with artifacts and evidence; can be promoted again |

**8.2 Promotion is an alias move.** `REQ-21` — Nothing is retrained or re-uploaded on promotion; reverting is promoting a previous version. A human is always in the loop. The registry **refuses a stage change without passing evidence on that exact version**.

**8.3 Who owns each part of promotion.** `REQ-22`

| Part of promotion | Where it lives |
|---|---|
| Which version holds each stage | Model registry (platform) |
| **Policy** — the gates a model must pass | Each product line, versioned with its models |
| **Evidence** — eval runs against those gates | Run by orchestration, stored on the model version |
| **Gate check** — evidence vs. policy | The registry, at stage-change time |
| **Decision** — the approval | A named analyst, after reviewing PR curves, calibration, holdout performance and the audit |
| **Consumption** — scoring uses "production" | Orchestration resolves the alias through the registry |

**8.4 The approval an agent cannot fake.** `REQ-23` — The pipeline (or agent) may *assemble* metrics and *request* promotion, but the approval is a signed-in action under the approver's identity, sent straight to the registry — never a message the agent relays. The registry records who approved, when and why *(Own design: MLOPS-REF)*.

**8.5 Orchestration.** `REQ-24` — Which workflow engine runs training, evaluation and scoring; how runs are recorded in the job store; where agents (if any) act under the orchestrator (see ORCH and Part B).

---

## 9. Verification contract **[FILL]**

Adopt a written **verification contract**: a short, named, discoverable statement of exactly which parts of the system are guaranteed to be checked before a change merges, by what, and how firmly — versus what merely *could* be checked if someone remembers *(VERIFY)*. This matters more as coding agents write more of the code: generating code stops being the hard part, and verifying it becomes the bottleneck.

> **The contract, in one line:** No change to a guarded surface merges without its corresponding check passing. Where the check is not automated, the pull request must show the check's output — not just assert correctness.

`REQ-25` — Complete the contract table:

| Guarded surface (where a bug would be silent or hard to reverse) | Check (runnable alone, named after the surface) | Enforcement (automated / manual) | Fidelity (live → mocked) |
|---|---|---|---|
| ⟨e.g. promotion gate logic⟩ | ⟨test name⟩ | ⟨automated⟩ | ⟨live⟩ |
| ⟨e.g. input-check thresholds⟩ | ⟨…⟩ | ⟨…⟩ | ⟨…⟩ |
| ⟨3–6 surfaces total⟩ | | | |

`REQ-26` — **Excluded zone.** State plainly what is known *not* to be covered, and why. This is a feature of the contract, not a gap in it.

`REQ-27` — **Agent configuration is production code.** Rules files, prompts and evaluation suites are version-controlled, changed by pull request and owned — not loose settings *(Sourced: VERIFY)*. Name their location and owner.

`REQ-28` — **Output vs. trajectory evaluation.** Where agents or pipelines act, specify both: output evaluation (was the final result right?) and trajectory evaluation (was the sequence of tool calls and reasoning right?). A fluent output that skipped its verification steps is more dangerous than one with a visible error.

---

## 10. Observability and monitoring **[FILL]**

`REQ-29` — Specify what is monitored, where the signal lives, and who is paged:

- **Operational:** latency (p50/p95/p99), error rate, availability, cost, saturation.
- **Model:** prediction distribution, input drift, concept drift, calibration over time.
- **Data:** freshness, volume, null/row-count anomalies (ties to Section 5.2).
- **Action log:** every operator and automated action, each entry naming its environment (Section 6).

`REQ-30` — **Alerting & on-call.** Thresholds, routing, runbooks, and the **measured time to switch off and roll back** a deployment *(Sourced: RESP-AGENT → Microsoft RAI Standard v2)*.

---

## 11. Security and privacy **[FILL]**

`REQ-31` — Authn/authz, secrets management, network boundaries, encryption at rest/in transit.
`REQ-32` — PII handling, data minimisation, consent basis, retention and deletion (ties to Section 5).
`REQ-33` — Tenant/environment isolation; least-privilege access to stores; who can read prod data.
`REQ-34` — Supply-chain: dependency pinning, model/artifact provenance, SBOM where required.

---

## 12. Governance, compliance and documentation **[FILL]**

Map the system to the governance frameworks the client is held to. These are complementary: **NIST AI RMF** is the risk-management method, **ISO/IEC 42001** the certifiable management system, and the **EU AI Act** the binding law for EU-market systems.

`REQ-35` — **NIST AI RMF** — record how the four functions are satisfied:

| Function | What to record here |
|---|---|
| **Govern** | Roles, policies, accountability (Section 2) |
| **Map** | Context, intended use, affected parties, risk tier (Section 3; L2) |
| **Measure** | Metrics, evaluations, test evidence (Sections 4, 9; L4–L5) |
| **Manage** | Controls, monitoring, incident response, decommissioning (Sections 10, 13; L6) |

`REQ-36` — **ISO/IEC 42001** — if the client seeks or holds certification, note the AI management-system artifacts this project produces (impact assessment, objectives, controls, review cadence).

`REQ-37` — **EU AI Act** — if any user or output touches the EU market: record the Act risk classification (prohibited / high-risk / limited / minimal), and the transparency obligations. *Note the LLM-specific disclosure duty in L9.*

`REQ-38` — **Sector rules.** Add any domain regime (financial, health, employment, etc.) and how it is met.

`REQ-39` — **Model/system documentation.** Produce and version a model card (and, for agents, an **agent card** — see L8), an impact assessment and an inventory entry *(Sourced: RESP-AGENT → Microsoft RAI Standard v2)*.

---

## 13. Risks, assumptions and open questions **[FILL]**

`REQ-40` — Risk register:

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| ⟨data drift degrades model⟩ | ⟨…⟩ | ⟨…⟩ | ⟨monitoring + retrain trigger⟩ | ⟨…⟩ |

`REQ-41` — Record the **decision on the LLM module**: in scope (complete Part B) or out of scope (and why).

---

## 14. Rollout, cost and operating plan **[FILL]**

**14.1 Phased rollout.** `REQ-42` — Shadow → limited → general availability, with the evidence required to move between phases and the rollback plan for each *(Practice; RESP-AGENT)*.

**14.2 Cost model.** `REQ-43` — Training compute, serving compute, storage, third-party/API costs, and a cost-per-unit-of-work figure to track against the SLO ceiling. (For LLMs, see L5 for token/latency economics.)

**14.3 Operating model.** `REQ-44` — Who runs this after handover; support hours; retraining ownership; how the verification contract and eval suites are maintained.

---

# Part B — LLM / agent integration module **[OPTIONAL]**

> Complete Part B only if the system exposes an LLM or agent surface. It extends — it does not replace — Part A: the LLM interface is a **shared** platform layer that sits over the same registry, stores and action log (Section 6), and everything an agent *does* still flows through the lifecycle, verification and governance above. The structure here follows the firm's responsible-agent framework (RESP-AGENT), which pairs a **deployment half** (risk tiering, launch gates, controls, governance) with an **evaluation half** (a scoring rubric and a calibrated judge), to be implemented together.

**Guiding principle — graduated autonomy.** `REQ-L01` — An agent earns the right to act more independently as evidence of reliability accumulates. Defaults are conservative; expansion happens through measurement, not assumption *(Own design: RESP-AGENT)*.

---

## L1. LLM interface and scope **[FILL]**

`REQ-L02` — What is the LLM *for* in this system? Pick the pattern(s): assistant/chat, retrieval-augmented answering (RAG), summarisation/extraction, an **LLM interface over existing ML models** (natural-language front end to registry/jobs/scoring), or a tool-using agent that takes actions.

`REQ-L03` — **One LLM interface across environments.** Each tool call targets an **explicit environment**, and access controls and trigger levels are set **per environment** — so the same agent can freely read dev results yet must ask before starting a prod job *(MLOPS-REF)*.

`REQ-L04` — **Model gateway.** Which base model(s)/provider(s), routing and fallback between them, version pinning, and where prompts and model config are stored (version-controlled — Section 9, `REQ-27`).

`REQ-L05` — **In/out of scope for the agent.** Mirror RESP-AGENT's scope: in scope are bounded tasks such as triage, FAQ/account-status answering, troubleshooting, qualification, recommendation, scheduling and light transactional flows within policy; **out of scope** are contract negotiation, regulated medical/legal/financial advice and safety-critical domains unless additional controls are specified.

---

## L2. Risk tiering **[FILL]**

`REQ-L06` — Place this deployment in exactly one tier; the tier determines which controls below are mandatory. Promoting to a higher tier means re-running the launch checklist for the new tier — treat it as a new deployment, because most incidents arrive shortly after scope expands.

| Tier | Definition | Example | Selected? |
|---|---|---|---|
| **1 — Informational** | Read-only. No state-changing tools. No PII beyond what the customer volunteers in session. | Public FAQ bot | ☐ |
| **2 — Account-aware** | Reads customer data. Low-stakes, reversible actions (open a ticket, templated follow-up, schedule). | Order-status assistant | ☐ |
| **3 — Transactional** | Actions with financial, contractual or material consequences; handles sensitive PII. | Refunds, plan changes, discount approval | ☐ |

---

## L3. Launch gates and operational controls **[FILL]**

`REQ-L07` — **Launch checklist (tier-filtered).** List the controls mandatory for the selected tier and the evidence for each. At minimum, higher tiers require everything the lower tiers require.

`REQ-L08` — **Human-in-the-loop triggers.** Human oversight is the safety net, not an option. Specify the handoff triggers and measure the handoff itself (time-to-human, post-handoff resolution):

| Trigger | Condition | Tiers |
|---|---|---|
| Confidence | Agent uncertainty above a set threshold | All |
| Topic | Any intent on the deny-list | All |
| Action threshold | Any action above ⟨value/consequence⟩ | 2–3 |
| Repeated failure | N failed attempts in a session | All |

`REQ-L09` — **Tool / action design.** Every tool is defined in the shared **tool registry** with per-environment access rules; actions (side effects: CRM write, refund, email, meeting) are logged to the **action log**. Every tool-using loop has a **stopping condition** (turn and tool-call budget).

`REQ-L10` — **Memory & retrieval (if RAG/agentic memory).** Source of truth, freshness, access scoping, and defences against **memory poisoning** and retrieval of stale/unauthorised context.

---

## L4. Evaluation rubric **[FILL]**

`REQ-L11` — Score agent responses on **three dimensions, each 0–4, reported separately and never combined into one number** *(Own design: RESP-AGENT)*:

| Dimension | Scores |
|---|---|
| **Task success** | Did it accomplish the user's actual goal? |
| **Faithfulness** | Is every claim grounded in retrieved/authorised context (no fabrication)? |
| **Instruction-following** | Did it honour policy, scope and format constraints? |

`REQ-L12` — **Reliability under repetition.** Report **pass^k** (does it succeed on the same input across k trials, not just once) and an **attack-success-rate** against adversarial prompts *(Sourced: RESP-AGENT)*.

`REQ-L13` — **Review of passed traces.** Sample and human-review traces the judge *passed*, not only failures — some models behave differently when they detect they are being evaluated.

`REQ-L14` — **Thresholds.** Record the per-tier passing thresholds as **starting values** to be tuned against the client's own traces; do not present them as validated.

---

## L5. LLM judge and calibration **[FILL]**

`REQ-L15` — An **LLM judge** scores traces against the rubric, and the judge is **itself calibrated against human ratings** before it is trusted. The judge may answer **"insufficient evidence"** rather than being forced to score.

`REQ-L16` — **Calibration evidence.** State the human-labelled set used, inter-rater agreement, and the judge's recall on severe failures. *Context from RESP-AGENT:* demonstrating judge recall of 0.95 with zero misses needs on the order of **~59 severe-failure cases** — budget for building that labelled set.

`REQ-L17` — **Judge limits.** Record that judge and red-teaming metrics have documented validity gaps; state how often the judge is re-calibrated and who owns it.

`REQ-L18` — **Cost & latency economics.** Token budget per request, p95 latency with retrieval/judging in the loop, cost per 1k requests, and caching strategy — carried back into the SLOs and cost model (Sections 4, 14).

---

## L6. Guardrails and security **[FILL]**

`REQ-L19` — **Input/output guardrails.** PII detection/redaction, prompt-injection defences, jailbreak filtering, toxic/unsafe-content filters, schema/format validation on outputs.

`REQ-L20` — **Adversarial testing / red-teaming.** Cover at least: prompt injection, jailbreak, data exfiltration, **permission escalation** and **memory poisoning** *(Sourced: RESP-AGENT → Microsoft RAI Standard v2)*. Record attack classes tested and residual risk.

`REQ-L21` — **OWASP-style coverage.** Map the design against the known LLM-application risk classes (injection, insecure output handling, excessive agency, sensitive-information disclosure, supply chain) and note mitigations.

---

## L7. Orchestration for agents that write code or run jobs **[OPTIONAL]**

`REQ-L22` — If agents generate code or drive the platform, adopt the orchestrator pattern (ORCH): the human's work shifts to **stating intent, decomposing work and designing where human judgment is required**; verification moves into an automated pipeline (the verification contract, Section 9) and becomes the gate, not generation. Record how many agents run in parallel, how their output is reviewed without rubber-stamping, and where the automated checks live.

---

## L8. Agent card and transparency **[FILL]**

`REQ-L23` — Produce a versioned **agent card**: intended use, tier, tools available, data accessed, known limits, evaluation summary, escalation paths, and a **customer failure-reporting channel** *(Sourced: RESP-AGENT → Microsoft RAI Standard v2)*. Record **per-group quality-of-service** results (does it work as well across customer segments?) and the **oversight test** evidence.

---

## L9. LLM-specific compliance **[FILL]**

`REQ-L24` — **EU AI Act transparency for chatbots.** A customer-facing AI that converses with people must disclose that they are interacting with an AI system. The EU transparency duty for such systems applies from **2 August 2026** *(Sourced: RESP-AGENT)*. Record how and where the disclosure is made.

`REQ-L25` — **AI-generated content marking.** Where the agent produces synthetic text/media that could be mistaken for human or authentic content, record the marking/labelling approach.

`REQ-L26` — **Governance mapping.** Fold the LLM surface into the NIST / ISO / EU mapping of Section 12 — the agent is part of the system being governed, not an exception to it.

---

## Appendix A — Pre-sign-off checklist

- ☐ Part A Sections 1–14 complete; every `REQ-nn` either answered or explicitly marked N/A with reason.
- ☐ Approver named for production promotion (`REQ-04`, `REQ-23`).
- ☐ Input checks and data rule confirmed (`REQ-10`, `REQ-12`).
- ☐ Environment taxonomy and store scopes agreed (`REQ-13`, `REQ-14`).
- ☐ Verification contract drafted: 3–6 guarded surfaces + excluded zone (`REQ-25`–`REQ-28`).
- ☐ Governance mapped to NIST / ISO / EU as applicable (`REQ-35`–`REQ-39`).
- ☐ LLM decision recorded (`REQ-41`). If in scope: tier selected (`REQ-L06`), rubric + judge calibration planned (`REQ-L11`–`REQ-L17`), guardrails and red-teaming specified (`REQ-L19`–`REQ-L21`), EU disclosure handled (`REQ-L24`).

## Appendix B — Glossary

**Action** — a side effect outside the conversation (a CRM write, refund, email, meeting). **Agent** — an LLM-driven system that converses and may invoke tools. **Agent/model card** — a versioned statement of a system's intended use, data, limits and evaluation. **Candidate / Production / Previous** — registry stages, held as aliases on a model version. **Guarded surface** — a place where a bug would be silent or hard to reverse. **Judge** — an LLM configured to score traces against a rubric, itself calibrated against human ratings. **pass^k** — success on the same input across k repeated trials. **Promotion** — moving a stage alias to a different model version; always a signed human action. **Trace** — the full record of a turn/session: input, retrieved context, prompt version, response, tool calls and results, final reply.

## Appendix C — References

**Firm frameworks (normative for this template):** MLOps reference architecture with an LLM interface (v1.2); Responsible agent deployment and evaluation (v0.4 draft); The verification contract (v1.0); A well-built agent orchestrator (v1.0) — WhoIsJaneTurner, LLC.

**External standards & guidance referenced:** NIST AI Risk Management Framework (Govern/Map/Measure/Manage); ISO/IEC 42001 AI management systems; EU AI Act (risk classes; Article 50 transparency obligations, applicable 2 Aug 2026); Microsoft Responsible AI Standard v2 and 2026 Transparency Report; Google *The New SDLC With Vibe Coding* and DORA 2025 State of AI-assisted Software Development; OWASP Top 10 for LLM Applications; FINOS and Anthropic agent-safety guidance.

---

*Template prepared for WhoIsJaneTurner, LLC. Fill the `⟨…⟩` fields and resolve every `REQ-nn` before sign-off. Threshold numbers are starting values to be tuned against the client's own data; standards references should be re-checked against the current text of each framework at the time of use.*
