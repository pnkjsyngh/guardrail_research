# System Profile: Arize AI / Phoenix

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-18

## 1. Overview

Arize Phoenix is an open-source (Apache 2.0) AI observability and evaluation framework that includes guardrail-like capabilities. Its Dataset Embeddings guard compares incoming prompts against known jailbreak embeddings — intercepted 86.4% of 656 jailbreak prompts in independent testing. Built on OpenTelemetry for broad framework compatibility (LangChain, LlamaIndex, OpenAI, Bedrock). Phoenix is the open-source core; Arize AX is the commercial hosted platform. Phoenix has no native transition from evaluation metrics to hard production guardrails.

**Vendor/Project:** [Arize AI](https://arize.com/) | [Phoenix GitHub](https://github.com/Arize-ai/phoenix) | [Arize AX Guardrails Docs](https://arize.com/docs/ax/security-and-settings/llm-security/guardrails)
**Type:** open-source (Phoenix / Apache 2.0) + commercial (Arize AX)

## 2. Deployment Models
- [x] Cloud-managed API (Arize AX platform)
- [x] On-premises / self-hosted (Phoenix standalone)
- [x] SDK / library (Python, Java)
- [ ] Agent / sidecar

## 3. Guardrail Locations
- [x] Pre-input (jailbreak detection via embedding comparison)
- [ ] Mid-pipeline
- [x] Post-input (evaluation metrics as guardrails)

Notes: Arize AX docs describe guardrails attachable to spans/traces. Phoenix alone has no native production guardrail enforcement — primarily an evaluation/observability tool.

## 4. Modality Support
- [x] Text
- [ ] Documents
- [ ] Images
- [ ] Audio
- [ ] Video

## 5. Constraint Types
- [x] ML classifier (embedding similarity for jailbreak detection)
- [x] LLM-as-judge (evaluation metrics — GPT-4 or similar judge model)
- [x] Rule-based
- [ ] RLHF safety
- [ ] Static analysis

## 6. Policy Language / Config Format

Format: Python SDK (OpenTelemetry-based instrumentation). Guardrails defined as evaluation metrics attached to spans.

Example (Python — auto-instrumentation):
```python
from phoenix.otel import register
tracer_provider = register(project_name="my-llm-app", endpoint="http://localhost:6006/v1/traces")
```

## 7. Extensibility

- OpenTelemetry-compatible — integrates with OpenAI, Bedrock, LangChain, LlamaIndex, and custom components without lock-in.
- Manual and auto-instrumentation toggles.
- Python and Java SDKs.
- Custom evaluation metrics definable.
- OTEL-based traces stream into Arize AX for hosted analysis.

## 8. Observability & Auditability

- Core strength: telemetry depth via OpenTelemetry.
- LangChain/LlamaIndex trace streaming into dashboards with root-cause analysis.
- Drift tracking across inputs, embeddings, and retrieval components.
- Arize AX adds hosted observability, alerting, and dashboard management.

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| Jailbreak interception rate | 86.4% (of 656 prompts) | futureagi.com 2025 comparison |
| LLM-as-judge latency | Multi-second (GPT-4 judge) | Braintrust/Galileo comparison |

Known limitations: No native hard guardrail enforcement in Phoenix — evaluation only. LLM-as-judge approach adds seconds of latency. Commercial Arize AX required for full production guardrails.

## 10. Pricing / Licensing

**License:** Apache 2.0 (Phoenix)
**Pricing model:** Phoenix is free and open source. Arize AX (commercial platform) has usage-based pricing; specific pricing not publicly listed.

## 11. Sources
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| arize_phoenix_github | Phoenix GitHub | https://github.com/Arize-ai/phoenix | 2026-03-18 |
| arize_ax_guardrails | Arize AX Guardrails docs | https://arize.com/docs/ax/security-and-settings/llm-security/guardrails | 2026-03-18 |
| arize_phoenix_overview | Arize Phoenix overview (Statsig) | https://www.statsig.com/perspectives/arize-phoenix-ai-observability | 2026-03-18 |
| futureagi_top5 | Top 5 AI Guardrail Tools 2025 | https://futureagi.com/blogs/top-5-ai-guardrailing-tools-2025 | 2026-03-18 |
