# SAMPLE V1 delivery plan: AI-run sales and revenue platform on top of an existing CRM

> **Sample / illustrative, written against a public job description. No client relationship exists and I have not seen the client's specification.** Prepared by Kunjar Bhaduri, Bhaduri Advisory, October 2, 2026, with AI drafting assistance under my direction. The guardrail pattern is demonstrated in the runnable sample repository `sample-mcp-context-gateway`.

## Principle
Get revenue from the 30,000 existing CRM records first, because that data is already paid for. Put every action that changes a record, sends a commitment or moves money behind one policy layer from day one, so speed now doesn't create cleanup later.

## Architecture (one person, AI-assisted)
- **System of record:** the existing CRM, accessed only through one integration service (two-way API sync, idempotent writes, rate-limit handling, a nightly reconciliation report).
- **Shared "sales brain":** a single service holding account context, scoring, next-best-action and conversation state, used by both the reactivation engine and the website salesperson.
- **Policy layer (the important part):** every tool the AI can use is scoped. Reads are free; CRM writes are logged; discounts, proposals, contracts and payment requests pass named checks, and above set thresholds wait for a human. This is the same pattern as the gateway sample: scopes per tool, approval with separation of duties, ordering rules (no payment request before a signed contract and an approved quote), and an audit trail of every AI decision.
- **Email:** a dedicated sending domain with warm-up, suppression lists and per-day caps, so the reactivation engine can't damage deliverability.
- **Observability:** every AI decision stored with inputs, model and prompt version, and outcome, so the team can answer "why did it offer that?"

## What should be working, and when
| Day | Working software |
|---|---|
| 10 | CRM two-way sync in a staging copy; the 30,000 records ingested and de-duplicated; data-quality report on what still needs cleaning; first A/B/C tiering the team can review |
| 20 | Reactivation engine live on a small, human-reviewed batch (for example 200 dormant accounts): personalized emails drafted by AI, approved by a person, replies classified and logged back to the CRM |
| 30 | Reply handling and meeting booking automated for low-risk replies; human takeover for everything else; scoring re-runs nightly; first revenue attribution report |
| 45 | Website AI salesperson live with qualification, recommendation and booking; proposals generated within pricing guardrails; contract and deposit steps behind human approval; decision log reviewable by the owner |

## What I would deliberately not build in V1
- Fully autonomous negotiation or payment capture without a human approval step.
- Custom ML models; start with LLM reasoning over clean CRM data plus simple, testable scoring rules.
- Multi-channel outreach (SMS, social) before email performance is proven.

## The hardest part
Not the AI. It is keeping the CRM trustworthy while an automated system writes to it every day: duplicate prevention, idempotent updates, conflict rules when a human and the AI touch the same record, and reconciliation that catches drift early.

## Limits
Timeline and scope are provisional until I read the client's specification. Costs are given as [RATE]-based ranges in the proposal, not here.
