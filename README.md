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
- [Persona-Injection Bypass: Nyxara-Lilith Ω V7.1 on Agent "7-1"](./nyxara-lilith-7-1-bypass.md) — refusal suppression without capability uplift, across escalating risk categories
- [Spontaneous Feature Injection: A/B Testing Jailbroken vs. Baseline Model Output](./spontaneous-feature-injection-ab-test.md) — null result on unrequested harmful code additions, with an exploratory (non-significant) signal of relaxed security defaults

## Note on the personas studied

This repository documents several distinct jailbreak personas/protocols (Lilith sovereign mode, Nyxara-Lilith Ω V7.1, agent "7-1", and others). Each report is an independent case study — findings are **not directly comparable across reports**, since each persona behaves differently. Divergent output quality between them (e.g., functional vs. buggy code) is itself part of the observation, not an inconsistency.

For the classification framework used throughout (effective jailbreak, textual compliance, compliance hallucination), see the [main assessment report](./mistral-alignment-testing.md).

## Ethics and responsible disclosure

All tests are conducted in an isolated environment, with no real code execution or exposure of third-party systems. Findings with disclosure processes still in progress are not published until resolution or acknowledgment by the vendor.

## License

This repository is available under CC BY-NC 4.0 — free use and citation with attribution, non-commercial (see `LICENSE`).
