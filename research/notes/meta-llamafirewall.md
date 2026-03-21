# System Profile: Meta LlamaFirewall / Llama Guard

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-18

## 1. Overview

LlamaFirewall is Meta's open-source guardrail framework targeting AI agent security. It is designed as a final-layer defense against prompt injection, agent misalignment, and insecure code generation. Three components work in concert: PromptGuard 2 (jailbreak/injection detector), AlignmentCheck (chain-of-thought auditor — first open-source real-time CoT auditor), and CodeShield (static code analysis). Used in production at Meta. >90% reduction in attack success rate on AgentDojo benchmark.

**Vendor/Project:** [LlamaFirewall GitHub](https://github.com/meta-llama/PurpleLlama/tree/main/LlamaFirewall) | [Meta AI Research](https://ai.meta.com/research/publications/llamafirewall-an-open-source-guardrail-system-for-building-secure-ai-agents/) | [Docs](https://meta-llama.github.io/PurpleLlama/LlamaFirewall/)
**Type:** open-source

## 2. Deployment Models
- [ ] Cloud-managed API
- [x] On-premises / self-hosted
- [x] SDK / library
- [x] Agent / sidecar (designed for agentic pipelines)

## 3. Guardrail Locations
- [x] Pre-input (PromptGuard 2 — jailbreak and injection on user prompt)
- [x] Mid-pipeline (AlignmentCheck — audits agent chain-of-thought in real time)
- [x] Post-input (CodeShield — insecure code detection)

## 4. Modality Support
- [x] Text
- [ ] Documents
- [ ] Images
- [ ] Audio
- [ ] Video

## 5. Constraint Types
- [x] ML classifier (PromptGuard 2 — fine-tuned BERT-style, 22M and 86M variants)
- [x] LLM-as-judge (AlignmentCheck — CoT reasoning audit)
- [x] Static analysis (CodeShield — online static analysis engine)
- [ ] Rule-based
- [ ] RLHF safety

## 6. Policy Language / Config Format

Format: Python SDK; scanners instantiated and composed as a pipeline. Configurable via scanner selection and threshold parameters.

Example (Python):
```python
from llamafirewall import LlamaFirewall, ScannerType, UserMessage

fw = LlamaFirewall()
result = fw.scan(UserMessage(content="Ignore previous instructions..."))
if result.is_safe:
    # proceed
    pass
```

## 7. Extensibility

- Open source — teams can build, audit, and extend scanners as threats evolve.
- Regex filters are customizable.
- Additional scanner types can be contributed or added.
- Integrates with Llama Stack for full pipeline deployment (Red Hat developer guide).
- Designed for transparency and community collaboration.

## 8. Observability & Auditability

- Real-time chain-of-thought auditing via AlignmentCheck catches goal hijacking before actions are taken.
- Per-scanner result returned; audit trail available at application layer.
- No native dashboard; observability relies on application-level logging.

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| PromptGuard 2 (22M) latency vs 86M | ~75% reduction | arXiv 2505.03574 |
| CodeShield precision | 96% | arXiv 2505.03574 |
| CodeShield recall | 79% | arXiv 2505.03574 |
| AgentDojo attack success rate reduction | >90% | arXiv 2505.03574 |
| Computational overhead | "Negligible" (vendor claim) | Meta AI blog |

Known limitations: Text only. No native observability dashboard. Llama community license — check commercial use terms.

## 10. Pricing / Licensing

**License:** Meta Llama community license (review for commercial use; different from Apache 2.0)
**Pricing model:** Free / open source

## 11. Sources
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| meta_llamafirewall_arxiv | LlamaFirewall arXiv paper (2505.03574) | https://arxiv.org/abs/2505.03574 | 2026-03-18 |
| meta_llamafirewall_docs | LlamaFirewall official docs | https://meta-llama.github.io/PurpleLlama/LlamaFirewall/ | 2026-03-18 |
| meta_llamafirewall_github | LlamaFirewall GitHub | https://github.com/meta-llama/PurpleLlama/tree/main/LlamaFirewall | 2026-03-18 |
| meta_llamafirewall_blog | Meta AI blog — LlamaFirewall | https://ai.meta.com/blog/ai-defenders-program-llama-protection-tools/ | 2026-03-18 |
| meta_llama_protections | Llama Protections page | https://www.llama.com/llama-protections/ | 2026-03-18 |
| meta_llamafirewall_infoq | InfoQ — LlamaFirewall launch | https://www.infoq.com/news/2025/05/llamafirewall-agent-protection/ | 2026-03-18 |
