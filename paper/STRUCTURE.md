# White Paper Structure: LLM/AI Guardrail Systems

This file defines the canonical outline for `guardrails_whitepaper.md`. Each section heading and required subsection is listed with a brief prompt describing the expected content. Do not skip sections; mark incomplete sections with `<!-- TODO -->`.

---

## 1. Executive Summary
<!-- 1–2 pages. State the research question, the systems surveyed, key findings, and top recommendations. Written last. -->

## 2. Background
### 2.1 What Are LLM Guardrails?
<!-- Define guardrails, distinguish from alignment training and RLHF. Explain why they are needed at inference time. -->

### 2.2 Threat Model
<!-- Describe the categories of harm guardrails defend against: prompt injection, jailbreaks, harmful content generation, PII leakage, insecure code output, hallucination, etc. -->

### 2.3 Enforcement Architecture Patterns
<!-- Explain the three main enforcement points: pre-input, mid-pipeline, post-output. Include a simple diagram or ASCII figure referencing `figures/enforcement_architecture.png`. -->

## 3. Methods
### 3.1 Vendor Selection Criteria
<!-- Explain how vendors were selected (initial list + discovery search). -->

### 3.2 Data Collection Process
<!-- Describe the documentation-only methodology: what was collected, where it is stored, and how gaps were handled. -->

### 3.3 Evaluation Dimensions
<!-- List and define the dimensions used across all system profiles (enforcement location, modality, policy language, extensibility, observability, performance, licensing). -->

## 4. System Profiles
<!-- One subsection per vendor. Each subsection summarizes the filled system profile from `research/notes/<vendor>.md`. Keep each profile to ~1 page in the paper; full details remain in the notes. -->

### 4.1 NVIDIA NeMo Guardrails
### 4.2 Amazon Bedrock Guardrails
### 4.3 Lakera Guard
### 4.4 Fiddler Guardrails
### 4.5 Guardrails AI
### 4.6 Microsoft Azure AI Content Safety + Prompt Shields
### 4.7 Meta LlamaFirewall / Llama Guard
### 4.8 IBM Granite Guardian
### 4.9 Enkrypt AI
### 4.10 Protect AI / LLM Guard
### 4.11 OpenGuardrails
### 4.12 Galileo Guardrails
### 4.13 Arize AI / Phoenix
### 4.14 CalypsoAI

## 5. Findings
### 5.1 Coverage by Enforcement Location
<!-- Table: which systems enforce pre-input / mid-pipeline / post-output. -->
<!-- Figure placeholder: `figures/coverage_enforcement.png` -->

### 5.2 Modality Support Comparison
<!-- Table: text / image / audio / video / documents per system. -->

### 5.3 Policy Authoring Experience
<!-- Qualitative comparison: rule-based DSLs vs. config files vs. code-first. -->

### 5.4 Extensibility and Integration Complexity
<!-- Qualitative: ease of adding custom rules, dependency count, integration time. -->

### 5.5 Observability and Auditability
<!-- Which systems expose logs, metrics, audit trails; relevance to compliance. -->

### 5.6 Performance Characteristics
<!-- Summarize vendor-published latency and throughput data. Note gaps where vendors have not published metrics. -->
<!-- Table placeholder: `figures/performance_table.md` -->

### 5.7 Security and Robustness
<!-- Known bypass techniques, adversarial robustness results from public writeups and vendor advisories. -->

### 5.8 Licensing and Deployment Constraints
<!-- Open source vs. commercial; on-prem vs. cloud-only; notable restrictions. -->

## 6. Comparative Analysis
<!-- Synthesize findings across all dimensions into a unified comparison. Include a summary matrix table. -->
<!-- Table placeholder: `figures/comparison_matrix.md` -->

## 7. Recommendations
### 7.1 For Teams Building on a Single Cloud Provider
### 7.2 For Teams Requiring On-Premises or Air-Gapped Deployment
### 7.3 For Agent / Multi-Step Pipeline Use Cases
### 7.4 For Compliance-Heavy Domains (Finance, Healthcare, Legal)
### 7.5 For Low-Latency / High-Throughput Applications

## 8. Limitations
<!-- Scope limitations: documentation-only assessment, no live benchmarking, vendor list not exhaustive, documentation may be outdated. -->

## 9. Conclusion
<!-- 1–2 paragraphs summarizing the state of the field and the most important gaps or open problems. -->

## References
<!-- Populated from `references.bib` via Pandoc citeproc. Do not hand-edit this section in the final document. -->

## Appendix
### Appendix A: System Profile Index
<!-- Table linking each vendor to its full profile in `research/notes/` and raw sources in `research/sources/`. -->

### Appendix B: Example Policy Configurations
<!-- Representative config snippets for each system; fenced code blocks with language tags. -->

### Appendix C: Adversarial Bypass Examples
<!-- Documented bypass techniques from public writeups; transformation used, observed behavior, and source citation. -->
