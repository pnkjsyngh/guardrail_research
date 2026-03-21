# System Profile: Galileo Guardrails

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-18

## 1. Overview

Galileo is an AI reliability platform for multi-agent systems combining observability, evaluation, and real-time guardrails. Its guardrail SDK screens prompts and completions using Luna-2 small language models, providing alerts for prompt injection, PII leakage, and hallucinations at sub-200ms latency. The Insights Engine performs automatic root cause analysis. The Agent Reliability Platform tier is available in Galileo's free plan.

**Vendor/Project:** [Galileo](https://galileo.ai/) | [Guardrail Metrics Docs](https://docs.galileo.ai/galileo/gen-ai-studio-products/galileo-guardrail-metrics)
**Type:** commercial (SaaS)

## 2. Deployment Models
- [x] Cloud-managed API
- [ ] On-premises / self-hosted
- [x] SDK / library (Python + TypeScript)
- [ ] Agent / sidecar

## 3. Guardrail Locations
- [x] Pre-input
- [x] Mid-pipeline (agentic observability — multi-step flow adherence)
- [x] Post-input

## 4. Modality Support
- [x] Text
- [ ] Documents
- [ ] Images
- [ ] Audio
- [ ] Video

## 5. Constraint Types
- [x] ML classifier (Luna-2 small language models)
- [x] LLM-as-judge (configurable)
- [x] Rule-based (custom metrics via code)
- [ ] RLHF safety
- [ ] Static analysis

## 6. Policy Language / Config Format

Format: Python and TypeScript SDKs. Custom metrics defined as code-based functions, LLM-as-a-judge prompts, or Luna-2 model evaluators.

## 7. Extensibility

- Custom metrics: code-based, LLM-as-a-judge, or Luna-2 evaluator.
- SDKs for Python and TypeScript.
- Insights Engine adds automatic failure mode identification and root cause analysis.
- Purpose-built metrics: flow adherence, task completion, conversation quality, agent efficiency.

## 8. Observability & Auditability

- Multi-agent agentic observability: traces, spans across agent steps.
- Insights Engine: automatic failure mode identification and actionable root cause analysis.
- Real-time alerting for guardrail violations.
- Dashboard covering agent efficiency, task completion, conversation quality.

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| Guardrail latency (Luna-2) | Sub-200ms | Galileo docs |

Known limitations: Cloud-only. No on-premises option. Pricing scales with trace volume.

## 10. Pricing / Licensing

**License:** Proprietary
**Pricing model:**
- Free tier: 5,000 traces/month, Agent Reliability Platform included.
- Pro: $100/month, 50,000 traces.
- Compute: ~$0.02 per million tokens.
- Enterprise: custom.

## 11. Sources
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| galileo_site | Galileo homepage | https://galileo.ai/ | 2026-03-18 |
| galileo_pricing | Galileo pricing | https://galileo.ai/pricing | 2026-03-18 |
| galileo_guardrail_metrics | Guardrail metrics docs | https://docs.galileo.ai/galileo/gen-ai-studio-products/galileo-guardrail-metrics | 2026-03-18 |
| galileo_agent_platform | Galileo Agent Reliability Platform announcement | https://www.prnewswire.com/news-releases/galileo-announces-free-agent-reliability-platform-302508172.html | 2026-03-18 |
