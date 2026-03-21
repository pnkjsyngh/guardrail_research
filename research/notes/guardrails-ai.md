# System Profile: Guardrails AI

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-20

## 1. Overview

Guardrails AI is an open-source Python framework for validating and structuring LLM outputs through modular "validators." Validators are available via the Guardrails Hub (a marketplace of pre-built risk measures) and can be composed into Input and Output Guards. Policy schemas can be defined as RAIL specs (XML-like) or Pydantic v2 models. A commercial hub tier adds enterprise features.

**Vendor/Project:** [Guardrails AI](https://guardrailsai.com/) | [GitHub](https://github.com/guardrails-ai/guardrails) | [Docs](https://guardrailsai.com/docs)
**Type:** open-source (Apache 2.0) + commercial hub

## 2. Deployment Models
- [x] Cloud-managed API (Guardrails Hub / server mode)
- [x] On-premises / self-hosted
- [x] SDK / library (Python)
- [ ] Agent / sidecar

## 3. Guardrail Locations
- [x] Pre-input (input guards)
- [ ] Mid-pipeline
- [x] Post-input (output guards, re-asking loop)

## 4. Modality Support
- [x] Text
- [ ] Documents
- [ ] Images
- [ ] Audio
- [ ] Video

## 5. Constraint Types
- [x] Rule-based (RAIL validators, regex, custom functions)
- [x] ML classifier (Hub validators using fine-tuned models)
- [x] LLM-as-judge (re-asking and self-correction flows)
- [ ] RLHF safety
- [ ] Static analysis

## 6. Policy Language / Config Format

Format: Python (Pydantic v2 BaseModel or RAIL XML-like spec). Validators are composed into Guard objects.

Example (Pydantic):
```python
from guardrails import Guard
from guardrails.hub import DetectPII, ToxicLanguage
from pydantic import BaseModel

class OutputSchema(BaseModel):
    response: str

guard = Guard.for_pydantic(OutputSchema).use_many(
    DetectPII(pii_entities=["EMAIL_ADDRESS"]),
    ToxicLanguage(threshold=0.5)
)
validated = guard(llm_output)
```

## 7. Extensibility

- Hub ecosystem of pre-built validators installable via `guardrails hub install`.
- Custom validators: function-based or class-based implementations.
- Streaming validation for incremental output checking.
- Pydantic v2 full compatibility (type system, decorators, field definitions).
- OpenTelemetry integration via `pydantic-ai-guardrails` for tracing.

## 7a. Hub Validator Taxonomy (68 validators total)

Validators fall into four tiers based on their runtime dependencies. This is critical for local/offline deployments.

### Tier 1 — Fully offline (pure logic, no model)
Run as pure Python immediately after `pip install`. No network required post-installation.

| Validator | What it does |
|---|---|
| `ban_list` | Fuzzy-matches against a user-defined word/phrase blocklist |
| `regex_match` | Validates against a regex pattern |
| `secrets_present` | Detects API keys, tokens, credentials via pattern matching |
| `valid_json` | Checks output is valid JSON |
| `valid_sql` | Checks output is valid SQL |
| `valid_python` | Checks output is valid Python |
| `competitor_check` | Flags competitor names from a rule-based list |
| `valid_url` | Validates URLs in output |
| `reading_time` | Enforces output length / reading time limits |

### Tier 2 — Offline after first download (specialized fine-tuned models)
Use purpose-built, task-specific models (not general LLMs). Models are downloaded from Hugging Face on first use and cached locally. After caching, set `use_local=True` and they run fully offline. For air-gapped environments: pre-cache models, then set `HF_HUB_OFFLINE=1`.

| Validator | Model powering it | What it does |
|---|---|---|
| `toxic_language` | Detoxify `toxic-bert` (HF) | Detects toxicity, threats, insults, hate across 7 categories |
| `detect_pii` | Microsoft Presidio (local NER) | Detects and anonymizes PII — runs entirely locally via `presidio-analyzer` + `presidio-anonymizer` |
| `bias_check` | HF classifier | Detects bias by age, gender, ethnicity, religion |
| `sensitive_topics` | HF classifier | Detects sensitive topic categories |
| `arize_dataset_embeddings` | Embedding model (HF) | Compares prompt against known jailbreak embedding dataset |

Key point: these validators use **specialized fine-tuned models**, not general-purpose LLMs. No OpenAI key or similar required.

### Tier 3 — Self-hosted endpoint (your infrastructure, your GPU)
Any Tier 2 validator can be pointed at a self-hosted FastAPI/Flask server running the model. This moves inference off-CPU onto your own GPU hardware without any external calls.

```python
from guardrails.hub import ToxicLanguage

guard = Guard().use(
    ToxicLanguage(validation_endpoint="http://localhost:8000/validate")
)
```

Your endpoint must implement the `_inference_remote` interface documented by Guardrails AI.

### Tier 4 — Requires external LLM API (not suitable for local-only)
These validators delegate judgment to whichever LLM you configure via LiteLLM. They default to `gpt-3.5-turbo` but accept any LiteLLM-compatible model string — including a locally hosted Ollama model.

| Validator | Default model | Notes |
|---|---|---|
| `unusual_prompt` | `gpt-3.5-turbo` (configurable) | Jailbreak / psychological prompt detection via LLM judgment |
| `provenance_llm` | Configurable | Factuality checking via LLM judge |
| `llm_critic` | Configurable | Grade response against criteria via LLM |
| `grounded_ai_hallucination` | Grounded AI API | Hallucination detection — external service, not configurable locally |
| `llm_rag_evaluator` | Arize + LLM API | RAG relevance — external service |

**Important:** For Tier 4 validators that use LiteLLM (`unusual_prompt`, `provenance_llm`, `llm_critic`), you can point them at a local Ollama model and they become fully local:
```python
UnusualPrompt(llm_callable="ollama/llama3")
```
Validators using proprietary external APIs (`grounded_ai_hallucination`, `llm_rag_evaluator`) cannot be made local.

### Hub installation requires internet (setup-time only)
`guardrails hub install hub://guardrails/<validator>` requires a Guardrails Hub API key at install time. Once installed and models cached, all Tier 1–3 validators run fully offline. Recommended workflow for air-gapped deployments: install and cache on a connected machine, transfer the environment.

## 8. Observability & Auditability

- OpenTelemetry / Logfire integration via `pydantic-ai-guardrails`.
- Guardrails Index (launched Feb 2025): benchmark of 24 guardrails across 6 categories including latency metrics.
- Guard call history accessible programmatically.

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| Typical latency budget | 200–300ms | Guardrails AI docs |
| Guardrails Index | 24 guardrails benchmarked across 6 categories | guardrailsai.com (Feb 2025) |

Known limitations: Re-asking loops add significant latency when LLM correction is triggered. Text only.

## 10. Pricing / Licensing

**License:** Apache 2.0 (core library)
**Pricing model:** Open source core free; Guardrails Hub / enterprise tier pricing not publicly listed — contact vendor.

## 11. Sources
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| guardrailsai_site | Guardrails AI homepage | https://guardrailsai.com/ | 2026-03-18 |
| guardrailsai_github | Guardrails AI GitHub | https://github.com/guardrails-ai/guardrails | 2026-03-18 |
| guardrailsai_docs | Guardrails AI documentation | https://guardrailsai.com/docs | 2026-03-18 |
| guardrailsai_pypi | guardrails-ai PyPI | https://pypi.org/project/guardrails-ai/ | 2026-03-18 |
