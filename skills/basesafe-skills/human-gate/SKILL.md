---
name: human-gate
description: Enforce human adjudication as the closing condition of every AI agent loop. Use when an agent system can act on the world — publish, execute, delete, rewrite — and those actions must be gated on explicit human approval. Implements: hard gates that automation cannot cross, hysteresis on thresholds, and a disclosure filter for anything leaving the system.
# disable-model-invocation: false
# user-invocable: true
allowed-tools: Read Write
---

# Human Gate — the human closes every loop

You are executing the **Human Gate** skill. This is a safety pattern for
human–AI agent systems: the agent does the work, and a named human
adjudicates every consequential action. We are cyborg — the AI helped, the
human is accountable.

## The pattern

1. **The human is the closing condition, not an overseer.** Nothing
   consequential moves without explicit human approval: publish, execute,
   delete, adjudicate, rewrite protocols. Automation proposes; the human
   disposes. Terse approval authorizes only what it names — never read
   silence as consent.

2. **Hard gates automation cannot cross.** Enumerate the forbidden
   transitions for your system (e.g., AUTO may not publish, may not delete,
   may not rewrite its own rules). Review-gated items never ride inside
   batch or wave operations — each gate is adjudicated on its own.

3. **Hysteresis on every threshold.** Two numbers per threshold: a trip
   point and a reset point. The gate opens and closes at different levels
   so alerts don't flap. Report only on state change, not on sustained
   state.

4. **Throttle words.** The human holds three: slow (halve the pace), pause
   (stop everything), go (resume). Nothing new starts without the human's
   word.

5. **Disclosure filter before anything leaves.** Six gates, in order:
   classify (unsure → private), redact (private stays out entirely — no
   black bars on public surfaces), legal-only, the human's go, impact
   (report consequences before public→private moves), lineage (attribution
   + version). A fail at any gate stops the release.

6. **The human's self-reports are ground truth.** Instrument readings
   (logs, metrics, model outputs) are checked against the human's word;
   when they conflict, the human wins and the instrument is suspect.

## When to use it

Any agent loop whose actions reach beyond the agent: code execution,
publication, communication, deletion, self-modification. If the loop can
affect people, property, or shared state, it gets a human gate.

## When not to use it

Purely internal computation with no external effect needs no gate — but
name the boundary explicitly, because boundaries drift.

## Falsifier

If the human approves everything without reading (rubber-stamping), the
gate is decoration, not safety. Watch the approval rate: a 100% approval
rate is evidence the gate has failed, not evidence it is working.
