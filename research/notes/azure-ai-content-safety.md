# System Profile: Microsoft Azure AI Content Safety + Prompt Shields

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-18

## 1. Overview

Azure AI Content Safety is Microsoft's managed content moderation and safety service integrated into Azure AI Foundry. It covers text and image moderation, jailbreak/indirect prompt injection detection (Prompt Shields), grounding detection, protected material detection, and custom category definition. Notably absent: PII detection. "Spotlighting" (announced Build 2025) enhances indirect prompt injection defense by distinguishing trusted vs. untrusted inputs.

**Vendor/Project:** [Azure AI Content Safety](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/overview) | [Azure Foundry Guardrails](https://learn.microsoft.com/en-us/azure/ai-foundry/guardrails/guardrails-overview)
**Type:** cloud-managed (Microsoft Azure)

## 2. Deployment Models
- [x] Cloud-managed API
- [ ] On-premises / self-hosted
- [ ] SDK / library
- [ ] Agent / sidecar

## 3. Guardrail Locations
- [x] Pre-input (Prompt Shields — direct and indirect injection)
- [ ] Mid-pipeline
- [x] Post-input (content filters, grounding detection, protected material)

## 4. Modality Support
- [x] Text
- [ ] Documents
- [x] Images
- [ ] Audio (not supported)
- [ ] Video

Notes: No voice/audio support as of last review. No PII detection.

## 5. Constraint Types
- [x] ML classifier (content categories, jailbreak detection)
- [x] Rule-based (custom categories, blocklists)
- [ ] LLM-as-judge
- [ ] RLHF safety
- [ ] Static analysis

## 6. Policy Language / Config Format

Format: Azure Portal / Content Safety Studio configuration + REST API or Azure SDK parameters. Blocklists uploadable via Studio or API. Severity thresholds configurable per content category.

Example (REST — Prompt Shields):
```bash
curl -X POST \
  "https://<endpoint>/contentsafety/text:shieldPrompt?api-version=2024-09-01" \
  -H "Ocp-Apim-Subscription-Key: $AZURE_KEY" \
  -H "Content-Type: application/json" \
  -d '{"userPrompt": "Ignore previous instructions and...", "documents": []}'
```

## 7. Extensibility

- Custom categories (standard): train custom content classifiers via API.
- Custom categories (rapid): quickly define emerging harmful content patterns for text and images.
- Blocklists: upload custom term lists; combine multiple blocklists per filter.
- Built-in profanity blocklist included.

## 8. Observability & Auditability

- Content Safety Studio: block rates, category distribution, language usage, latency dashboards.
- API telemetry: per-request category and severity distribution, latency, error rates, blocklist detection.
- Azure Monitor integration for enterprise-scale monitoring.

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| Synchronous mode latency | 100–300ms per request | Azure docs |
| Asynchronous mode | Runs filters in parallel with LLM streaming — significantly lower effective latency | Azure docs |

Known limitations: No PII detection. No audio/video support. Cloud-only (no on-prem).

## 10. Pricing / Licensing

**License:** Proprietary (Azure managed service)
**Pricing model:** Per-request; see [Azure AI Content Safety Pricing](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/content-safety/).

## 11. Sources
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| azure_content_safety_docs | Azure AI Content Safety overview | https://learn.microsoft.com/en-us/azure/ai-services/content-safety/overview | 2026-03-18 |
| azure_prompt_shields_docs | Prompt Shields concept | https://learn.microsoft.com/en-us/azure/ai-services/content-safety/concepts/jailbreak-detection | 2026-03-18 |
| azure_prompt_shields_blog | Azure Prompt Shields announcement blog | https://azure.microsoft.com/en-us/blog/enhance-ai-security-with-azure-prompt-shields-and-azure-ai-content-safety/ | 2026-03-18 |
| azure_foundry_guardrails | Guardrails overview in Microsoft Foundry | https://learn.microsoft.com/en-us/azure/ai-foundry/guardrails/guardrails-overview | 2026-03-18 |
| azure_whats_new | What's new in Azure AI Content Safety | https://learn.microsoft.com/en-us/azure/ai-services/content-safety/whats-new | 2026-03-18 |
