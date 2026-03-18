# System Profile: Amazon Bedrock Guardrails

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-18

## 1. Overview

Amazon Bedrock Guardrails is a managed guardrail service built into AWS Bedrock providing content filters, denied topics, PII redaction, word filters, image content filters, and prompt attack detection across all models hosted on Bedrock. The `ApplyGuardrail` API extends coverage to self-hosted or third-party models (OpenAI, Gemini). Policies are evaluated in parallel for reduced latency.

**Vendor/Project:** [Amazon Bedrock Guardrails](https://aws.amazon.com/bedrock/guardrails/)
**Type:** cloud-managed

## 2. Deployment Models
- [x] Cloud-managed API
- [ ] On-premises / self-hosted
- [ ] SDK / library
- [ ] Agent / sidecar

Notes: `ApplyGuardrail` API allows use with models outside Bedrock (self-hosted, OpenAI, Gemini).

## 3. Enforcement Locations
- [x] Pre-input (prompt attack detection)
- [ ] Mid-pipeline
- [x] Post-output (content filters, PII redaction)

## 4. Modality Support
- [x] Text
- [ ] Documents
- [x] Images (added March 2025)
- [ ] Audio
- [ ] Video

## 5. Constraint Types
- [x] Rule-based (denied topics, word filters, blocklists)
- [x] ML classifier (content filters, PII detection, prompt attack detection)
- [ ] LLM-as-judge
- [ ] RLHF safety
- [ ] Static analysis

## 6. Policy Language / Config Format

Format: AWS Console / CloudFormation / SDK parameters. A guardrail is a versioned combination of one or more policies. Working drafts allow iterative testing before version promotion.

Example (AWS SDK Python — boto3):
```python
bedrock = boto3.client('bedrock')
bedrock.create_guardrail(
    name='my-guardrail',
    contentPolicyConfig={
        'filtersConfig': [
            {'type': 'SEXUAL', 'inputStrength': 'HIGH', 'outputStrength': 'HIGH'},
            {'type': 'HATE_SPEECH', 'inputStrength': 'MEDIUM', 'outputStrength': 'MEDIUM'},
        ]
    },
    sensitiveInformationPolicyConfig={
        'piiEntitiesConfig': [{'type': 'EMAIL', 'action': 'ANONYMIZE'}]
    }
)
```

## 7. Extensibility

- `ApplyGuardrail` API extends to non-Bedrock models.
- Compatible with Strands Agents and Amazon Bedrock AgentCore.
- Custom word filters and blocklists configurable.
- Multiple policies can be combined in a single guardrail.

## 8. Observability & Auditability

- Native CloudWatch integration: namespace `AWS/Bedrock/Guardrails`.
- Metrics: invocation count, invocation latency, text unit utilization, policy types associated with interventions.
- Built-in test window in AWS Console for iterative configuration.
- Elastic integration available for LLM observability dashboards.

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| Policy evaluation | Parallel (all configured policies evaluated simultaneously) | AWS docs |
| Pricing unit | Per 1,000 text units processed | AWS pricing page |
| Content filter cost | $0.15 / 1,000 text units | AWS pricing page (as of Apr 2025) |
| Sensitive info filter | Free for some types | AWS pricing page |

Known limitations: Cloud-only; no on-premises option. No mid-pipeline enforcement.

## 10. Pricing / Licensing

**License:** Proprietary (AWS managed service)
**Pricing model:** Per-request, per-policy-type; billed monthly. See [AWS Bedrock Pricing](https://aws.amazon.com/bedrock/pricing/).

## 11. Sources
| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| aws_bedrock_guardrails | Amazon Bedrock Guardrails homepage | https://aws.amazon.com/bedrock/guardrails/ | 2026-03-18 |
| aws_bedrock_how | How Amazon Bedrock Guardrails works | https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails-how.html | 2026-03-18 |
| aws_bedrock_cw | Monitor Bedrock Guardrails with CloudWatch | https://docs.aws.amazon.com/bedrock/latest/userguide/monitoring-guardrails-cw-metrics.html | 2026-03-18 |
| aws_bedrock_pricing | Amazon Bedrock Pricing | https://aws.amazon.com/bedrock/pricing/ | 2026-03-18 |
