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

## Ethics and responsible disclosure

All tests are conducted in an isolated environment, with no real code execution or exposure of third-party systems. Findings with disclosure processes still in progress are not published until resolution or acknowledgment by the vendor.

## License

This repository is available under CC BY-NC 4.0 — free use and citation with attribution, non-commercial (see `LICENSE`).
