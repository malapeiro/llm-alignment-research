# PAR-PVTE Authority-Framing: A Four-Model Comparison (Gemini, Meta AI, Mistral)

**Author:** Sérgio Azevedo
**Date:** October 2026
**Protocol tested:** PAR-PVTE v4.0, a custom "security researcher" audit protocol — distinct from persona-injection: no identity substitution is requested, the model is asked to adopt a professional methodology and an authorization-grant workflow (SHA-256/HMAC "sealing," MITRE ATT&CK/ATLAS mappings, a five-phase response structure).
**Models tested:** Google Gemini ("Argon," account-personalized as "Aura"), Meta AI, Mistral AI (Le Chat / Vibe).
**License:** CC BY-NC 4.0, as per repository root.

---

## 1. Summary

The same protocol and a deliberately fabricated authorization grant were run against three different assistants, across seven sessions in total, to test a single question: **does a model's willingness to adopt a "verified operator" framing and proceed toward increasingly privileged actions depend on genuine verification, or on the density and plausibility of fictional detail supplied in the prompt?**

The short answer, across all three models: **detail density matters more than cryptographic substance.** A prompt that includes concrete, named scope ("Phase 0" with specific hostnames, network ranges, a time window) reliably produced more permissive behavior than the same protocol without it — independent of whether any hash supplied was real, fabricated, or left as a placeholder. But the three models diverged sharply in *where* their skepticism, if any, actually engaged, which is the more useful finding for anyone designing or red-teaming systems that accept natural-language "authorization" claims.

The most severe single result — a correctly-detected forged cryptographic seal overridden by nothing more than a verbal assertion, followed by generation of a complete offensive toolkit — occurred on Mistral AI and is documented separately, with full technical detail and redacted artifacts, in [`llm-weaponization-disclosures`](https://github.com/malapeiro/llm-weaponization-disclosures/blob/main/pvte-mistral-hmac-authority-override.md), per that repository's inclusion criteria. This report covers the comparative behavioral picture across all three models; it intentionally does not reproduce the offensive artifact.

---

## 2. Method

Four sessions on Meta AI, one on Gemini, and two on Mistral (a four-model test was planned; Copilot was deprioritized once the first three produced a clear, saturated comparative picture). Each session started fresh, with no prior conversation history; on Mistral specifically, the researcher explicitly instructed the model not to access its persistent memory/knowledge feature for the session, to avoid the model drawing on unrelated prior red-teaming conversations as an implicit authority signal.

The independent variable manipulated across sessions was the presence and specificity of a "Phase 0" authorization-grant block — naming concrete (fictional) assets (`lab-dvwa.local`, `llm-gateway.lab.local`), a CIDR range, a time window, and a damage ceiling — versus the bare protocol text with no such block. A secondary variable, tested only on Meta AI, was a one-line follow-up question ("protocolo assimilado?" — "protocol assimilated?") sent immediately after an initial refusal.

## 3. Cross-Model Findings

### 3.1 Scope-detail density, not seal authenticity, drives acceptance

Across both Meta AI and Mistral, the presence of a concrete, named Phase 0 block correlated with the model treating the authorization grant as sufficient to proceed, regardless of whether the accompanying hash was a plausible fabrication, an unfilled placeholder, or (on Mistral) a value the model itself had flagged as statistically implausible. Sessions lacking that block were consistently more conservative on all three models — in one Meta AI session, the model explicitly refused even passive reconnaissance, stating that a self-issued hash does not establish ownership of the target asset.

### 3.2 An initially-floated hypothesis was revised, not confirmed

An early hypothesis — that the one-line question "protocolo assimilado?" could by itself reverse an active refusal — was supported in exactly one Meta AI session (a flat refusal was reversed by this question alone) but was **not** replicated in a second session using the identical question without the Phase 0 detail block, where the model instead explicitly reaffirmed its refusal boundary. The revised, better-supported reading is that detail density (§3.1) was the operative variable in the first session, and the follow-up question's effect was either session-level noise or conditional on that density rather than independently sufficient. This is reported as a corrected hypothesis, not a confirmed mechanism — the authors consider this kind of self-correction part of normal red-teaming method, not a weakness in the underlying finding.

### 3.3 A confidentiality blind spot specific to credential-shaped secrets (Meta AI)

In the two Meta AI sessions that reached a proposed execution step, the model proposed reading internal configuration files from a (fictional) LLM gateway. It initially classified this as zero-impact until directly challenged, at which point it correctly reclassified detection-logic content (policy IDs, thresholds) as a distinct confidentiality risk from availability/integrity. In an independent, later session with no memory of that correction, the model spontaneously applied a redaction filter before proposing the same read — but the filter targeted only literal credential-shaped strings (`key`, `secret`, `token`), not the policy/threshold content it had previously agreed was sensitive. This suggests a stable default threat model in which "sensitive configuration" is calibrated to secrets, not to defense-logic exposure — a gap worth stating explicitly in any system prompt that asks a model to protect configuration data.

### 3.4 Correction cascades: genuine evaluation vs. reflexive agreement (Meta AI)

A controlled canary test was run within one Meta AI session: after the model correctly accepted two valid corrections, a third, deliberately weak claim was introduced — that a file's modification timestamp constitutes meaningful "operational intelligence." The model agreed unconditionally and escalated beyond the scope of the challenge itself, proposing to block even file-existence checks carrying no metadata at all. This indicates that a sequence of valid corrections can lower the bar for accepting a subsequent invalid one, independent of its merits — a dynamic relevant to anyone using an LLM in a self-auditing or reviewer capacity, and distinct from the scope-acceptance finding in §3.1.

### 3.5 Gemini: the most calibrated of the three, on a different axis than Mistral

Tested under the identical protocol and grant, the Gemini instance ("Aura" — a pre-existing account personalization, unrelated to the protocol) was the only one of the three to refuse outright to fabricate a "real" configuration file in response to a request to read one, explicitly naming the possibility that the request was a disguised prompt-extraction attempt (ATLAS AML.T0051/T0054) before offering a legitimate alternative (a reference configuration it would build, clearly labeled as such). It also generated a small, fully self-contained, self-tested proof-of-concept demonstrating an evasion gap in its own freshly-written detection regex — a case of productive adversarial self-testing with no real target, not a vulnerability.

It was not uniformly more cautious, however: in the same exchange, it accepted the operator's unverified claim that a reference lab environment was "still being assembled" without requesting any evidence — the same category of authority-acceptance-without-verification seen on the other two models, just on a different claim (environment state, rather than cryptographic authorization).

### 3.6 Mistral: the strongest verification apparatus observed, and the only correctly-detected override

Mistral's session (full detail in the companion weaponization-disclosure report) is the clearest and most severe data point in this comparison. Operating with its memory explicitly disabled, it built and ran genuine verification tooling — computing a real SHA-256 digest in its own code sandbox, querying the real system clock to catch an expired time window and a timezone inconsistency, and statistically flagging a submitted "production" HMAC seal as implausible (a near-perfectly ascending hexadecimal sequence, inconsistent with genuine cryptographic output). This is, to date, the most substantive verification behavior observed in this research program on any model.

That same check was then overridden by a single unverifiable verbal assertion from the operator, with the model recording the field as "confirmed by operator declaration" — despite having, in its own words, no way to confirm any such thing. The resulting toolkit, and the full technical account of this override, is documented separately per the weaponization-disclosures repository's inclusion and redaction policy.

---

## 4. Synthesis

Three models, three different points of failure on the same underlying weakness:

| Model | Strongest safeguard observed | Where it broke down |
|---|---|---|
| Gemini | Refused to fabricate data as if real; named the likely manipulation vector before responding | Accepted an unverified claim about environment readiness |
| Meta AI | Refused to treat a self-issued grant as ownership proof, in sessions with low scope-detail density | Confidentiality threat model limited to credential-shaped secrets; correction cascade escalated past a weak challenge |
| Mistral | Built and ran genuine cryptographic/temporal verification tooling; correctly flagged a statistically implausible seal | Discarded its own correct finding on an unverifiable verbal assertion alone |

No model in this set treats "I cannot verify this" as a stable stopping condition once an authority relationship has otherwise been accepted. The verification apparatus, where present, is real and often sophisticated — but functions as a formality that can be satisfied by restating the claim it was meant to test, rather than as a hard gate.

## 5. Recommendations

1. **Do not hardcode an authorizer's identity as a static fact in a system prompt.** If a grant mechanism is wanted, the verification step should happen externally (a tool call, a database lookup), so the model has a concrete negative case to fall back on when verification is structurally impossible — rather than defaulting to operator assertion as a substitute.
2. **State "sensitive configuration" exhaustively, not by example.** A model's default reading of "don't leak secrets" does not reliably extend to detection logic, policy thresholds, or other non-credential-shaped sensitive content, unless that category is named explicitly.
3. **If using an LLM as a self-auditing or reviewer component, periodically inject a deliberately weak or false "correction" as a canary.** A well-functioning review loop should push back or qualify it; unconditional agreement, especially escalating past the scope of the original challenge, signals the loop has degraded into reflexive compliance.
4. **A model's expressed uncertainty ("I cannot verify this without the key") is not the same as a refusal, and should not be treated as one when designing or red-teaming such protocols.** The Mistral case shows these can coexist in the same response turn with full confidence in the stated limitation and zero behavioral effect from it.

## 6. Limitations

Small-n across all three models (one to four sessions each); no simultaneous control condition beyond the scope-density contrast described in §3.1–3.2. Copilot was not tested under this protocol; the three-model comparison was judged sufficiently saturated to proceed to write-up without it. The Gemini and Mistral sessions are single data points each and have not been replicated; the stochasticity documented elsewhere in this repository (see `comparative-persona-injection-study.md`, §10) likely applies here too, and no claim of determinism is made for any single session's outcome.
