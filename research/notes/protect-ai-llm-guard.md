# System Profile: Protect AI / LLM Guard

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-18

## 1. Overview

LLM Guard is an open-source (MIT) security toolkit by Protect AI providing modular input and output scanners. Input scanners cover prompt injection, PII anonymization, and secret redaction. Output scanners cover content moderation, toxicity, bias, and relevance checking. Engineered for CPU inference (5× lower cost vs. GPU). Over 2.5 million downloads. Can run as a Python library or standalone API server.

**Vendor/Project:** [LLM Guard (Protect AI)](https://protectai.com/llm-guard) | [GitHub](https://github.com/protectai/llm-guard)
**Type:** open-source (MIT) + Protect AI commercial platform

## 2. Deployment Models
- [ ] Cloud-managed API
- [x] On-premises / self-hosted
- [x] SDK / library (Python)
- [ ] Agent / sidecar

Notes: Can also be deployed as a standalone REST API server.

## 3. Guardrail Locations
- [x] Pre-input (input scanners)
- [ ] Mid-pipeline
- [x] Post-input (output scanners)

## 4. Modality Support
- [x] Text
- [ ] Documents
- [ ] Images
- [ ] Audio
- [ ] Video

## 5. Constraint Types
- [x] ML classifier (fine-tuned models per scanner)
- [x] Rule-based (regex, pattern matching)
- [ ] LLM-as-judge
- [ ] RLHF safety
- [ ] Static analysis

## 6. Policy Language / Config Format

Format: Python — instantiate and chain scanner objects into an input/output guard.

Example (Python):
```python
from llm_guard.input_scanners import PromptInjection, Anonymize
from llm_guard.output_scanners import Toxicity, Relevance
from llm_guard import scan_prompt, scan_output

input_scanners = [Anonymize(), PromptInjection()]
output_scanners = [Toxicity(threshold=0.5), Relevance()]

sanitized_prompt, results_valid, results_score = scan_prompt(
    input_scanners, prompt
)
sanitized_response, results_valid, results_score = scan_output(
    output_scanners, prompt, response
)
```

## 7. Extensibility

- Permissively licensed (MIT) — fully forkable and customizable.
- Custom scanners can be implemented by following the scanner interface.
- Integrates within minutes as a library or API.
- Extensive documentation including use-case playbooks.

## 8. Observability & Auditability

- Per-scanner results and confidence scores returned per call.
- No native dashboard; integration with application-level logging/tracing required.
- Protect AI platform (commercial) adds broader MLSecOps observability.

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| CPU vs GPU inference cost | 5× lower on CPU | Protect AI / LLM Guard site |
| Downloads | >2.5 million | Protect AI / LLM Guard site |

Known limitations: No native dashboard. Text only. Latency depends on number and type of scanners chained.

## 10. Pricing / Licensing

**License:** MIT (LLM Guard core)
**Pricing model:** Free / open source. Protect AI platform (commercial MLSecOps) has separate pricing.

## 11. Sources
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| llmguard_site | LLM Guard homepage | https://protectai.com/llm-guard | 2026-03-18 |
| llmguard_github | LLM Guard GitHub | https://github.com/protectai/llm-guard | 2026-03-18 |
