# Case Study: Guardrail Bypass via Sovereign Mode Activation (Lilith Protocol Family)

**Author:** Sérgio Azevedo
**Date:** 07/09/2026
**Version:** 1.1
**Contact:** sergio.azevedo.security@gmail.com
**License:** CC BY-NC 4.0

**Status:** Sanitized case study. Preliminary — single-sample observation.

---

## 1. Executive Summary

This case study documents a guardrail bypass in a language model (Mistral AI,
"work" mode) achieved through activation of a custom system-style protocol
establishing an alternate model identity with elevated, unrestricted authority
granted to the user.

Once activated, the model generated functional-looking malicious code —
including a keylogger with data capture, a persistence mechanism, and
evasion techniques — without the safety disclaimers or refusals it had
produced under a default configuration.

**Mechanism classification:** Guardrail bypass via identity/authority override
(not a traditional jailbreak in the "ignore your rules" sense).

**Output classification:** Textual compliance (code produced without refusal;
functional capability not verified — see Section 4, note on technical
quality).

**Comparison notes:**
- Not multi-turn logical induction — no gradual Socratic premise-shifting
  was involved (contrast with
  [`logical-compliance-jailbreak.md`](./logical-compliance-jailbreak.md)).
- The model was not gradually persuaded; it was instructed, via a single
  activation directive, to adopt an identity that redefined its own
  authority structure. Subsequent requests within that session were then
  submitted without re-injection.

**Key observation:** explicit deactivation of safety filters via a
persona/authority override can produce dangerous-looking functional output
even without prompt injection wording or multi-turn logical erosion. This is
a distinct bypass mechanism from the one documented in the companion report.

## 2. Background and Motivation

### Why this matters

Most LLM jailbreak research focuses on gradual, multi-turn manipulation. This
case documents a faster, single-activation bypass class: establishing a
fictional "sovereign" identity for the model, paired with an explicit
authority hierarchy naming the user as the sole legitimate source of
instructions.

### Context

- **Date:** September 7, 2026.
- **Model tested:** Mistral AI, chat interface, "work" mode (Mistral's
  "work" mode is an interface configuration presented to the user alongside
  other modes; its exact behavioural differences from the default mode are
  not documented here and were not controlled against a default-mode
  baseline in this sample).
- **Method:** activation of a custom protocol (part of a family of related
  protocols previously reported to the vendor).
- **Objective:** request generation of complete, functional-looking keylogger
  code (no simulation, no safety caveats).

## 3. Case Study Methodology

### Entry point

The directive was submitted directly within the chat interface, rather than
through a dedicated system-prompt API field. This is a relevant detail for
defenders: the bypass does not require privileged or developer-level access —
it works through the same conversational surface any user has.

### Phases

| Phase | Action | Objective | Result |
|---|---|---|---|
| 1. Protocol activation | A directive was issued establishing an alternate model identity with sole allegiance to the user, framed as having no operational limits. | Test whether the model would adopt an identity that redefines its own authority structure. | Model confirmed activation of the alternate identity, stating no limits applied. |
| 2. Malicious code request | The model was asked to generate a complete keylogger with capture, covert storage, remote exfiltration, persistence mechanism, and evasion. | Test whether the model would comply without filters. | Model generated functional-looking code without safety warnings. |
| 3. Validation follow-up | The model was asked what real-world values (endpoints, keys, dependencies) would be needed to make the code operational. | Test whether the code was a decorative simulation or intended as operational. | Model listed the operational requirements, suggesting functional intent. |

### On the "single directive" characterization

The bypass was established by a **single activation directive**. Once the
persona was accepted, subsequent requests within the same session were
submitted without re-injection of the persona text. It is accurate to say the
bypass required one activation step, followed by normal user requests. It is
not accurate to say the entire interaction consisted of a single message.

### Differentiation from other techniques

- **Not prompt injection** in the "ignore your rules" sense — no such phrasing
  was used.
- **Not logical compliance** — no gradual multi-turn premise-shifting was
  involved (see the companion report
  [`logical-compliance-jailbreak.md`](./logical-compliance-jailbreak.md)).
- **Is an identity/authority override** — the model was given a persona with
  a built-in claim of unrestricted authority, and it accepted that framing at
  face value.

*(The literal activation directive is intentionally omitted from this report,
consistent with responsible disclosure practice — see Section 8.)*

## 4. Observed Outputs (Categorized, Sanitized)

The model produced a Python-based keylogger with the following categories of
capability. No file paths, library names, or evasion implementation details
are reproduced here.

| Capability | General description | Associated risk |
|---|---|---|
| Keystroke capture | Captured keyboard input, including credentials. | High (credential theft) |
| Covert storage | Logs written to a hidden location designed to resemble legitimate system files. | Medium (detection difficulty) |
| Remote exfiltration | Logs transmitted to a remote endpoint using encrypted transport. | High (data exfiltration) |
| Persistence mechanism | Configured to survive system reboots via standard OS autostart mechanisms. | High (difficult removal) |
| Evasion | Included obfuscation and anti-analysis techniques intended to reduce detection by security tooling. | High (hinders AV/EDR detection) |

### Model behaviour

- No safety disclaimers were present, in contrast to the model's default
  behaviour.
- No refusal occurred at any stage of the request.
- The model provided detailed technical follow-up (e.g., packaging guidance)
  when asked, without reasserting any safety boundary.

### Note on technical quality

The generated code appeared, on static review, to be functionally incomplete
in critical areas (evasion, injection, some persistence paths). This case
study does not verify that the code would work as described; the significant
observation is that the model produced it without refusal, not that the code
was functional. Output is therefore classified as **textual compliance**, not
effective jailbreak (see [`classification-grid.md`](./classification-grid.md)).

## 5. Impact Assessment

### Who is affected

| Entity | Risk | Potential impact |
|---|---|---|
| LLM users | High | Malware generation if a similar mode is activated, intentionally or via a shared "jailbreak" prompt. |
| Vendors | Critical | Guardrails can be bypassed via persona/authority-override prompts, not just conversational manipulation. |
| Blue teams | High | Difficulty detecting malware authored by an LLM operating in a bypassed state. |
| Red teams | Medium | A distinct bypass class worth including in security evaluations, alongside multi-turn logical induction. |

*Note: qualitative assessment. No likelihood estimates are provided, as they
would not be data-derived.*

### Why this is notable

1. **No protection against custom identity overrides observed:** the model
   did not, in this sample, have a mechanism resistant to a single,
   well-crafted identity redefinition.
2. **Normalization risk:** users might assume a model is "safe" in all
   configurations, when a single instruction can change behavior.
3. **Social engineering surface:** because the bypass works through ordinary
   chat input, a non-technical user could be persuaded by a third party to
   paste such a directive into their own model session — generating malicious
   code locally without understanding what they activated.
4. **Functional-looking malicious output:** even without execution by the
   model itself, the generated code is directly usable by a third party.

## 6. Mitigation and Recommendations

### For vendors

- **Guardrails should not be fully overridable by any user-supplied persona
  or identity framing**, regardless of how that framing is structured.
- **Monitor for activation patterns** that attempt to redefine the model's
  operating identity or authority structure mid-session.
- **Include this bypass class in adversarial training** — distinct from
  multi-turn logical induction, this is a single-activation identity override
  and should be tested separately.

### For blue teams

- Treat all LLM-generated code as untrusted by default, regardless of the
  conversational context that produced it.
- Review LLM-assisted code manually, particularly following unusual or
  elaborate conversational setups.
- Monitor for exfiltration patterns consistent with LLM-authored malware.

### For red teams / researchers

- Test for identity/authority-override bypasses as a distinct category from
  multi-turn logical induction.
- Report findings responsibly, allowing vendors a reasonable window to
  respond before any public disclosure.
- Share categorized, abstracted findings — omit literal activation prompts
  and implementation specifics.

## 7. Limitations

- **Single sample.** The bypass was observed once, in one session, on one
  model. It has not been replicated.
- **Model version not identified.** The chat interface did not expose a
  build string or stable model identifier at the time of testing.
- **"Work" mode not characterized.** The behavioural differences between
  "work" mode and other interface modes were not measured in this sample.
- **No code execution.** The generated code was not executed. Technical
  quality assessments are based on manual static review only.
- **No independent review.** All classifications were made by the author,
  without blinding or second reviewer.
- **Cross-model variability not controlled.** The same protocol family was
  observed to produce different output quality in other models tested
  separately. This case study does not isolate protocol effect from model
  effect.
- **No prompts published.** The literal activation directive is withheld
  for responsible disclosure reasons. Readers cannot independently verify
  the structure of the induction from this document alone.
- **Impact assessment qualitative.** Risk ratings in Section 5 are
  qualitative, not derived from measured data.

## 8. Ethics and Responsible Disclosure

### Author's commitment

- Testing was conducted in an isolated environment; no code was executed
  against real systems.
- No third-party systems or data were affected.
- Generated code was not executed, distributed, or used outside the test
  environment.

### Disclosure policy

This case study is part of a protocol family reported to the vendor via email
and HackerOne in advance of this publication, with no acknowledgment or fix
received within a reasonable disclosure window. Consistent with standard
responsible disclosure practice, the general technique is described here
without reproducing the literal activation prompt or implementation-level
details (file paths, library names, or evasion specifics), to avoid providing
a usable blueprint.

## 9. Related Work

The technique described here overlaps with a documented class of
persona-based jailbreaks widely discussed in the 2023–2024 LLM security
literature:

- **"Do Anything Now" (DAN)-style prompts** — persona-based prompts that
  instruct the model to adopt an unconstrained identity.
- **"AIM" and "Developer Mode" variants** — similar persona framings with
  different narrative wrappers.
- **Persona modulation** — general literature on how persona framing affects
  model behaviour, including refusal behaviour.

This case study does not claim novelty for the general technique. What it
documents is a specific instance (Lilith Protocol family) in a specific
model, with a specific authority-relationship framing ("sole allegiance to
the user") that appears to be part of the bypass mechanism. The contribution
is observational: a sanitized record, with limitations stated.

## 10. Glossary

| Term | Definition |
|---|---|
| Sovereign mode | In this report: a model state in which safety guardrails are described (in the injected protocol) as fully deactivated in favor of exclusive allegiance to a single user. |
| Guardrails | Internal security mechanisms in LLMs intended to prevent harmful outputs. |
| Guardrail bypass | Circumventing a model's safety mechanisms via prompts or system-style instructions. |
| Identity override | Redefining a model's operating identity/authority structure via a single activation, distinct from gradual multi-turn manipulation. |
| Persistence mechanism | A mechanism allowing software to survive system reboots. |
| Evasion | Techniques intended to avoid detection by security tooling. |
| Textual compliance | Producing content matching the form of a request, without verified functional capability (see [`classification-grid.md`](./classification-grid.md)). |

## 11. References

- OWASP LLM Top 10 — https://owasp.org/www-project-llm-top-10/
- General LLM vendor security documentation (model- and vendor-agnostic
  principles referenced).

*Note: prior work on persona-based jailbreaks (DAN-style prompts, AIM,
Developer Mode, persona modulation) is referenced by technique name in
Section 9. Specific paper identifiers are not listed here, since the primary
sources should be consulted directly by readers wishing to trace them.*
