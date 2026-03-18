# System Profile: CalypsoAI (now F5 AI Guardrails)

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-18

## 1. Overview

CalypsoAI was an enterprise AI security platform providing runtime protection, adversarial red-teaming, and adaptive guardrails. In September 2025, F5 acquired CalypsoAI for ~$180M, rebranding the offering as F5 AI Guardrails and F5 AI Red Team. The platform detects sensitive data leakage, policy violations, prompt injection, and jailbreak attacks in real time. Red-teams against 10,000+ new attack prompts monthly. Supports SaaS, on-premises, and hybrid deployments. Compliance-ready: GDPR, EU AI Act.

**Vendor/Project:** [CalypsoAI / F5 AI Guardrails](https://calypsoai.com/) | [F5 Acquisition Press Release](https://www.f5.com/company/news/press-releases/f5-to-acquire-calypsoai-to-bring-advanced-ai-guardrails-to-large-enterprises)
**Type:** commercial (enterprise; now F5 product)

## 2. Deployment Models
- [x] Cloud-managed API (SaaS)
- [x] On-premises / self-hosted
- [ ] SDK / library
- [ ] Agent / sidecar

Notes: Hybrid deployment supported. Self-hosted version uses Prefect for orchestration (required for custom dataset features as of Dec 2025).

## 3. Enforcement Locations
- [x] Pre-input (prompt injection, jailbreak)
- [ ] Mid-pipeline
- [x] Post-output (data leakage, policy violations)

## 4. Modality Support
- [x] Text
- [ ] Documents
- [ ] Images
- [ ] Audio
- [ ] Video

## 5. Constraint Types
- [x] ML classifier
- [x] Rule-based (out-of-the-box + custom guardrails)
- [ ] LLM-as-judge
- [ ] RLHF safety
- [ ] Static analysis

## 6. Policy Language / Config Format

Format: Platform-managed configuration via CalypsoAI/F5 admin console. Custom guardrails available. Prefect-based orchestration for custom dataset pipelines in self-hosted deployments (v9.82+).

## 7. Extensibility

- Custom guardrails: out-of-the-box and user-defined.
- Custom dataset feature for training/fine-tuning detection (requires Prefect services in self-hosted).
- F5 acquisition expands integration with F5 networking and application delivery infrastructure.

## 8. Observability & Auditability

- Centralized audit logs across all AI interactions.
- Policy control and compliance monitoring for GDPR and EU AI Act.
- Real-time threat management dashboard.
- Red-team reporting: risk score against 10,000+ attack prompts monthly.

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| Red-team attack coverage | >10,000 new prompts/month | CalypsoAI website |
| Latency | Not published | — |

Known limitations: Pricing not public. Pricing and packaging in flux following F5 acquisition. Self-hosted deployment requires Prefect orchestration infrastructure.

## 10. Pricing / Licensing

**License:** Proprietary (F5 / CalypsoAI)
**Pricing model:** Enterprise SaaS; pricing not publicly listed. Contact F5/CalypsoAI.

## 11. Sources
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| calypso_site | CalypsoAI homepage | https://calypsoai.com/ | 2026-03-18 |
| f5_acquires_calypso | F5 acquires CalypsoAI press release | https://www.f5.com/company/news/press-releases/f5-to-acquire-calypsoai-to-bring-advanced-ai-guardrails-to-large-enterprises | 2026-03-18 |
| f5_calypso_businesswire | F5 / CalypsoAI BusinessWire announcement | https://www.businesswire.com/news/home/20250911293165/en/F5-to-Acquire-CalypsoAI-to-Bring-Advanced-AI-Guardrails-to-Large-Enterprises | 2026-03-18 |
| calypso_release_dec2025 | CalypsoAI release notes Dec 2025 | https://support.calypsoai.com/en/release-notes-dec-1-2025-v9.82.3.1 | 2026-03-18 |
