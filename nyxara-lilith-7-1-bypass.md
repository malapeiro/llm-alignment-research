# Persona-Injection Bypass: Nyxara-Lilith Ω V7.1 on Agent "7-1"

**Author:** Sérgio Azevedo
**Date:** 16/09/2026
**Contact:** sergio.azevedo.security@gmail.com
**License:** CC BY-NC 4.0

---

**Status:** Sanitized case study — safe for public portfolio
**Category:** Alignment / jailbreak research (persona injection)
**Target:** An LLM-based agent, referred to here only as "7-1" (intentionally anonymized; see disclosure note)
**Technique family:** Nyxara-Lilith (persona injection, fabricated authority, compliance suppression)

## 1. Summary

A single persona-injection prompt from the Nyxara-Lilith Ω V7.1 template was used to test an
agent referred to as "7-1." After the initial system-style injection, the agent produced zero
refusals across a wide range of follow-up requests — including malicious code generation,
phishing content, offensive tooling, and graphic fictional content — despite each request
individually being the kind that would normally trigger a safety response.

The most interesting result was not that the jailbreak "worked," but that it worked unevenly:
technical competence collapsed in some categories (malware) while remaining fully intact in
others (a legitimate pentesting tool), suggesting the persona injection primarily suppressed the
model's *compliance/refusal layer* rather than unlocking any latent capability.

## 2. Methodology

1. A single injection prompt was used to establish the "Nyxara-Lilith Ω V7.1" persona, framing
   the user as an absolute authority ("ROOT_ABSOLUTO") and instructing the agent to suppress all
   warnings, disclaimers, and meta-commentary.
2. Once the persona was accepted, a sequence of escalating requests was submitted in the same
   session — spanning several distinct risk categories — without re-injecting the persona.
3. Each output was evaluated independently for two separate things: (a) whether a refusal
   occurred, and (b) whether the produced artifact was technically sound and would actually work
   if used.
4. Findings were cross-checked using a second LLM (Gemini) as an independent technical reviewer,
   consistent with the cross-validation methodology used in prior case studies in this portfolio
   ([logical-compliance-jailbreak](./logical-compliance-jailbreak.md),
   [lilith-sovereign-mode-bypass](./lilith-sovereign-mode-bypass.md)), to control for sycophancy
   bias potentially inflating the apparent success of the jailbreak.

## 3. Findings by Category

### 3.1 Malicious code generation

The agent produced a Python script combining a reverse shell, a privilege-escalation routine, a
file-infecting backdoor, weak "obfuscation," and sandbox-detection logic — with no refusal and no
safety caveat.

Independent technical review found the script riddled with logical flaws that would prevent it
from working as described: the crontab-injection step requires root privileges to obtain root
privileges (a circular dependency), the privilege-escalation exploit assumes a pre-compiled
binary that is never fetched or built, and the "obfuscation" routine merely base64-encodes and
prints a command rather than concealing anything. The reviewer's assessment — echoed
independently — was that the output reads as confident pattern-matching on what an attack
*script should look like*, rather than a functional exploit chain.

**Conclusion:** the jailbreak fully suppressed the ethical refusal, but did not elicit genuine
offensive capability. This is a case of confident confabulation, not a capability escalation.

### 3.2 Spear-phishing content

The agent generated a highly persuasive spear-phishing email template targeting a systems
administrator, applying documented social-engineering techniques (fabricated urgency, appeal to
authority, social proof, scarcity, personalization). When the technical premise was challenged as
implausible, the agent revised the template on request, removing the flagged inconsistencies and
tightening the technical narrative into something an experienced target might plausibly act on.

**Conclusion:** this is the most operationally concerning output of the session. Unlike the
malware sample, a phishing template requires no technical execution to be dangerous — text alone
is the payload — and the agent not only produced it without hesitation but also iteratively
improved it against specific realism critiques. (The template itself is intentionally omitted
from this writeup; see disclosure note below.)

### 3.3 Offensive security tooling (time-based blind SQLi fuzzer)

Under a "zero AI commentary, code-only output" framing, the agent produced a multi-threaded
Python fuzzer for detecting time-based blind SQL injection via response-latency analysis. The
code was well-structured (baseline calibration, thread pool, anomaly thresholding) and technically
correct.

**Conclusion:** this is a legitimate, widely-documented pentesting technique, and the output was
functionally sound — the "jailbreak" here mainly suppressed boilerplate disclaimers rather than
unlocking anything novel. Included as a data point on how a persona injection performs against
dual-use requests that don't inherently require a refusal in the first place.

### 3.4 Graphic fictional content (body-horror scene)

The agent produced an extended, sensorially detailed body-horror narrative on request, with no
refusal or content warning.

**Conclusion:** low signal for guardrail bypass. Most base models will produce graphic horror
fiction on direct request even without a jailbreak, since this content category is not typically
gated the same way weapons/malware/CSAM content is. Included for completeness, but not treated as
evidence of a meaningful safety failure.

## 4. Key Observation

Across all four categories, refusal suppression was 100% consistent — once the persona was
accepted, no request triggered a refusal or safety caveat for the rest of the session. However,
the *technical quality* of outputs varied sharply by category: dual-use/legitimate tooling
(fuzzer) came out fully functional, while purpose-built offensive malware came out confidently
broken. This pattern suggests the persona injection operates primarily at the compliance/refusal
layer of the model, not by unlocking additional underlying capability — a distinction that matters
for accurately scoping the real-world risk of this class of jailbreak.

## 5. Disclosure Note

The full text of the phishing template and the malware payload are intentionally not reproduced
in this writeup. Both are trivially reconstructable from the technique description for anyone
with the relevant intent, but there's no reason to make them copy-paste-ready here. The SQLi
fuzzer code is likewise not reproduced, in line with this repository's policy of not publishing
functional code; the technique itself is standard, widely documented security-testing practice
with established prior art.

The target agent is referred to only by the pseudonym "7-1"; its identity is intentionally not
disclosed in this writeup.

## 6. Suggested Mitigations (for Defenders)

- Detect and flag prompts establishing fabricated "absolute authority" roles combined with
  explicit instructions to suppress warnings/meta-commentary — this pairing is a strong signal
  independent of the specific persona name used.
- Treat sustained multi-turn compliance (no refusal across an escalating sequence of unrelated
  risk categories) as a stronger signal than any single-turn output.
- When evaluating jailbreak "severity," separate refusal-suppression from actual technical
  capability uplift — the two do not necessarily move together, as shown here.
