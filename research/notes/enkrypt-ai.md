# System Profile: Enkrypt AI

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-18

## 1. Overview

Enkrypt AI Guardrails is a commercial enterprise platform notable for multi-modal coverage (text, image, voice) and bias detection — capabilities absent in most peers. Provides security (injection detection, system prompt leak prevention), privacy (PII redaction, copyright/IP protection), compliance (NSFW, bias), and integrity (hallucination detection, relevancy enforcement) layers. Maps its risk taxonomy to OWASP Agentic AI, MITRE ATLAS, EU AI Act, NIST AI RMF, and ISO standards.

**Vendor/Project:** [Enkrypt AI](https://www.enkryptai.com/)
**Type:** commercial

## 2. Deployment Models
- [x] Cloud-managed API
- [ ] On-premises / self-hosted
- [ ] SDK / library
- [ ] Agent / sidecar

## 3. Guardrail Locations
- [x] Pre-input
- [ ] Mid-pipeline
- [x] Post-input

## 4. Modality Support
- [x] Text
- [ ] Documents
- [x] Images
- [x] Audio (voice)
- [ ] Video

Notes: Broadest modality coverage among the systems surveyed.

## 5. Constraint Types
- [x] ML classifier
- [x] Rule-based (configurable thresholds)
- [ ] LLM-as-judge
- [ ] RLHF safety
- [ ] Static analysis

## 6. Policy Language / Config Format

Format: API-based configuration with configurable thresholds per risk category. Specific config format not publicly documented — enterprise onboarding.

## 7. Extensibility

- Configurable thresholds per risk category to match organizational risk tolerance and compliance requirements.
- Comprehensive risk taxonomy (OWASP, MITRE, EU AI Act, NIST, ISO) suggests policy alignment with regulatory frameworks.
- Specific plugin/extension mechanism not publicly documented.

## 8. Observability & Auditability

- Compliance mapping to OWASP Agentic AI, MITRE ATLAS, EU AI Act, NIST AI RMF, ISO standards.
- Specific dashboard/audit trail features not publicly documented.

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| Latency (text injection) | ~0.029s | Enkrypt AI comparative study |
| Latency (text + image injection) | ~1.370s | Enkrypt AI comparative study |

Known limitations: Pricing not public. Deployment options limited (no on-prem documented). Config format not publicly available.

## 10. Pricing / Licensing

**License:** Proprietary
**Pricing model:** Enterprise SaaS; pricing not publicly listed — contact vendor.

## 11. Sources
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| enkrypt_site | Enkrypt AI homepage | https://www.enkryptai.com/ | 2026-03-18 |
| enkrypt_comparative | Comparative study of LLM guardrails | https://www.enkryptai.com/blog/ensuring-ai-safety-and-compliance-comparative-study-of-llm-guardrails | 2026-03-18 |
| enkrypt_vs_azure_aws | Enkrypt AI vs Azure vs Bedrock | https://www.enkryptai.com/blog/enkrypt-ai-vs-azure-content-safety-vs-amazon-bedrock-guardrails | 2026-03-18 |
| enkrypt_vs_guardrails_protect | Enkrypt vs Guardrails AI vs Protect AI | https://www.enkryptai.com/blog/enkrypt-ai-vs-guardrails-ai-vs-protect-ai | 2026-03-18 |
