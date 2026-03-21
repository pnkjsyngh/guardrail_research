# System Profile: NVIDIA NeMo Guardrails

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-18

## 1. Overview

NVIDIA NeMo Guardrails is an open-source toolkit for adding programmable guardrails to LLM-based conversational and agentic applications. It supports topical rails, safety rails, security rails, RAG grounding, PII detection, jailbreak prevention, and multilingual/multimodal content safety via a custom DSL called Colang. Integrates with LangChain, LangGraph, and LlamaIndex.

**Vendor/Project:** [NeMo Guardrails GitHub](https://github.com/NVIDIA-NeMo/Guardrails) | [NVIDIA Developer Docs](https://developer.nvidia.com/nemo-guardrails)
**Type:** open-source

## 2. Deployment Models
- [ ] Cloud-managed API
- [x] On-premises / self-hosted
- [x] SDK / library (embedded in application)
- [x] Agent / sidecar (multi-agent support)

## 3. Guardrail Locations
- [x] Pre-input
- [x] Mid-pipeline (parallel rails execution)
- [x] Post-input

## 4. Modality Support
- [x] Text
- [ ] Documents
- [ ] Images (multimodal safety referenced but text primary)
- [ ] Audio
- [ ] Video

Notes: Documentation references "multilingual, multimodal content safety" but primary SDK support is text.

## 5. Constraint Types
- [x] Rule-based (Colang flows)
- [x] ML classifier (fine-tuned classifiers for PII, toxicity, jailbreak)
- [x] LLM-as-judge
- [ ] RLHF safety
- [ ] Static analysis

## 6. Policy Language / Config Format

Format: Colang DSL (`.co` files) + YAML config. Colang 1.0 (default) and Colang 2.0 (complete overhaul with parallel flows, async actions, Python-like syntax) are both supported.

Example (Colang 1.0):
```colang
define user ask politics
  "What do you think about the president?"
  "Who should I vote for?"

define bot refuse politics
  "I'm not able to discuss political topics."

define flow politics
  user ask politics
  bot refuse politics
```

## 7. Extensibility

- Integrates with LangChain, LangGraph, LlamaIndex out of the box.
- Custom actions can be written in Python and registered with the runtime.
- GPU acceleration available for low-latency deployments.
- Multi-agent deployment support.
- Community-contributed rail types via GitHub.

## 8. Observability & Auditability

- OpenTelemetry integration (migrated in recent releases) for standardized observability.
- Traces cover LLM calls, rail execution times, and token usage.
- Parallel rails execution with detailed tracing per rail.
- Plans to expand semantic conventions for Generative AI Systems in future releases.

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| Rails execution | Parallel (recent releases) | NVIDIA docs |
| Latency overhead | Not published (depends on rails configured) | — |

Known limitations: Latency scales with number of active rails and LLM-as-judge usage. No published benchmark numbers.

## 10. Pricing / Licensing

**License:** Apache 2.0
**Pricing model:** Free / open source

## 11. Sources
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| nvidia_nemo_github | NeMo Guardrails GitHub | https://github.com/NVIDIA-NeMo/Guardrails | 2026-03-18 |
| nvidia_nemo_docs | NeMo Guardrails Developer Docs | https://developer.nvidia.com/nemo-guardrails | 2026-03-18 |
| nvidia_nemo_colang | Colang Language Guide | https://docs.nvidia.com/nemo/guardrails/latest/configure-rails/colang/index.html | 2026-03-18 |
| nvidia_nemo_colang_syntax | Colang Syntax Guide | https://docs.nvidia.com/nemo/guardrails/latest/user-guides/colang-language-syntax-guide.html | 2026-03-18 |
| nvidia_nemo_full_docs | NeMo Guardrails Full Docs | https://docs.nvidia.com/nemo/guardrails/latest/index.html | 2026-03-18 |
