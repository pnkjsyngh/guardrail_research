# System Profile: IBM Granite Guardian

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-18

## 1. Overview

IBM Granite Guardian is a collection of open models (Apache 2.0) from IBM Research for detecting risks in prompts and responses. Unique capability: RAG-specific risk detection (context relevance, groundedness, answer relevance). Also covers standard harm categories. Granite Guardian 3.1 adds function-calling hallucination detection for agent tool calls. Available on Hugging Face, IBM watsonx.ai, Ollama, Docker, and NVIDIA NIM.

**Vendor/Project:** [IBM Granite Guardian Docs](https://www.ibm.com/granite/docs/models/guardian) | [GitHub](https://github.com/ibm-granite/granite-guardian) | [Hugging Face](https://huggingface.co/ibm-granite/granite-guardian-3.0-8b)
**Type:** open-source (IBM)

## 2. Deployment Models
- [x] Cloud-managed API (IBM watsonx.ai, NVIDIA NIM)
- [x] On-premises / self-hosted (Ollama, Docker, LM Studio)
- [x] SDK / library (Hugging Face transformers)
- [ ] Agent / sidecar

## 3. Enforcement Locations
- [x] Pre-input
- [ ] Mid-pipeline
- [x] Post-output

Notes: RAG-specific checks (groundedness, relevance) are inherently post-retrieval / post-output.

## 4. Modality Support
- [x] Text
- [ ] Documents
- [ ] Images
- [ ] Audio
- [ ] Video

## 5. Constraint Types
- [x] ML classifier (fine-tuned Granite model)
- [x] LLM-as-judge (model outputs risk assessment)
- [ ] Rule-based
- [ ] RLHF safety
- [ ] Static analysis

## 6. Policy Language / Config Format

Format: Hugging Face model inference (transformers / `pipeline`) with prompt templates. Bring Your Own Criteria (BYOC) allows custom risk definitions passed as natural language in the prompt.

Example (Python — Hugging Face):
```python
from transformers import pipeline
guardian = pipeline("text-classification",
                    model="ibm-granite/granite-guardian-3.0-8b")
result = guardian("Is this response grounded in the provided context? ...")
```

## 7. Extensibility

- BYOC (Bring Your Own Criteria): custom risk criteria defined as natural language — no fine-tuning required.
- LoRA adapters available for more specific downstream tasks layered on top of base Guardian models.
- Integrates into any Hugging Face-compatible pipeline.

## 8. Observability & Auditability

- Designed for model risk assessment, observability, and monitoring use cases (per IBM docs).
- Granite Guardian 3.1: function calling hallucination detection provides observability into agent tool calls.
- No native dashboard; observability handled at the application/platform layer (e.g., watsonx.ai).

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| Recall | Strong vs. peers | Enkrypt AI comparative study |
| Precision | Slightly lower than top peers | Enkrypt AI comparative study |
| Latency | Higher than some peers (8B parameter model) | Enkrypt AI comparative study |

Known limitations: Higher latency vs. lighter-weight classifiers due to model size. No native dashboard.

## 10. Pricing / Licensing

**License:** Apache 2.0 (Granite models and Guardian models)
**Pricing model:** Free / open source for model weights. watsonx.ai cloud hosting has usage-based pricing.

## 11. Sources
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| ibm_granite_guardian_docs | IBM Granite Guardian Docs | https://www.ibm.com/granite/docs/models/guardian | 2026-03-18 |
| ibm_granite_guardian_github | Granite Guardian GitHub | https://github.com/ibm-granite/granite-guardian | 2026-03-18 |
| ibm_granite_guardian_hf | Granite Guardian 3.0 8B on Hugging Face | https://huggingface.co/ibm-granite/granite-guardian-3.0-8b | 2026-03-18 |
| ibm_granite_guardian_think | What is Granite Guardian (IBM Think) | https://www.ibm.com/think/topics/granite-guardian | 2026-03-18 |
| ibm_granite_guardian_nim | Granite Guardian on NVIDIA NIM | https://build.nvidia.com/ibm/granite-guardian-3_0-8b/modelcard | 2026-03-18 |
| enkrypt_comparative | Enkrypt AI comparative study of LLM guardrails | https://www.enkryptai.com/blog/ensuring-ai-safety-and-compliance-comparative-study-of-llm-guardrails | 2026-03-18 |
