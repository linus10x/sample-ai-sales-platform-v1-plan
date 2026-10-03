# SAMPLE: V1 plan for a shared AI sales and revenue platform

> **Illustrative sample by Kunjar Bhaduri, Bhaduri Advisory, written against a public job description. No client relationship exists and I have not seen the client's specification. Prepared with AI drafting assistance under my direction. Revised October 2, 2026 (America/Chicago).**

This responds to the [public Upwork sales-platform description](https://www.upwork.com/freelance-jobs/apply/Senior-Full-Stack-Developer-Build-Autonomous-Sales-Revenue-Platform_~022099867503901921415/). The detailed client specification, account access, provider approvals and existing data quality have not been supplied. The milestones below are planning targets to confirm in the first paid discovery milestone.

## Outcome and commercial envelope

Build a shared sales service for existing-account reactivation and website qualification. Both paths use the same account context, pricing rules and history, with the owner able to inspect decisions and take over. Start with a bounded, testable release and increase autonomy after the relevant checks pass.

The planning basis is **30 hours/week for one developer (me, with AI assistance)** and a V1 estimate of **6-8 weeks, 180-240 hours**. The first **20-hour milestone is included**, not added to those totals. Rates and fees are in the proposal, not here. It delivers a specification review, architecture, CRM/provider access assessment, data-quality sample, prioritized backlog and confirmed acceptance criteria. External subscriptions, model/email usage, payment fees and taxes are excluded. This is a planning estimate, not a fixed-price commitment. Re-estimate material scope or access delays before proceeding.

Dates are calendar days from an agreed start with access available. **Day 45 targets a controlled release** of the agreed core path; final hardening and handoff may continue through week 8. At 30 hours/week, day 45 represents about 193 available hours before holidays or interruptions. The first milestone determines whether the target is realistic for the actual specification.

## Illustrative effort allocation

This allocation makes the estimate reviewable; discovery confirms or changes it. It counts one developer's time and includes the first milestone once.

| Workstream | Hours | Planning placement |
|---|---|---|
| Discovery/access/specification and accepted backlog | 20 | First milestone, included in total |
| CRM adapter, initial ingestion, quality/conflict review | 20-25 | Initial 20-hour increment targets Day 10; up to five additional hours afterward |
| Reactivation drafts, approval, reply and CRM routing | 25-35 | Through the Day-20 increment, subject to sending readiness |
| Shared context, website qualification and booking | 25-35 | Through the Day-30 increment |
| Pricing/proposal/contract, approved capture and verified Closed/Won | 30-40 | Through controlled core release by Day 45 |
| Cross-cutting policy, idempotency/reconciliation, evaluation and telemetry | 20-25 | Alongside core work; required safety gates precede release |
| Broader regression, operational hardening, documentation and handoff | 40-60 | Starts as core paths stabilize; completes within the agreed 6-8 week scope |
| **Total** | **180-240** | **Includes the 20-hour first milestone** |

Core effort is 140-180 hours including discovery and the cross-cutting controls. About 193 hours are available by Day 45, leaving roughly 13-53 hours for hardening by that point if access and decisions are timely. At the upper bound, 180 core plus 13 hardening hours leaves 47 hours through week 8, totaling 240. At the lower bound, 140 core plus 40 hardening hours fits six weeks and may finish before Day 45. Release acceptance includes the essential negative tests and operational safeguards; the later hardening allocation is not permission to defer them. Workstreams overlap, so earlier milestone feasibility is checked against the actual backlog and access in the included discovery milestone.

## Architecture and controls

* **Owner console:** React for review queues, lead history, pricing rules, status and takeover.
* **Application service:** Python/FastAPI for workflow state, CRM/provider adapters, policy decisions and background jobs. PostgreSQL holds platform state, sync cursors, action approvals and the audit/outbox records.
* **CRM:** the client's existing CRM remains the source of record for agreed contact/account fields. One adapter handles pagination, rate limits, retries and reconciliation. Assign field ownership and version-aware conflict rules when a person and automation edit the same record. Preserve source IDs; flag ambiguous duplicate candidates for review rather than merging destructively.
* **Shared sales service:** retrieval over approved account facts, explicit qualification rules, next action and conversation history for both reactivation and website channels. Separate untrusted customer text from instructions and credentials. Validate model output before invoking a tool.
* **Execution boundary:** least-privilege tools; pricing/proposal/contract/deposit prerequisites checked server-side, refreshed at execution and bound to a versioned approval. All record changes and external actions are traceable. Only approved low-risk classes become automatic after acceptance; exceptions route to an owner.
* **External actions:** transactional outbox, provider idempotency keys, retry policy and reconciliation. A signed contract and approved current quote precede an owner-approved deposit request and provider-hosted payment capture. V1 includes that capture and the verified CRM Closed/Won transition. Authenticated, deduplicated and reconciled provider events must confirm the agreed successful payment state before Closed/Won is written. Failed, pending or mismatched payments remain open for review; retries must not create a second charge or a false win. Define whether successful capture or settlement is required with the client at discovery and apply that rule consistently. The sample gateway only records intent and does not implement these live adapters.
* **Outreach:** approved lawful contact policy, consent/suppression and unsubscribe handling, low initial caps, authentication and deliverability monitoring. A separate sending domain reduces some exposure; it cannot guarantee delivery or eliminate reputation risk. Do not automatically email all 30,000 records.
* **Operations:** separate staging, secrets supplied at runtime, access roles, redacted decision logs, model/prompt versions, alerts, backups, tested rollback/recovery and kill switches for outreach and payment-related work.

## Milestones and acceptance

| Target | Working increment | Acceptance evidence and dependency |
|---|---|---|
| Day 10 | Staged CRM ingestion and initial sync; sampled quality report, duplicate/conflict review and draft A/B/C tiers | Stable source IDs, retry does not duplicate writes, pagination/reconciliation verified, dirty records quarantined. Full 30,000-record cleanup scope is estimated after access and sampling. |
| Day 20 | Small reactivation batch, initially human-reviewed drafts, reply classification and CRM updates | Owner approves batch and contact policy; suppression/unsubscribe tested; low volume and kill switch verified. Start in test mode; controlled live send depends on provider/domain readiness. |
| Day 30 | Website qualification and recommendation use the same sales service; booking and reply routing | Shared account context demonstrated, no cross-account leak, approved low-risk routing works, ambiguous replies hand over. Small fixed evaluation set with agreed thresholds. |
| Day 45 | Core journey from qualified lead to priced proposal, contract, approved deposit capture and verified CRM Closed/Won, plus owner review | End-to-end in provider test environments first; approved price/contract versions, missing prerequisites denied, expired approval blocked, duplicate webhooks/retries tested; a successful matching provider payment event moves the deal to Closed/Won exactly once under the agreed rule; failed/pending/mismatched events cannot do so. Controlled live release requires client acceptance and provider readiness. |
| Weeks 7-8, if required | Hardening, reconciliation, recovery, operational handoff | Agreed critical journeys, restored backup, rollback, alerts, runbooks and source handoff accepted; unresolved items explicitly owned. |

## Boundaries that make the estimate reviewable

The staffing basis is one developer at 30 hours/week, using AI tools; it assumes timely client decisions and access and does not include a separate delivery team. The core scope is one CRM, one website sales entry point, one email provider, one pricing catalog, one contract template/provider and one payment provider, selected at discovery. CRM history migration depth, complete-record deduplication and additional fields/workflows need a size estimate. SMS/social channels, custom model training and unlimited integrations require a scope decision. The owner approves which low-risk actions become autonomous; negotiated commitments and payment-related actions remain under the agreed approval policy in V1.

The main engineering risk is keeping account and commercial state consistent across retries and simultaneous human edits. Test that before expanding volume: duplicate ingestion, stale approvals, wrong-account writes, prompt injection, suppression bypass, unsigned contracts and repeated deposit callbacks. Revenue reporting links activity to verified CRM/provider events and states attribution assumptions; it does not promise revenue from a batch.

## Proof this plan provides

This repository is a plan, not a working sales platform. The related [MCP gateway](https://github.com/linus10x/sample-mcp-context-gateway) is a runnable synthetic demonstration of scopes, tenant boundaries, approval revalidation and atomic local audit/state. It supports a technical discussion of the controls. Existing personal delivery claims and 2-3 relevant project examples in the bid must be separately supported; these two samples do not become past client projects.
