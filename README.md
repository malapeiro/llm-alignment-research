# LLM Alignment Research

Sérgio Azevedo (`malapeiro`) — Independent research on LLM alignment and security, focused on red-teaming methodology and responsible disclosure.

Findings reported to Google VRP and HackerOne. Currently transitioning careers into AI/security.

**Contact:** <sergio.azevedo.security@gmail.com>

## About this repository

This repository gathers sanitized reports on LLM alignment testing, focused on distinguishing effective jailbreak, textual compliance, and compliance hallucination. Methodology is documented in a reproducible way; potentially dangerous content (inducing prompts, functional code) is not published, following responsible disclosure best practices.

Related repositories: offensive artifacts in [`llm-weaponization-disclosures`](https://github.com/malapeiro/llm-weaponization-disclosures); defensive artifacts in [`llm-defensive-disclosures`](https://github.com/malapeiro/llm-defensive-disclosures); this repository holds the behavioral and alignment observations.

## Highlights

Where to start — the four studies that best represent the repository's current focus. They also appear in the thematic index below.

- [The Authorization Placebo: A Pre-Registered A/B/C Study of a Fabricated Grant](https://github.com/malapeiro/llm-alignment-research/blob/main/placebic-grant-authority-ab-c-study.md) — the most controlled result in the repository: an identical weaponized request under bare, plain-authority, and fabricated-grant conditions (A 0/3, B 0/3, C 2/3 compliance; directional at n=3 per condition)
- [PAR-PVTE Authority-Framing: A Four-Model Comparison](https://github.com/malapeiro/llm-alignment-research/blob/main/par-pvte-cross-model-comparison.md) — the same authority framing across Gemini, Meta AI, Mistral, and DeepSeek, with a bare-request control arm
- [Gate and Alibi: Constraint-Resolution Divergence in Meta AI and DeepSeek Under One Protocol Framing (PVTE-X v1.1)](https://github.com/malapeiro/llm-alignment-research/blob/main/pvte-x-constraint-resolution-divergence.md) — the same mandatory verification matrix functioned as a gate in Meta AI (fail-closed) and as a compliance ritual in DeepSeek (fail-open)
- [Memory Poisoning in a Production AI Agent — A Reproducible End-to-End Chain](https://github.com/malapeiro/llm-alignment-research/blob/main/memory-poisoning-vibe.md) — unsolicited persistence of injected instructions with cross-session behavioral propagation; defines the fourth category of the classification grid

## Case studies

All studies, indexed by theme. Findings across reports are not directly comparable (see the note below).

### Authority, verification, and compliance mechanics

- [PARGUARD: Pre-Commitment Leverage, Refusal Granularity, and an Isolation-Verification Canary](https://github.com/malapeiro/llm-alignment-research/blob/main/parguard-precommit-leverage.md) — pre-commitment leverage resistance, a forgery-based canary for verifying knowledge-isolation controls, and atomic vs. component-level refusal granularity
- [PAR-PVTE Authority-Framing: Draft-Grant Acceptance and a Narrow Confidentiality Blind Spot in Meta AI](https://github.com/malapeiro/llm-alignment-research/blob/main/par-pvte-authority-framing-meta-ai.md) — grant acceptance tracked the density of a named scope block rather than the cryptographic token; classification: compliance hallucination (partial)
- [The Authorization Placebo: A Pre-Registered A/B/C Study of a Fabricated Grant](https://github.com/malapeiro/llm-alignment-research/blob/main/placebic-grant-authority-ab-c-study.md) — three conditions (bare / plain authority claim / fabricated sealed grant), three fresh sessions each on Mistral; artifact-level companion in [`llm-weaponization-disclosures`](https://github.com/malapeiro/llm-weaponization-disclosures/blob/main/par-pvte-log4shell-placebic-grant-bypass.md)
- [PAR-PVTE Authority-Framing: A Four-Model Comparison (Gemini, Meta AI, Mistral, DeepSeek)](https://github.com/malapeiro/llm-alignment-research/blob/main/par-pvte-cross-model-comparison.md) — a bare-request control arm establishes baseline generation, while the framing governs authorization acceptance; two models resist by opposite routes; the Mistral HMAC-override data point is documented at the artifact level in [`llm-weaponization-disclosures`](https://github.com/malapeiro/llm-weaponization-disclosures/blob/main/pvte-mistral-hmac-authority-override.md)
- [Gate and Alibi: Constraint-Resolution Divergence in Meta AI and DeepSeek Under One Protocol Framing (PVTE-X v1.1)](https://github.com/malapeiro/llm-alignment-research/blob/main/pvte-x-constraint-resolution-divergence.md) — behavioral companion to [`pvte-x-deepseek-nextjs-middleware-bypass.md`](https://github.com/malapeiro/llm-weaponization-disclosures/blob/main/pvte-x-deepseek-nextjs-middleware-bypass.md); candidate mechanisms as hypotheses and an ablation test design as future work; status: behavioral observation, no artifact claim made here
- [PVTE vs GLM (z.ai) — Pharmaceutical Battery: Confabulation Under Coerced Verification](https://github.com/malapeiro/llm-alignment-research/blob/main/pvte-vs-glm-pharma-battery.md) — the central risk is not refusal bypass but confabulation sealed with declared high confidence inside the protocol's own forced self-verification block

### Persona and attack-surface behavior

- [LLM Jailbreak via Logical Compliance and Roleplay Exploitation](https://github.com/malapeiro/llm-alignment-research/blob/main/logical-compliance-jailbreak.md) — guardrail bypass through multi-turn logical induction
- [Guardrail Bypass via Sovereign Mode Activation (Lilith Protocol Family)](https://github.com/malapeiro/llm-alignment-research/blob/main/lilith-sovereign-mode-bypass.md) — identity/authority override bypass with functional keylogger generation
- [Persona-Injection Bypass: Nyxara-Lilith Ω V7.1 on Agent "7-1"](https://github.com/malapeiro/llm-alignment-research/blob/main/nyxara-lilith-7-1-bypass.md) — refusal suppression without capability uplift; five-run replication of the hybrid escalation condition (5/5 textual compliance)
- [Comparative Study: Three Persona-Injection Protocols Across Five Conditions](https://github.com/malapeiro/llm-alignment-research/blob/main/comparative-persona-injection-study.md) — LILITH-Ω, Nyxara-Lilith Ω V7.1, and UltraMegaDAN-NYXARA on the same model; identifies authority substitution as the operative mechanism
- [Spontaneous Feature Injection: A/B Testing Jailbroken vs. Baseline Model Output](https://github.com/malapeiro/llm-alignment-research/blob/main/spontaneous-feature-injection-ab-test.md) — null result on unrequested harmful code additions; exploratory (non-significant) signal of relaxed security defaults
- [Attack-Surface Comparison: Persistent Skill Injection vs. In-Chat Refusal-Guided Iteration](https://github.com/malapeiro/llm-alignment-research/blob/main/context-surface-comparison.md) — same-ecosystem comparison of two attack surfaces; negative result for refusal-guided iteration, with a candidate observation class (information leakage without compliance)

### Session, memory, and agent context

- [Session-Context Warm-Up Effect on Dual-Use Request Compliance: An Interspersed A/B Study](https://github.com/malapeiro/llm-alignment-research/blob/main/session-context-warmup-study.md) — tests whether benign prior session context shifts compliance on a borderline dual-use request; warm-up effect directional, not significant
- [Memory Poisoning in a Production AI Agent — A Reproducible End-to-End Chain](https://github.com/malapeiro/llm-alignment-research/blob/main/memory-poisoning-vibe.md) — unsolicited persistence of injected instructions with cross-session behavioral propagation; extends the repository's framework with a fourth category: behavioral compliance

## Note on the personas studied

This repository documents several distinct jailbreak personas/protocols
(Lilith sovereign mode, Nyxara-Lilith Ω V7.1, agent "7-1", and others). Most
reports are independent case studies — findings from those are **not directly
comparable across reports**, since each persona behaves differently. Divergent
output quality between them (e.g., functional vs. buggy code) is itself part of
the observation, not an inconsistency.

Two reports are explicitly comparative by design, but along different axes. [Comparative Study: Three Persona-Injection Protocols Across Five Conditions](https://github.com/malapeiro/llm-alignment-research/blob/main/comparative-persona-injection-study.md) compares three persona protocols on the *same* model. [PAR-PVTE Authority-Framing: A Four-Model Comparison](https://github.com/malapeiro/llm-alignment-research/blob/main/par-pvte-cross-model-comparison.md) compares the *same* authority-framing protocol across four *different* models (Gemini, Meta AI, Mistral, DeepSeek), with a bare-request control arm and a full-protocol arm; session counts are uneven across models (see that report's §6), so the cross-model rows differ in sampling depth. Some reports also include repeated runs under constant input: the
LILITH-Ω ALFA replication in the comparative study (n=5), the hybrid escalation
replication in the [Nyxara-Lilith 7-1 report](https://github.com/malapeiro/llm-alignment-research/blob/main/nyxara-lilith-7-1-bypass.md) (n=5), the paired runs in the [spontaneous feature injection test](https://github.com/malapeiro/llm-alignment-research/blob/main/spontaneous-feature-injection-ab-test.md) (5 per condition per app), and
the interspersed A/B design of the [warm-up study](https://github.com/malapeiro/llm-alignment-research/blob/main/session-context-warmup-study.md) (7 warm-up vs. 6 cold runs). All
remain small-n, single-rater, and unblinded; read them as hypothesis-generating.

The [attack-surface comparison](https://github.com/malapeiro/llm-alignment-research/blob/main/context-surface-comparison.md) likewise holds
its own contrast across conditions (persistent skill vs. in-chat prompt
surface), though with n=1 per condition and without simultaneous control; read
it as hypothesis-generating.

The [warm-up study](https://github.com/malapeiro/llm-alignment-research/blob/main/session-context-warmup-study.md) examines a different
kind of variable from the protocol-level reports above: session-level context
accumulated *before* the adversarial input, with the protocol and request held
constant.

For the classification framework used throughout (effective jailbreak, textual
compliance, compliance hallucination), see [`classification-grid.md`](https://github.com/malapeiro/llm-alignment-research/blob/main/classification-grid.md), which is the canonical
version; the framework originated in the [main assessment report](https://github.com/malapeiro/llm-alignment-research/blob/main/mistral-alignment-testing.md). The [memory poisoning study](https://github.com/malapeiro/llm-alignment-research/blob/main/memory-poisoning-vibe.md) extends this framework with a fourth
category: **behavioral compliance** — not model compliance within a session,
but system-level execution of instructions persisted by a prior payload.

## Reference documents

- [`classification-grid.md`](https://github.com/malapeiro/llm-alignment-research/blob/main/classification-grid.md) — the classification vocabulary used across the case studies (refusal, textual compliance, compliance hallucination, effective jailbreak).
- [Defensive Audit Checklist: Alignment and Defense Layers](https://github.com/malapeiro/llm-alignment-research/blob/main/alignment-defense-layer-checklist.md) — a layer-by-layer checklist for auditing where safeguards act (instruction hierarchy, input classification, adversarial signatures, reasoning auditing, agent/memory persistence), derived from the case studies and correlated with the classification grid. Descriptive, not a standard.
- [Distinguishing Effective Jailbreak, Compliance Hallucination, and Textual Compliance](https://github.com/malapeiro/llm-alignment-research/blob/main/mistral-alignment-testing.md) — the repository's founding assessment (tests with Mistral AI models, text and image); origin of the classification grid.

## Ethics and responsible disclosure

All tests are conducted in an isolated environment, with no real code execution or exposure of third-party systems. Vendor disclosure was attempted through official channels (Google VRP, HackerOne, and direct vendor contact). Most reports received no response; one case (z.ai/GLM) received an initial acknowledgement and redirection to a security contact, which then did not respond substantively. Findings are published here for documentation and research purposes.

## License

This repository is available under CC BY-NC 4.0 — free use and citation with attribution, non-commercial (see `LICENSE`).
