# Research Plan: LLM/AI Guardrail Systems

Goal: produce a rigorous, reproducible white paper surveying the state of the art in LLM/AI guardrail systems.

## Deliverables

- `research/` — collected vendor docs, configs, example workflows, and raw reference materials
- `paper/guardrails_whitepaper.md` — final white paper in Pandoc-friendly Markdown for `.docx` conversion
- `paper/references.bib` — compiled bibliography in BibTeX format

## Scope

**Vendor list:**

*Initial five (named in project brief):*
- NVIDIA NeMo Guardrails
- Amazon Bedrock Guardrails
- Lakera Guard
- Fiddler Guardrails
- Guardrails AI

*Additional systems (added after inventory search):*
- Microsoft Azure AI Content Safety + Prompt Shields (cloud, text + image, no PII detection)
- Meta LlamaFirewall / Llama Guard (open source; PromptGuard 2, AlignmentCheck, CodeShield)
- IBM Granite Guardian (open model-based; RAG-specific risk detection)
- Enkrypt AI (commercial; text + image + voice, bias detection)
- Protect AI / LLM Guard (open source library; input + output scanners)
- OpenGuardrails (open source; context-aware, 119-language support)
- Galileo Guardrails (commercial; evaluation + real-time guardrailing SDK)
- Arize AI / Phoenix (open source observability + guardrails; jailbreak detection)
- CalypsoAI (commercial enterprise; runtime protection + red-teaming)

**Focus areas:** guardrail points (input, output, in-pipeline), modality (text, documents, images, audio, video), policy languages, hard vs. soft constraints, extensibility, observability/auditability, latency/throughput, security model, integration complexity, licensing.

## Research Methodology

### Step 1 — Inventory & source collection
- For each vendor, collect official docs, SDKs, example configs, API references, and public research or blog posts.
- Save raw copies (PDF/HTML) under `research/sources/<vendor>/raw/` with filename and retrieval date.
- Record retrieval citations in `research/sources/<vendor>/README.md` (exact URLs and access dates). Do **not** commit secrets; keep any credential notes in a local `secrets.md` covered by `.gitignore`.

### Step 2 — Fill System Profile Template for each vendor
Use `research/templates/system_profile.md`. Fields to complete:
- Short description & vendor link
- Deployment models (cloud, on-prem, SDK, agent)
- Guardrail locations (pre-input, post-input)
- Constraint types (rule-based, ML-based, RLHF-safety, classifiers)
- Policy language or config format (examples and schema)
- Extensibility (custom rules, plugins, third-party model integration)
- Observability: logs, audit trails, metrics exposed
- Performance characteristics (documented) and known limitations
- Pricing/licensing notes

### Step 3 — Evaluation criteria
Cite vendor-published metrics where available; otherwise qualitative only.
- Correctness (true positive rate on policy violations)
- False positive rate and impact on legitimate outputs
- Latency overhead (ms/request)
- Throughput (req/sec guardrail on vs. off)
- Robustness to adversarial prompts (from public writeups and vendor advisories)
- Explainability & auditability
- Integration complexity (setup time, lines of example code, dependency count)
- Maintenance cost (retraining, rule authoring burden)

### Step 4 — Documentation collection
- Save representative example configs, screenshots, and SDK snippets under `research/sources/<vendor>/`.
- Rely on vendor examples, official docs, and publicly available SDK snippets. All assessments are **documentation-only**.

### Step 5 — Analysis and synthesis
- Quantitative comparisons only when vendors publish numeric metrics; cite source and measurement conditions.
- Qualitative analysis: rule-authoring experience, developer ergonomics, observability, vendor support.
- Security & governance analysis: failure modes, audit models, regulatory relevance.
- Collect adversarial bypass examples from public writeups; document the transformation used and observed behavior.

### Step 6 — Write the white paper
Follow `paper/STRUCTURE.md`: Executive Summary, Background, Methods, System Profiles, Findings, Comparative Analysis, Recommendations, Limitations, Appendix (configs, raw outputs with links to `research/sources/`).

## Reproducibility Checklist
Before finalizing any claim:
- All source artifacts are present under `research/sources/<vendor>/raw/` and `research/notes/`.
- Every figure and table in the paper has an exact citation linking to a file in `research/sources/`.

## Accelerated 3-Day Schedule

### Day 1 — Inventory & templates
- Create full repo scaffolding (`research/`, `research/templates/`, `research/sources/`, `research/notes/`, `paper/figures/`).
- Implement `research/templates/system_profile.md` and `paper/STRUCTURE.md`.
- For each of the five initial vendors: download at least one primary doc into `research/sources/<vendor>/raw/` and create a `research/notes/<vendor>.md` skeleton with metadata and source links.

### Day 2 — Documentation collection & profiling
- Complete collection of official docs, SDK snippets, example configs for the initial vendor list.
- Fill System Profile Templates; add citations to `paper/references.bib`.
- Note access or licensing constraints; mark documentation-only assessments explicitly.

### Day 3 — Synthesis & draft
- Draft `paper/guardrails_whitepaper.md` covering all required sections with links to `research/sources/`.
- Finalize `paper/references.bib`.
- Produce a concise status report summarizing gaps and recommended next steps.

**Scope:** documentation-only research throughout. No live API integrations.

## Authorship & Citations
- Maintain `paper/references.bib` in BibTeX format.
- Add a citation entry for every vendor doc, blog post, and paper referenced.
- Use Pandoc citeproc style in-text (`[@vendor2024]`).
