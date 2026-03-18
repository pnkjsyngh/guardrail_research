# System Profile: Guardrails AI

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-18

## 1. Overview

Guardrails AI is an open-source Python framework for validating and structuring LLM outputs through modular "validators." Validators are available via the Guardrails Hub (a marketplace of pre-built risk measures) and can be composed into Input and Output Guards. Policy schemas can be defined as RAIL specs (XML-like) or Pydantic v2 models. A commercial hub tier adds enterprise features.

**Vendor/Project:** [Guardrails AI](https://guardrailsai.com/) | [GitHub](https://github.com/guardrails-ai/guardrails) | [Docs](https://guardrailsai.com/docs)
**Type:** open-source (Apache 2.0) + commercial hub

## 2. Deployment Models
- [x] Cloud-managed API (Guardrails Hub / server mode)
- [x] On-premises / self-hosted
- [x] SDK / library (Python)
- [ ] Agent / sidecar

## 3. Enforcement Locations
- [x] Pre-input (input guards)
- [ ] Mid-pipeline
- [x] Post-output (output guards, re-asking loop)

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
