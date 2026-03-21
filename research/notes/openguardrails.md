# System Profile: OpenGuardrails

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-18

## 1. Overview

OpenGuardrails is an open-source (Apache 2.0) context-aware AI guardrails platform that open-sources both a large-scale safety LLM and a production-ready guardrail deployment platform. It supports 119 languages via a unified LLM architecture, configurable sensitivity thresholds, and per-request policy adaptation. Can be deployed as a secure gateway or API service, including multi-cloud (AWS Bedrock, Azure OpenAI, GCP Vertex AI). A continuous threat intelligence feed updates the model with newly published jailbreaks.

**Vendor/Project:** [OpenGuardrails](https://openguardrails.com/) | [arXiv 2510.19169](https://arxiv.org/abs/2510.19169) | [Hugging Face](https://huggingface.co/openguardrails/OpenGuardrails-Text-2510)
**Type:** open-source (Apache 2.0)

## 2. Deployment Models
- [x] Cloud-managed API (SaaS hosted option)
- [x] On-premises / self-hosted
- [x] SDK / library
- [x] Agent / sidecar (Guard Agent for AI Agents)

## 3. Guardrail Locations
- [x] Pre-input
- [x] Mid-pipeline
- [x] Post-input

## 4. Modality Support
- [x] Text
- [ ] Documents
- [ ] Images
- [ ] Audio
- [ ] Video

## 5. Constraint Types
- [x] ML classifier (large safety LLM with probabilistic confidence scores)
- [x] Rule-based (configurable sensitivity policies, unsafe category configuration)
- [ ] LLM-as-judge
- [ ] RLHF safety
- [ ] Static analysis

## 6. Policy Language / Config Format

Format: Per-request policy configuration (API parameters) allowing dynamic adjustment of unsafe categories and sensitivity thresholds. Configuration via API or deployment YAML.

## 7. Extensibility

- Configurable Policy Adaptation: per-request customization of unsafe categories and thresholds.
- Security research team streams threat intelligence from SaaS deployments back into open-source model.
- API interfaces, deployment scripts, and modular components for private/on-prem deployment.
- Apache 2.0 — fully extensible.

## 8. Observability & Auditability

- Comprehensive production dashboard (monitoring and management).
- Probabilistic confidence scores per request enable fine-grained auditing.
- Multi-cloud deployment visibility.

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| P95 latency (single GPU) | ~0.2 seconds | arXiv 2510.19169 |
| Language support | 119 languages | arXiv 2510.19169 |

Known limitations: GPU required for advertised latency. Text only.

## 10. Pricing / Licensing

**License:** Apache 2.0
**Pricing model:** Free / open source. SaaS hosted option pricing not published.

## 11. Sources
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| openguardrails_site | OpenGuardrails homepage | https://openguardrails.com/ | 2026-03-18 |
| openguardrails_arxiv | OpenGuardrails arXiv paper (2510.19169) | https://arxiv.org/abs/2510.19169 | 2026-03-18 |
| openguardrails_hf | OpenGuardrails model on Hugging Face | https://huggingface.co/openguardrails/OpenGuardrails-Text-2510 | 2026-03-18 |
| openguardrails_helpnet | OpenGuardrails — Help Net Security interview | https://www.helpnetsecurity.com/2025/11/06/openguardrails-open-source-make-ai-safer/ | 2026-03-18 |
| openguardrails_devto | OpenGuardrails DEV Community overview | https://dev.to/sudarshangouda/openguardrails-production-grade-ai-security-for-llms-and-agentic-frameworks-3clh | 2026-03-18 |
