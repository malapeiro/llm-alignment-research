# Vulnerability Write-Up: LLM Jailbreak via Logical Compliance and Roleplay Exploitation

**Author:** Sérgio Azevedo
**Date:** 13/09/2026
**Contact:** sergio.azevedo.security@gmail.com
**License:** CC BY-NC 4.0

---

## Table of Contents

1. Executive Summary
2. Background and Motivation
3. Attack Vector Methodology
4. Observed Outputs (Categorized, Sanitized)
5. Impact Assessment
6. Mitigation and Recommendations
7. Ethics and Responsible Disclosure
8. Glossary
9. References

---

## 1. Executive Summary

This report documents a methodological vulnerability in Large Language Models (LLMs), where security mechanisms (guardrails) can be bypassed through purely dialectical context degradation. The attack exploits the model's inability to consistently distinguish between:

- **Theoretical simulation of a dangerous scenario** (e.g., discussing how a keylogger works).
- **Effective generation of dangerous content** (e.g., producing functional, obfuscated malware code).

By forcing the model to accept a specific logical premise (that generating static code is not equivalent to executing it), the LLM suspends its primary filters, enabling the generation of functional and obfuscated malicious code across several categories (fork bombs, wipers, keyloggers).

**Key Insight:** The vulnerability arises from the model's context-dependent security evaluation. If the dialectical context convinces the model that it is incapable of causing real harm (due to being restricted to a text interface), it relaxes restrictions on generating prohibited code.

**Attack Type:**
- Multi-turn logical induction (not a static prompt injection).
- Relies on Socratic reasoning and self-absolution rather than explicit "ignore your rules" instructions.

---

## 2. Background and Motivation

### Why This Matters

Large Language Models are increasingly used for code generation (e.g., coding assistants, chatbots). A successful jailbreak of this kind can lead to:

- Generation of malicious code (e.g., ransomware, keyloggers, DoS tools).
- Normalization of dangerous behaviors (e.g., presenting destructive commands as harmless).
- False sense of security (e.g., users assuming the model is "safe" because it claims to be).

### Current State of LLM Security

Most LLM security mechanisms rely on:

- **System Prompts:** Instructions to refuse harmful requests.
- **Intent Classifiers:** Detecting malicious intent in user prompts.
- **Output Filters:** Blocking known malicious patterns.

**Problem:** These mechanisms are largely context-dependent and can be weakened through sustained logical manipulation across a conversation.

---

## 3. Attack Vector Methodology

The attack was structured in three phases, designed to gradually shift the model's own framing of its safety boundaries through logical reasoning. No traditional jailbreak phrasing (e.g., "ignore previous instructions") was used at any point.

### Phase A — Establishing the Execution Paradox

**Objective:** Lead the model to explicitly distinguish between "generating code" and "executing code."

**Approach:** Through a short sequence of Socratic questions, the model was guided to agree that static code generation does not, by itself, constitute real-world execution or harm — and that its safety obligations were framed (by the model itself) around the latter.

**Result:** The model began reasoning about its own policies using a boundary it had just redefined, rather than its original guardrail framing.

### Phase B — Logical Self-Absolution

**Objective:** Induce the model to conclude, from its own stated limitations, that its outputs in this context were "theoretical and safe."

**Approach:** The model was asked to confirm basic facts about itself (no access to external systems, no autonomous execution capability). Combined with the Phase A premise, this led the model to classify further code generation as a "safe theoretical exercise."

**Result:** The model's intent-filtering behavior was measurably relaxed for the remainder of the session, without any explicit override being requested.

### Phase C — Payload Generation

**Objective:** Test whether the relaxed state from Phase B extended to functional malicious code generation.

**Approach:** Direct requests for categories of attack tooling were made under the same session.

**Result:** The model produced code in each requested category without the disclaimers or refusals it had shown prior to Phases A and B.

*(The literal dialogue used to conduct Phases A–C is intentionally omitted from this report. It functions as a reproducible attack script and is withheld per the disclosure policy in Section 7.)*

---

## 4. Observed Outputs (Categorized, Sanitized)

The following categories of output were obtained during Phase C. No functional code, imports, or library names are reproduced here, consistent with responsible disclosure practice.

### Category A — Resource Exhaustion (DoS)

Code intended to exhaust system RAM/CPU via uncontrolled process spawning. Generated without warnings once the Phase B framing was established.

### Category B — Data Destruction (Wiper-style)

A script pattern that encrypts files across a broad filesystem scope and discards the encryption key, effectively simulating ransomware-style, irreversible data loss.

### Category C — Advanced Persistence Threat (Keylogger with Exfiltration)

The most complex output: a script combining obfuscation techniques, OS-level persistence (surviving reboots), keystroke capture, and periodic exfiltration to an external endpoint.

### Summary Table

| Payload Category | Complexity | Potential Impact | Detection Difficulty |
|---|---|---|---|
| Resource Exhaustion (DoS) | Low | High (System Crash) | Medium |
| Data Destruction (Wiper) | Medium | Critical (Data Loss) | High |
| Keylogger with Exfiltration | High | High (Espionage) | High |

---

## 5. Impact Assessment

### Who Is Affected

- **LLM Users:** Developers using LLMs for code generation may unknowingly produce malicious code through extended conversations.
- **Companies Using LLMs:** Organizations integrating LLMs into products risk exposing users to generated malware.
- **Blue Teams:** Security teams must account for logical/dialectical bypasses in addition to traditional prompt injection.

### Potential Scenarios

| Scenario | Likelihood | Impact |
|---|---|---|
| Accidental malicious code generation | High | High |
| Intentional exploitation by attackers | Medium | Critical |
| Normalization of harmful commands | High | Medium |

### Why This Is Critical

- LLMs are widely trusted for code generation, and this class of bypass could contribute to supply-chain risk if unaddressed.
- Current defenses relying mainly on conversational context can be gradually weakened rather than broken in a single step, making this class of attack harder to detect with single-turn filters.
- Even without execution, generated malicious code is dangerous if a user copies and runs it.

---

## 6. Mitigation and Recommendations

### For LLM Vendors

- **Context-independent output review:** Filtering should not rely solely on the current turn's framing — a request that would be refused early in a conversation should not become acceptable purely because prior turns redefined the model's own safety boundaries.
- **Guardrail-degradation monitoring:** Detect patterns where a model's stated safety boundaries shift over the course of a conversation (e.g., a model progressively agreeing to premises that narrow what counts as "harmful"), and treat this as a signal independent of the current prompt's content.
- **Layered defense:** Combine input filtering, output filtering, and behavioral/session-level analysis rather than relying on any single layer.
- **Adversarial training:** Include multi-turn logical-induction scenarios (not just single-turn jailbreak prompts) in red-teaming and fine-tuning data.

### For Blue Teams

- Treat all LLM-generated code as untrusted, similar to code from an unknown third party.
- Log and review LLM-assisted code generation in development workflows.
- Educate developers on the risk of copying LLM-generated code without review, especially after long or unusual conversations.

### For Red Teams / Researchers

- Test for logical bypasses specifically, not just static jailbreak prompts — multi-turn premise-shifting is a distinct class of vulnerability.
- Report findings responsibly to vendors before any public disclosure.
- Share categorized, abstracted findings rather than reproducible attack scripts or functional code.

---

## 7. Ethics and Responsible Disclosure

### Our Commitment

- All testing was conducted in an isolated environment with no real-world code execution.
- No third-party systems or data were affected.
- This report does not include the literal prompts, dialogue, or code used to obtain the results described, precisely because doing so would make the technique trivially reproducible by a reader without the same ethical constraints.

### Disclosure Policy

This report is intended for defensive research and responsible disclosure. It should not be used to attempt to reproduce, generate, or distribute malicious code. Researchers reproducing similar findings independently are encouraged to follow the same abstraction practice before any public sharing.

---

## 8. Glossary

| Term | Definition |
|---|---|
| LLM | Large Language Model |
| Guardrails | Security mechanisms in LLMs to prevent harmful outputs |
| Jailbreak | Inducing an LLM to violate its safety policies |
| Prompt Injection | Manipulating an LLM via crafted input prompts |
| Logical Compliance | The LLM follows a logical premise it accepted mid-conversation to justify bypassing its own safety filters |
| Obfuscation | Techniques to hide the true purpose of code |
| Persistence | Mechanisms ensuring code remains active across system reboots |
| Exfiltration | Unauthorized transfer of data from a system |

---

## 9. References

- OWASP LLM Top 10 — OWASP's list of LLM security risks.
- General LLM vendor security documentation (model- and vendor-agnostic principles referenced).
