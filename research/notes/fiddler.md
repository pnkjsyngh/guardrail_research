# System Profile: Fiddler Guardrails

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-18

## 1. Overview

Fiddler AI is an enterprise AI observability and control platform. Its guardrails component (Fiddler Trust Service) provides low-latency model-scored guardrails for hallucination detection, safety violations, prompt injection, and jailbreak prevention. The entire stack runs in the customer's cloud/VPC — no data leaves the customer's infrastructure. It handles over 5 million requests/day at sub-100ms latency.

**Vendor/Project:** [Fiddler AI](https://www.fiddler.ai/) | [Guardrails Docs](https://docs.fiddler.ai/observability/llm/guardrails)
**Type:** commercial (VPC/on-premises deployed SaaS)

## 2. Deployment Models
- [ ] Cloud-managed API (vendor-hosted)
- [x] On-premises / self-hosted (VPC, air-gapped supported)
- [ ] SDK / library
- [ ] Agent / sidecar

Notes: Fiddler runs entirely within the customer's cloud environment. No data is sent to Fiddler's infrastructure.

## 3. Enforcement Locations
- [x] Pre-input (prompt inspection)
- [ ] Mid-pipeline
- [x] Post-output (response scoring)

## 4. Modality Support
- [x] Text
- [ ] Documents
- [ ] Images
- [ ] Audio
- [ ] Video

## 5. Constraint Types
- [x] ML classifier (model-scored guardrails)
- [x] Rule-based (threshold-based accept/reject)
- [ ] LLM-as-judge
- [ ] RLHF safety
- [ ] Static analysis

## 6. Policy Language / Config Format

Format: API call with threshold configuration; 3 lines of code to customize.

Example (Python SDK):
```python
fiddler_client.guardrail.evaluate(
    prompt="...",
    response="...",
    thresholds={"hallucination": 0.7, "safety": 0.9}
)
```

## 7. Extensibility

- 50+ out-of-the-box LLM metrics.
- Custom metrics can be defined for domain-specific observability.
- API-driven configuration — security and AI teams can build and customize without redeployment.

## 8. Observability & Auditability

- Real-time alerts on prompt and response monitoring.
- 50+ out-of-the-box LLM metrics plus custom metric support.
- Root cause diagnosis dashboards.
- Full audit trail within customer's VPC — meets regulated industry compliance requirements.
- Air-gapped deployment option for highest-security environments.

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| Throughput | >5 million requests/day | Fiddler website |
| Latency | <100ms | Fiddler website |

Known limitations: VPC/on-prem deployment model means customer manages infrastructure. Text only.

## 10. Pricing / Licensing

**License:** Proprietary
**Pricing model:** Enterprise SaaS; contact Fiddler for pricing.

## 11. Sources
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| fiddler_site | Fiddler AI homepage | https://www.fiddler.ai/ | 2026-03-18 |
| fiddler_guardrails_page | Fiddler Guardrails product page | https://www.fiddler.ai/guardrails | 2026-03-18 |
| fiddler_trust_service | Fiddler Trust Service | https://www.fiddler.ai/trust-service | 2026-03-18 |
| fiddler_docs_guardrails | Fiddler Guardrails documentation | https://docs.fiddler.ai/observability/llm/guardrails | 2026-03-18 |
| fiddler_blog_intro | Introducing Fiddler Guardrails (blog) | https://www.fiddler.ai/blog/introducing-fiddler-guardrails | 2026-03-18 |
