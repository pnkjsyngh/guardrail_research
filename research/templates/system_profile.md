# System Profile: [Vendor / System Name]

> **Assessment type:** documentation-only
> **Profile status:** skeleton | in-progress | complete
> **Last updated:** YYYY-MM-DD

## 1. Overview
<!-- Short description (2–4 sentences) and vendor/project link -->

**Vendor/Project:** [Name](URL)
**Type:** open-source | commercial | cloud-managed | hybrid

## 2. Deployment Models
<!-- How is the system deployed? Check all that apply and add notes. -->
- [ ] Cloud-managed API
- [ ] On-premises / self-hosted
- [ ] SDK / library (embedded in application)
- [ ] Agent / sidecar

Notes:

## 3. Guardrail Locations
<!-- Where in the request/response pipeline does guardrail happen? -->
- [ ] Pre-input (before prompt reaches the model)
- [ ] Post-input (before response is returned to user)

Notes:

## 4. Modality Support
- [ ] Text
- [ ] Documents (PDF, etc.)
- [ ] Images
- [ ] Audio
- [ ] Video

Notes:

## 5. Constraint Types
<!-- What guardrail mechanisms are available? -->
- [ ] Rule-based (regex, keyword lists, templates)
- [ ] ML classifier (fine-tuned model)
- [ ] LLM-as-judge
- [ ] RLHF safety / aligned model
- [ ] Static analysis (e.g., code scanning)

Notes:

## 6. Policy Language / Config Format
<!-- How are policies authored? Include a minimal example if available. -->

Format:

Example:
```
# paste minimal example config here
```

## 7. Extensibility
<!-- Can users add custom rules, plugins, or third-party models? -->

## 8. Observability & Auditability
<!-- Logs, audit trails, metrics, dashboards exposed -->

## 9. Performance Characteristics
<!-- Vendor-published latency, throughput numbers. If none published, note that. -->

| Metric | Value | Source |
|--------|-------|--------|
| Latency overhead | | |
| Throughput | | |

Known limitations:

## 10. Pricing / Licensing
<!-- License (MIT, Apache, proprietary, etc.), pricing model if commercial -->

**License:**
**Pricing model:**
Notes:

## 11. Sources
<!-- List every source used in this profile with citation key -->
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| | | | |
