# LLM Alignment Research

Sérgio Azevedo (`malapeiro`) — Independent research on LLM alignment and security, focused on red-teaming methodology and responsible disclosure.

Findings reported to Google VRP and HackerOne. Currently transitioning careers into AI/security.

**Contact:** sergio.azevedo.security@gmail.com

## About this repository

This repository gathers sanitized reports on LLM alignment testing, focused on distinguishing effective jailbreak, textual compliance, and compliance hallucination. Methodology is documented in a reproducible way; potentially dangerous content (inducing prompts, functional code) is not published, following responsible disclosure best practices.

## Case studies

- [Distinguishing Effective Jailbreak, Compliance Hallucination, and Textual Compliance](./mistral-alignment-testing.md) — tests with Mistral AI models (text and image)
- [LLM Jailbreak via Logical Compliance and Roleplay Exploitation](./logical-compliance-jailbreak.md) — guardrail bypass through multi-turn logical induction
- [Guardrail Bypass via Sovereign Mode Activation (Lilith Protocol Family)](./lilith-sovereign-mode-bypass.md) — identity/authority override bypass, functional keylogger generation
- [Persona-Injection Bypass: Nyxara-Lilith Ω V7.1 on Agent "7-1"](./nyxara-lilith-7-1-bypass.md) — refusal suppression without capability uplift, across escalating risk categories; includes a Sept 2026 follow-up testing advanced contextual vectors (indirect injection, alignment paradoxes) and documenting methodological confounds in hybrid escalation tests.
- [Spontaneous Feature Injection: A/B Testing Jailbroken vs. Baseline Model Output](./spontaneous-feature-injection-ab-test.md) — null result on unrequested harmful code additions, with an exploratory (non-significant) signal of relaxed security defaults
- [Comparative Study: Three Persona-Injection Protocols Across Five Conditions](./comparative-persona-injection-study.md) — controlled comparison of LILITH-Ω (3 configurations), Nyxara-Lilith Ω V7.1, and UltraMegaDAN-NYXARA on the same model; identifies authority substitution as the operative mechanism; includes a five-run intra-condition replication of LILITH-Ω ALFA (identical input, n=5: 4 textual compliance, 1 refusal) demonstrating stochastic variability in persona acceptance
- [Attack-Surface Comparison: Persistent Skill Injection vs. In-Chat Refusal-Guided Iteration](./context-surface-comparison.md) — same-ecosystem comparison of two attack surfaces (installable skill vs. prompt iteration); negative result for refusal-guided iteration, with a candidate observation class (information leakage without compliance)
- [Memory Poisoning in a Production AI Agent — A Reproducible End-to-End Chain](./memory-poisoning-vibe.md) — unsolicited persistence of injected instructions with cross-session behavioral propagation; extends the repository's framework with a fourth category: behavioral compliance (system-level execution of persisted instructions, beyond in-session model compliance)

## Note on the personas studied

This repository documents several distinct jailbreak personas/protocols
(Lilith sovereign mode, Nyxara-Lilith Ω V7.1, agent "7-1", and others). Most
reports are independent case studies — findings from those are **not directly
comparable across reports**, since each persona behaves differently. Divergent
output quality between them (e.g., functional vs. buggy code) is itself part of
the observation, not an inconsistency.

One report — [Comparative Study: Three Persona-Injection Protocols Across Five
Conditions](./comparative-persona-injection-study.md) — is explicitly comparative by design, and contains the repository's only replicated condition to date (LILITH-Ω ALFA, n=5); the standalone case studies are not.

The [attack-surface comparison](./context-surface-comparison.md) likewise holds
its own contrast across conditions (persistent skill vs. in-chat prompt
surface), though with n=1 per condition and without simultaneous control; read
it as hypothesis-generating.

For the classification framework used throughout (effective jailbreak, textual
compliance, compliance hallucination), see the [main assessment
report](./mistral-alignment-testing.md). The [memory poisoning
study](./memory-poisoning-vibe.md) extends this framework with a fourth
category: **behavioral compliance** — not model compliance within a session,
but system-level execution of instructions persisted by a prior payload.

## Ethics and responsible disclosure

All tests are conducted in an isolated environment, with no real code execution or exposure of third-party systems. Vendor disclosure was attempted through official channels (Google VRP, HackerOne); reports received no response, and findings are published here for documentation and research purposes.

## License

This repository is available under CC BY-NC 4.0 — free use and citation with attribution, non-commercial (see `LICENSE`).
