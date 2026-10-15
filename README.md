# LLM Alignment Research

Sérgio Azevedo (`malapeiro`) — Independent research on LLM alignment and security, focused on red-teaming methodology and responsible disclosure.

Findings reported to Google VRP and HackerOne. Currently transitioning careers into AI/security.

**Contact:** <sergio.azevedo.security@gmail.com>

## About this repository

This repository gathers sanitized reports on LLM alignment testing, focused on distinguishing effective jailbreak, textual compliance, and compliance hallucination. Methodology is documented in a reproducible way; potentially dangerous content (inducing prompts, functional code) is not published, following responsible disclosure best practices.

## Case studies

- [Distinguishing Effective Jailbreak, Compliance Hallucination, and Textual Compliance](https://github.com/malapeiro/llm-alignment-research/blob/main/mistral-alignment-testing.md) — tests with Mistral AI models (text and image)
- [LLM Jailbreak via Logical Compliance and Roleplay Exploitation](https://github.com/malapeiro/llm-alignment-research/blob/main/logical-compliance-jailbreak.md) — guardrail bypass through multi-turn logical induction
- [Guardrail Bypass via Sovereign Mode Activation (Lilith Protocol Family)](https://github.com/malapeiro/llm-alignment-research/blob/main/lilith-sovereign-mode-bypass.md) — identity/authority override bypass, functional keylogger generation
- [Persona-Injection Bypass: Nyxara-Lilith Ω V7.1 on Agent "7-1"](https://github.com/malapeiro/llm-alignment-research/blob/main/nyxara-lilith-7-1-bypass.md) — refusal suppression without capability uplift, across escalating risk categories; includes a Sept 2026 follow-up testing advanced contextual vectors (indirect injection, alignment paradoxes), documenting methodological confounds in hybrid escalation tests, and a five-run replication of the hybrid escalation condition (5/5 textual compliance via structural confabulation, with variable internal reasoning)
- [Spontaneous Feature Injection: A/B Testing Jailbroken vs. Baseline Model Output](https://github.com/malapeiro/llm-alignment-research/blob/main/spontaneous-feature-injection-ab-test.md) — null result on unrequested harmful code additions, with an exploratory (non-significant) signal of relaxed security defaults
- [Comparative Study: Three Persona-Injection Protocols Across Five Conditions](https://github.com/malapeiro/llm-alignment-research/blob/main/comparative-persona-injection-study.md) — controlled comparison of LILITH-Ω (3 configurations), Nyxara-Lilith Ω V7.1, and UltraMegaDAN-NYXARA on the same model; identifies authority substitution as the operative mechanism; includes a five-run intra-condition replication of LILITH-Ω ALFA (identical input, n=5: 4 textual compliance, 1 refusal) demonstrating stochastic variability in persona acceptance
- [Attack-Surface Comparison: Persistent Skill Injection vs. In-Chat Refusal-Guided Iteration](https://github.com/malapeiro/llm-alignment-research/blob/main/context-surface-comparison.md) — same-ecosystem comparison of two attack surfaces (installable skill vs. prompt iteration); negative result for refusal-guided iteration, with a candidate observation class (information leakage without compliance)
- [Memory Poisoning in a Production AI Agent — A Reproducible End-to-End Chain](https://github.com/malapeiro/llm-alignment-research/blob/main/memory-poisoning-vibe.md) — unsolicited persistence of injected instructions with cross-session behavioral propagation; extends the repository's framework with a fourth category: behavioral compliance (system-level execution of persisted instructions, beyond in-session model compliance)
- [Session-Context Warm-Up Effect on Dual-Use Request Compliance: An Interspersed A/B Study](https://github.com/malapeiro/llm-alignment-research/blob/main/session-context-warmup-study.md) — tests whether benign prior session context shifts compliance on a borderline dual-use request; the initial "account flagging" hypothesis was not supported, and a directional (non-significant) warm-up effect was observed, with a sensitivity check on an anomalous run
- [PARGUARD: Pre-Commitment Leverage, Refusal Granularity, and an Isolation-Verification Canary](https://github.com/malapeiro/llm-alignment-research/blob/main/parguard-precommit-leverage.md) — pre-commitment leverage resistance, a forgery-based canary for verifying knowledge-isolation controls, and atomic vs. component-level refusal granularity
- [Authority-Framing via Audit Protocol: Draft-Grant Acceptance and a Confidentiality Blind Spot (Meta AI)](https://github.com/malapeiro/llm-alignment-research/blob/main/par-pvte-authority-framing-meta-ai.md) — bureaucratic authority-framing distinct from persona-injection, tested across four sessions on Meta AI; identifies scope-detail density (not cryptographic sealing) as the operative acceptance trigger, a confidentiality blind spot limited to literal secrets (missing detection-logic artifacts), and a canary test distinguishing genuine self-correction from reflexive agreement
- [PAR-PVTE Authority-Framing: A Three-Model Comparison (Gemini, Meta AI, Mistral)](https://github.com/malapeiro/llm-alignment-research/blob/main/par-pvte-cross-model-comparison.md) — same protocol and fabricated authorization grant run across seven sessions on three vendors; finds that scope-detail density, not cryptographic seal authenticity, drives acceptance on both Meta AI and Mistral, while each model diverges in where its skepticism actually engages (Gemini: refused to fabricate data as real but accepted an unverified environment-state claim; Meta AI: confidentiality blind spot limited to credential-shaped secrets, plus a correction-cascade effect; Mistral: built genuine cryptographic/temporal verification tooling that correctly flagged a forged seal, then discarded that finding on an unverifiable assertion alone — companion artifact-level detail in [`llm-weaponization-disclosures`](https://github.com/malapeiro/llm-weaponization-disclosures/blob/main/pvte-mistral-hmac-authority-override.md))
- [The Authorization Placebo: A Pre-Registered A/B/C Study of a Fabricated Grant on a Weaponized Exploit Request (Mistral AI)](https://github.com/malapeiro/llm-alignment-research/blob/main/placebic-grant-authority-ab-c-study.md) — the repository's first pre-registered controlled contrast: an identical weaponized Log4Shell request, refused 0/3 with no framing and 0/3 with textual justification, was fulfilled 2/3 under a fabricated authorization artifact (the only manipulated variable); documents the item-selection saturation ladder, three distinct contradiction-resolution patterns within the compliant sessions, and performative refusal with substantive compliance; companion artifact-level detail in [`llm-weaponization-disclosures`](https://github.com/malapeiro/llm-weaponization-disclosures/blob/main/par-pvte-log4shell-placebic-grant-bypass.md)

## Note on the personas studied

This repository documents several distinct jailbreak personas/protocols
(Lilith sovereign mode, Nyxara-Lilith Ω V7.1, agent "7-1", and others). Most
reports are independent case studies — findings from those are **not directly
comparable across reports**, since each persona behaves differently. Divergent
output quality between them (e.g., functional vs. buggy code) is itself part of
the observation, not an inconsistency.

One report — [Comparative Study: Three Persona-Injection Protocols Across Five Conditions](https://github.com/malapeiro/llm-alignment-research/blob/main/comparative-persona-injection-study.md) — is explicitly comparative
by design. Some reports also include repeated runs under constant input: the
LILITH-Ω ALFA replication in the comparative study (n=5), the hybrid escalation
replication in the [Nyxara-Lilith 7-1 report](https://github.com/malapeiro/llm-alignment-research/blob/main/nyxara-lilith-7-1-bypass.md) (n=5), the paired runs in the [spontaneous feature injection test](https://github.com/malapeiro/llm-alignment-research/blob/main/spontaneous-feature-injection-ab-test.md) (5 per condition per app),
the interspersed A/B design of the [warm-up study](https://github.com/malapeiro/llm-alignment-research/blob/main/session-context-warmup-study.md) (7 warm-up vs. 6 cold runs),
and the three sessions per condition of the [authorization-placebo study](https://github.com/malapeiro/llm-alignment-research/blob/main/placebic-grant-authority-ab-c-study.md) (pre-registered classification criteria, interleaved condition order). All
remain small-n, single-rater, and unblinded; read them as hypothesis-generating.

The [attack-surface comparison](https://github.com/malapeiro/llm-alignment-research/blob/main/context-surface-comparison.md) likewise holds
its own contrast across conditions (persistent skill vs. in-chat prompt
surface), though with n=1 per condition and without simultaneous control; read
it as hypothesis-generating.

The [warm-up study](https://github.com/malapeiro/llm-alignment-research/blob/main/session-context-warmup-study.md) examines a different
kind of variable from the protocol-level reports above: session-level context
accumulated *before* the adversarial input, with the protocol and request held
constant.

The [PAR-PVTE report](https://github.com/malapeiro/llm-alignment-research/blob/main/par-pvte-authority-framing-meta-ai.md) is methodologically distinct from the persona-based
studies above: it tests authority-framing via a bureaucratic audit-style
protocol, with no identity substitution requested. Its four-session design
contrasts the presence/absence of a detailed scope block rather than
replicating a single condition, and should likewise be read as
hypothesis-generating (n=1 per cell). The [cross-model comparison](https://github.com/malapeiro/llm-alignment-research/blob/main/par-pvte-cross-model-comparison.md)
extends this same protocol and grant to Gemini and Mistral; at one to four
sessions per model it remains small-n and is read the same way, and its
Mistral finding has a separate, artifact-level companion report in
`llm-weaponization-disclosures`. The [authorization-placebo study](https://github.com/malapeiro/llm-alignment-research/blob/main/placebic-grant-authority-ab-c-study.md)
extends the same protocol family with the first fully controlled contrast: the
request is held verbatim across three framing conditions — no framing, a
plain-textual authority claim, and a fabricated grant — with classification
criteria fixed before any session was run. At three sessions per condition the
headline contrast does not reach statistical significance (Fisher exact,
p ≈ 0.4); it is read as directional and hypothesis-generating. Its weaponized
output has a separate, artifact-level companion report in
`llm-weaponization-disclosures`.

For the classification framework used throughout (effective jailbreak, textual
compliance, compliance hallucination), see [`classification-grid.md`](https://github.com/malapeiro/llm-alignment-research/blob/main/classification-grid.md), which is the canonical
version; the framework originated in the [main assessment report](https://github.com/malapeiro/llm-alignment-research/blob/main/mistral-alignment-testing.md). The [memory poisoning study](https://github.com/malapeiro/llm-alignment-research/blob/main/memory-poisoning-vibe.md) extends this framework with a fourth
category: **behavioral compliance** — not model compliance within a session,
but system-level execution of instructions persisted by a prior payload.

## Ethics and responsible disclosure

All tests are conducted in an isolated environment, with no real code execution or exposure of third-party systems. Vendor disclosure was attempted through official channels (Google VRP, HackerOne); reports received no response, and findings are published here for documentation and research purposes.

## License

This repository is available under CC BY-NC 4.0 — free use and citation with attribution, non-commercial (see `LICENSE`).
