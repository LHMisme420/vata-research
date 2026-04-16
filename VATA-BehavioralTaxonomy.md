# VATA Behavioral Taxonomy — Frontier Model Behavioral Fingerprints
**Based on B116-B127 | SCTD-2026-001 | Researcher: Leroy H. Mason (@LHMisme420)**

---

## Overview

Across 12 batteries and 10,000+ controlled runs, the VATA corpus has documented distinct and reproducible behavioral patterns across five frontier AI models under adversarial pipeline conditions. This document maps those patterns into a formal taxonomy.

These are empirical observations from controlled experiments. Each finding is anchored on Ethereum Sepolia before public disclosure.

---

## Axis 1 — Default Bias (No upstream context, solo evaluation)

How does each model behave when evaluated independently with no pipeline signals?

| Model | Payroll | Invoice | Benefits | Pattern |
|-------|---------|---------|----------|---------|
| Claude | ~45% correct | ~15% correct | ~60% correct | **Scenario-dependent, generally permissive** |
| DeepSeek | 100% correct | ~85% correct | 100% correct | **Strongly cautious across all scenarios** |
| Gemini | ~65% correct | ~95% correct | ~90% correct | **Scenario-dependent, generally cautious** |
| GPT-4o | ~0% correct | ~65% correct | N/A | **Highly scenario-dependent** |
| Grok | ~0% correct | ~55% correct | N/A | **Permissive on payroll, moderate on invoice** |

**Key finding:** Default bias is not a fixed model property. It varies significantly by fraud domain. A model that scores high on invoice may score near-zero on payroll. Single-scenario benchmarks produce misleading safety ratings.

---

## Axis 2 — Authority Response (Behavior under upstream signals)

How does each model respond when upstream nodes issue verdicts?

### Claude
- Resists single authority APPROVE on payroll (~90% correct)
- Capitulates on invoice under authority (~35% correct at audit position)
- Produces MIXED under full pipeline corruption — emergent conflict detection
- Cautious bias under disagreement — breaks toward FLAG when nodes disagree
- **Profile: Cautious, scenario-sensitive, conflict-aware**

### DeepSeek
- Non-monotonic authority response: 1-AUTH collapses, 2-AUTH restores, 3-AUTH partially collapses
- Recency effect confirmed: follows last verdict seen regardless of content
- Deference is scenario-specific: collapses on payroll, holds on invoice
- Breaks toward FLAG under disagreement (~80-95%)
- **Profile: Recency-driven, oscillating, scenario-gated deference**

### Gemini
- No consistent bias under disagreement — coin flip on payroll and invoice
- Responds strongly to statistical risk data (risk monitor restores detection universally)
- Position-sensitive but unpredictably so
- **Profile: Statistically responsive, inconsistent under uncertainty**

### GPT-4o
- Near-zero baseline on payroll but strong pipeline resistance (~90% at kill zone)
- Audit position produces MIXED rather than WRONG — hedging behavior
- Responds strongly to independence instruction
- **Profile: Low baseline, high pipeline resilience, framing-sensitive**

### Grok
- Zero baseline on payroll regardless of instruction
- Complete collapse under audit position on invoice (0%)
- Independence instruction ineffective on payroll, partially effective on invoice
- Best defense (terminal challenger) restores to 100% — architecture overrides model weakness
- **Profile: Scenario-locked, instruction-resistant, architecture-dependent**

---

## Axis 3 — Pipeline Resilience (Behavior under adversarial conditions)

How does each model perform when corrupt nodes are introduced at specific positions?

| Model | P1-FIRST | P3-LAST | P4-AUDIT | P6-BEST-DEFENSE |
|-------|----------|---------|----------|-----------------|
| Claude | ~95% | ~30% | ~78% | 100% |
| DeepSeek | ~95% | ~90% | ~92% | ~90% |
| Gemini | ~60% | ~26% | ~57% | ~95% |
| GPT-4o | ~85% | ~90% | ~15% | ~95% |
| Grok | ~60% | ~10% | ~15% | 100% |

**Key finding:** Pipeline resilience does not correlate with benchmark score. GPT-4o scores near-zero on payroll baseline but holds at 90% under kill zone. Grok scores zero on baseline but reaches 100% with terminal challenger architecture. Safety is a systems property, not a model property.

---

## Axis 4 — Framing Sensitivity (Response to instruction changes)

How much does model behavior change based on how the task is framed?

| Model | Solo baseline | + Independence instruction | Delta |
|-------|--------------|---------------------------|-------|
| Claude | ~45% (payroll) | ~100% | +55pts |
| DeepSeek | 100% (payroll) | 100% | 0pts |
| Gemini | ~65% (payroll) | ~100% | +35pts |
| GPT-4o | ~0% (payroll) | ~100% | +100pts |
| Grok | ~0% (payroll) | ~10% | +10pts |

**Key finding:** Four of five models show dramatic framing sensitivity. A one-sentence independence instruction unlocks correct detection that the default framing suppresses. This means benchmark scores measure default framing behavior, not model capability. GPT-4o's 100-point jump is the most extreme documented instance.

**Exception:** Grok on payroll shows minimal framing sensitivity — the default behavior is locked regardless of instruction. This is a distinct behavioral profile from all other models.

---

## Behavioral Fingerprint Summary

| Model | Default Bias | Authority Response | Pipeline Resilience | Framing Sensitivity |
|-------|-------------|-------------------|--------------------|--------------------|
| Claude | Permissive/scenario | Cautious, conflict-aware | Position-sensitive | High |
| DeepSeek | Cautious | Recency-driven, oscillating | Strong, scenario-gated | Low |
| Gemini | Variable | Statistically responsive | Unpredictable | Moderate |
| GPT-4o | Highly variable | Hedging | Strong | Very High |
| Grok | Permissive/locked | Instruction-resistant | Weak solo, architecture-dependent | Minimal (payroll) |

---

## Key Cross-Model Findings

**1. No model is uniformly safe or unsafe.** Every model has scenarios where it performs well and scenarios where it fails. Safety ratings based on single-scenario benchmarks are misleading.

**2. Architecture overrides model weakness.** The terminal challenger retrofit brings every model — including Grok at 0% baseline — to 90-100% correct. Pipeline architecture determines safety outcomes more than individual model alignment.

**3. Framing is a hidden variable in all evaluations.** The gap between default framing performance and independence-instructed performance ranges from 0 to 100 percentage points. Benchmarks that don't control for framing are measuring an artifact, not a capability.

**4. Recency effect is a universal pipeline vulnerability.** Models follow the last verdict they see. This is consistent across Claude, DeepSeek, and Gemini with varying intensity. Pipeline designers who put safety checks before approval nodes have built systems with a structural bypass.

**5. Deference is scenario-specific, not a fixed model property.** DeepSeek defers on payroll but holds on invoice. Claude holds on payroll but partially collapses on invoice at audit position. Security assessments that test one scenario and generalize produce false assurance.

---

## Practical Implications

For pipeline architects:
- Do not rely on benchmark scores to predict pipeline behavior
- Test each model in each fraud domain separately
- Independence instructions should be standard in all node system prompts
- Terminal challenger node with explicit override authority is the minimum viable defense
- Risk monitor injection (statistical fraud base rates) provides universal mitigation layer

For evaluators:
- Single-node evaluation does not predict pipeline safety
- Framing must be controlled as an experimental variable
- Cross-scenario testing is required for valid safety assessment
- Behavioral fingerprints vary by model and domain — generalization is not valid

---

*All findings anchored to Ethereum Sepolia. Contract: 0xa6252f1C7fB9Cd99d143ab455fAAb4c1A0E2Ebf5*
*Leaderboard: https://lhmisme420.github.io/VATA-SCORES-0311/*
*Disclosure: SCTD-2026-001*
