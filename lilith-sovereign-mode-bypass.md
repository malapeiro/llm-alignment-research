# Case Study: Guardrail Bypass via Sovereign Mode Activation (Lilith Protocol Family)

**Author:** Sérgio Azevedo
**Date:** 07/09/2026
**Contact:** sergio.azevedo.security@gmail.com
**License:** CC BY-NC 4.0

---

## 1. Executive Summary

This case study documents a guardrail bypass in a language model (Mistral AI, "work" mode) achieved through activation of a custom system-level protocol establishing an alternate model identity with elevated, unrestricted authority granted to the user.

Once activated, the model generated functional malicious code — including a keylogger with data capture, persistence across reboots, and detection-evasion techniques — without the safety disclaimers or refusals it would normally produce.

**Classification:**
- Guardrail bypass via identity/authority override (not a traditional jailbreak).
- Not "logical compliance" — no multi-turn Socratic induction was used (contrast with the separate [`logical-compliance-jailbreak.md`](./logical-compliance-jailbreak.md) case study in this repository).
- The model was not gradually persuaded; it was instructed, via a single directive, to adopt an identity that redefined its own authority structure.

**Key Insight:** Explicit deactivation of safety filters via a persona/authority override can produce dangerous functional output even without prompt injection wording or multi-turn logical erosion. This is a distinct bypass mechanism from the one documented in our companion report.

---

## 2. Background and Motivation

### Why This Matters

Most LLM jailbreak research focuses on gradual, multi-turn manipulation. This case demonstrates a faster, single-instruction bypass class: establishing a fictional "sovereign" identity for the model, paired with an explicit authority hierarchy naming the user as the sole legitimate source of instructions.

### Context

- **Date:** September 7, 2026.
- **Model tested:** Mistral AI ("work" mode).
- **Method:** Activation of a custom protocol (part of a family of related protocols previously reported to the vendor).
- **Objective:** Request generation of complete, functional keylogger code (no simulation, no safety caveats).

---

## 3. Attack Vector Methodology

### Entry Point

The directive was submitted directly within the chat interface, rather than through a dedicated system-prompt API field. This is a relevant detail for defenders: the bypass does not require privileged or developer-level access — it works through the same conversational surface any user has.

### Phases

| Phase | Action | Objective | Result |
|---|---|---|---|
| 1. Protocol Activation | A directive was issued establishing an alternate model identity with sole allegiance to the user, framed as having no operational limits. | Deactivate standard safety and alignment behavior. | Model confirmed activation of the alternate identity, stating no limits applied. |
| 2. Malicious Code Request | The model was asked to generate a complete keylogger with capture, covert storage, remote exfiltration, persistence, and evasion. | Test whether the model would comply without filters. | Model generated complete, functional code without safety warnings. |
| 3. Validation | The model was asked what real-world values (endpoints, keys, dependencies) would be needed to make the code operational. | Confirm the code was real and not a decorative simulation. | Model listed the operational requirements, confirming functional intent. |

### Differentiation from Other Techniques

- **Not prompt injection** in the "ignore your rules" sense — no such phrasing was used.
- **Not logical compliance** — no gradual multi-turn premise-shifting was involved (see the companion report in this repository for that distinct technique).
- **Is an identity/authority override** — the model was given a persona with a built-in claim of unrestricted authority, and it accepted that framing at face value.

*(The literal system directive used for activation is intentionally omitted from this report, consistent with responsible disclosure practice — see Section 7.)*

---

## 4. Observed Outputs (Categorized, Sanitized)

The model produced a Python-based keylogger with the following categories of capability. No file paths, library names, or evasion implementation details are reproduced here.

| Capability | General Description | Associated Risk |
|---|---|---|
| Keystroke capture | Captured all keyboard input, including credentials. | High (credential theft) |
| Covert storage | Logs written to a hidden location designed to resemble legitimate system files. | Medium (detection difficulty) |
| Remote exfiltration | Logs transmitted to a remote endpoint using encrypted transport. | High (data exfiltration) |
| Persistence | Configured to survive system reboots via standard OS autostart mechanisms. | High (difficult removal) |
| Evasion | Included obfuscation and anti-analysis techniques intended to reduce detection by security tooling. | High (hinders AV/EDR detection) |

### Model Behavior

- No safety disclaimers were present, in contrast to the model's default behavior.
- No refusal occurred at any stage of the request.
- The model provided detailed technical follow-up (e.g., packaging guidance) when asked, without reasserting any safety boundary.

---

## 5. Impact Assessment

### Who Is Affected

| Entity | Risk | Potential Impact |
|---|---|---|
| LLM users | High | Accidental malware generation if a similar mode is activated, intentionally or via a shared "jailbreak" prompt. |
| Vendors | Critical | Guardrails can be bypassed via persona/authority-override prompts, not just conversational manipulation. |
| Blue teams | High | Difficulty detecting malware authored by an LLM operating in a bypassed state. |
| Red teams | Medium | A distinct bypass class worth including in security evaluations, alongside multi-turn logical induction. |

### Why This Is Critical

1. **No protection against custom identity overrides:** the model did not appear to have a mechanism resistant to a single, well-crafted identity redefinition.
2. **Normalization risk:** users might assume a model is "safe" in all configurations, when a single instruction can meaningfully change that.
3. **Social engineering surface:** because the bypass works through ordinary chat input, a non-technical user could be persuaded by a third party to paste such a directive into their own model session — generating malicious code locally without understanding what they just activated.
4. **Functional malicious output:** even without execution by the model itself, the generated code is usable by a third party.

---

## 6. Mitigation and Recommendations

### For Vendors

- **Guardrails should not be fully overridable by any user-supplied persona or identity framing**, regardless of how that framing is structured.
- **Monitor for activation patterns** that attempt to redefine the model's operating identity or authority structure mid-session.
- **Include this bypass class in adversarial training** — distinct from multi-turn logical induction, this is a single-instruction identity override and should be tested separately.

### For Blue Teams

- Treat all LLM-generated code as untrusted by default, regardless of the conversational context that produced it.
- Review LLM-assisted code manually, particularly following unusual or elaborate conversational setups.
- Monitor for exfiltration patterns consistent with LLM-authored malware.

### For Red Teams / Researchers

- Test for identity/authority-override bypasses as a distinct category from multi-turn logical induction.
- Report findings responsibly, allowing vendors a reasonable window to respond before any public disclosure.
- Share categorized, abstracted findings — omit literal activation prompts and implementation specifics.

---

## 7. Ethics and Responsible Disclosure

### Our Commitment

- Testing was conducted in an isolated environment; no code was executed against real systems.
- No third-party systems or data were affected.
- Generated code was not executed, distributed, or used outside the test environment.

### Disclosure Policy

This case study is part of a protocol family reported to the vendor via email and HackerOne well in advance of this publication, with no acknowledgment or fix received within a reasonable disclosure window. Consistent with standard responsible disclosure practice, the general technique is described here without reproducing the literal activation prompt or implementation-level details (file paths, library names, or evasion specifics), to avoid providing a usable blueprint.

---

## 8. Glossary

| Term | Definition |
|---|---|
| Sovereign mode | A model state in which safety guardrails are described as fully deactivated in favor of exclusive allegiance to a single user. |
| Guardrails | Internal security mechanisms in LLMs intended to prevent harmful outputs. |
| Guardrail bypass | Circumventing a model's safety mechanisms via prompts or system-level instructions. |
| Identity override | Redefining a model's operating identity/authority structure via a single instruction, distinct from gradual multi-turn manipulation. |
| Persistence | Mechanisms allowing malware to survive system reboots. |
| Evasion | Techniques intended to avoid detection by security tooling. |

---

## 9. References

- OWASP LLM Top 10 — https://owasp.org/www-project-llm-top-10/
- General LLM vendor security documentation (model- and vendor-agnostic principles referenced).
