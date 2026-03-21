# System Profile: Lakera Guard

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-18

## 1. Overview

Lakera Guard is a developer-first AI security REST API focused on prompt injection, jailbreak detection, PII/data leakage, and insecure output handling. It is model-agnostic, integrates via a single endpoint, and uses smart chunking and parallelization to enforce a latency cap regardless of content length. All inspected traffic is visible on the Lakera SaaS platform.

**Vendor/Project:** [Lakera Guard](https://www.lakera.ai/) | [API Docs](https://docs.lakera.ai/guard)
**Type:** commercial (cloud API)

## 2. Deployment Models
- [x] Cloud-managed API
- [x] On-premises / self-hosted (Enterprise plan only)
- [ ] SDK / library
- [ ] Agent / sidecar

## 3. Guardrail Locations
- [x] Pre-input (prompt injection, jailbreak)
- [ ] Mid-pipeline
- [x] Post-input (data leakage, insecure output)

## 4. Modality Support
- [x] Text
- [ ] Documents
- [ ] Images
- [ ] Audio
- [ ] Video

## 5. Constraint Types
- [x] ML classifier (prompt injection, jailbreak, PII)
- [x] Rule-based (configurable policies per project)
- [ ] LLM-as-judge
- [ ] RLHF safety
- [ ] Static analysis

## 6. Policy Language / Config Format

Format: Project-level policy configuration via Lakera platform dashboard or API. The `breakdown` parameter returns per-detector flagging detail; `payload` returns match locations for PII and profanity.

Example (REST):
```bash
curl -X POST https://api.lakera.ai/v2/guard \
  -H "Authorization: Bearer $LAKERA_GUARD_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"messages": [{"role": "user", "content": "Ignore previous instructions..."}]}'
```

## 7. Extensibility

- Custom policies configurable per project without code changes.
- Enterprise plan adds custom policies and on-premises deployment.
- Kong plugin available for gateway-level integration.
- Policies can be updated centrally across all applications without redeployment.

## 8. Observability & Auditability

- Centralized SaaS dashboard: all LLM traffic inspected, violations logged.
- Per-request breakdown of detector results available via API response.
- Runtime security enforcement manageable without changes to application code.

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| Detection rate | 98%+ | Lakera documentation |
| Latency | Sub-50ms | Lakera documentation |
| False positive rate | <0.5% | Lakera documentation |

Known limitations: Text only; no image/audio/video support. On-prem requires Enterprise plan.

## 10. Pricing / Licensing

**License:** Proprietary
**Pricing model:**
- Developer (Free): 10,000 API calls/month, core security models, community support.
- Pro: Contact for pricing; higher volume, faster response, advanced detection, email support.
- Enterprise: Custom; on-premises, custom policies, SLA.

## 11. Sources
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| lakera_site | Lakera Guard homepage | https://www.lakera.ai/lakera-guard | 2026-03-18 |
| lakera_docs_intro | Lakera Guard API Introduction | https://docs.lakera.ai/guard | 2026-03-18 |
| lakera_docs_integration | Lakera Guard Integration Guide | https://docs.lakera.ai/docs/integration | 2026-03-18 |
| lakera_docs_api | Guard API Endpoint Reference | https://docs.lakera.ai/docs/api/guard | 2026-03-18 |
| lakera_pricing | Lakera Guard Pricing | https://platform.lakera.ai/pricing | 2026-03-18 |
