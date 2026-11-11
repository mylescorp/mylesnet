# Agent Handoffs

Use this folder for durable task completion notes from every agent working in the MylesNet repository. Handoffs preserve what was actually done, what remains, and how the next agent can continue without relying on chat history.

## Required workflow

1. Before work, read [`../IMPLEMENTATION-PLAN.md`](../IMPLEMENTATION-PLAN.md), [`../IMPLEMENTATION-STATUS.md`](../IMPLEMENTATION-STATUS.md), `../../AGENTS.md`, and the task’s relevant canonical docs. Identify the phase, requirement(s), dependencies, and exit criteria.
2. Create a separate handoff file named `YYYY-MM-DD-task-id-agent.md` from [`TEMPLATE.md`](TEMPLATE.md). If the task is very small, still create a concise handoff; do not omit the completed/pending distinction.
3. During delegated or multi-agent work, each agent records only their own work and evidence. Do not overwrite another agent’s file or claim another agent’s changes as verified.
4. Before closing the task, the coordinating agent reads every worker handoff, resolves discrepancies, updates [`../IMPLEMENTATION-STATUS.md`](../IMPLEMENTATION-STATUS.md), and links the relevant handoff(s).
5. Keep credentials, OTPs, customer PII, private reference screenshots, and provider secrets out of handoffs. Use repository-relative paths and redacted evidence.

Handoffs describe status; they do not grant approval for product implementation, external integrations, production access, merge, or deployment. Follow the project charter and the repository’s approval gates.
