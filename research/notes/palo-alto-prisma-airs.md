# System Profile: Palo Alto Networks Prisma AIRS — AI Runtime Security

> **Assessment type:** documentation-only
> **Profile status:** complete
> **Last updated:** 2026-03-31

## 1. Overview

Prisma AIRS (AI Runtime Security) is Palo Alto Networks' commercial, enterprise-grade platform for securing AI applications, models, agents, and data in production. Launched in April 2025 (v1), updated to v2.0 in October 2025, and v3.0 in March 2026, the platform consolidates AI-specific threat detection with conventional network firewall capabilities. It offers two complementary interception modes—a network-layer inline firewall (Network Intercept) and a code-embedded scanning API (API Intercept)—and extends to agentic workloads via an MCP-aware relay and an AI Agent Gateway (preview as of March 2026).

**Vendor/Project:** [Palo Alto Networks — Prisma AIRS](https://www.paloaltonetworks.com/prisma/prisma-ai-runtime-security)
**Type:** Commercial — cloud-managed control plane (Strata Cloud Manager) with customer-deployed firewall instances; hybrid deployment possible

---

## 2. Deployment Models

- [x] Cloud-managed API (API Intercept: SaaS scanning endpoint; credentials managed in Strata Cloud Manager)
- [x] On-premises / self-hosted (Network Intercept firewall VM: ESXi `.ova` or KVM `.qcow2` image)
- [x] SDK / library (Python SDK `aisecurity` on PyPI; supports Python 3.9–3.13, sync and async)
- [x] Agent / sidecar (pan-mcp-relay sidecar for MCP tool call scanning; PAN-CNI DaemonSet for Kubernetes pod-level interception)

### Notes

**Network Intercept (AI Runtime Firewall):**
The inline firewall is a VM-Series-based virtual appliance that can be deployed in:
- Public clouds: AWS, Azure, GCP — automated via Strata Cloud Manager (Terraform templates) or manually via AMI/image selection. On AWS Marketplace the product is BYOL (Bring Your Own License).
- Private clouds / on-premises: ESXi (`.ova`) and KVM (`.qcow2`) images are downloadable for standalone deployment. Minimum spec: 2 vCPUs, 4.5 GB RAM, 3 network interfaces (management, client-side, server-side). OpenShift and Rancher container platforms also supported.
- Hyperscale Security Fabric (HSF) on KVM: cluster-mode deployment managed via Strata Cloud Manager (Terraform) and Panorama for high-throughput environments.
- Management plane: either Strata Cloud Manager (SCM) — cloud-hosted, supports Logical Router config — or on-premises Panorama (supports both Logical Router and Virtual Router configurations).

**API Intercept:**
A REST/SDK scanning service. Applications call out to regional SaaS endpoints at scan time:
- US: `https://service.api.aisecurity.paloaltonetworks.com`
- EU (Germany): `https://service-de.api.aisecurity.paloaltonetworks.com`
- India: `https://service-in.api.aisecurity.paloaltonetworks.com`
- Singapore: `https://service-sg.api.aisecurity.paloaltonetworks.com`

This mode requires outbound HTTPS connectivity to the regional endpoint. There is no documented self-hosted or air-gapped variant of the API Intercept scanning backend; the scanning intelligence runs as a Palo Alto-operated cloud service. Data does not require TLS decryption at the network layer — scanning is applied to the plaintext payload passed via the SDK or REST API.

**Financial-sector relevance:** The Network Intercept firewall VM can run fully on-premises (ESXi/KVM) with Panorama as the management plane, which does not require a cloud dependency for traffic inspection itself. However, the Strata Cloud Manager control plane (used for policy push and logging aggregation) is cloud-hosted. For strictly air-gapped deployments, Panorama (on-premises) is the supported management alternative. API Intercept currently calls out to cloud SaaS endpoints and does not support a self-hosted scanning backend.

---

## 3. Guardrail Locations

- [x] Pre-input (before prompt reaches the model) — both intercept modes support `pre_call` / `beforeRequestHook`
- [x] Post-input (LLM response scanned before returning to user) — both intercept modes support `post_call` / `afterRequestHook`

### Notes

**Network Intercept** inspects traffic inline at the network layer between AI applications and LLM model endpoints (east-west, north-south, inbound, outbound). It acts simultaneously on request and response without requiring application code changes.

**API Intercept** exposes explicit lifecycle hooks:
- `pre_call` — scan prompt before sending to LLM
- `during_call` — parallel scan (non-blocking path) while LLM is invoked
- `post_call` — scan LLM response before returning to user
- `pre_mcp_call` — scan MCP tool inputs before tool execution
- `during_mcp_call` — parallel scan of MCP tool inputs

There is no documented `post_mcp_call` hook (LiteLLM integration notes this as a current limitation). The MCP relay (pan-mcp-relay) scans tool descriptions, input parameters, and tool responses as a transparent proxy between MCP clients and servers.

---

## 4. Modality Support

- [x] Text (prompts, responses, tool call arguments)
- [x] Documents (RAG data source scanning for poisoning/PII — API Intercept)
- [ ] Images — not documented as a detection target for runtime guardrailing
- [ ] Audio
- [ ] Video

### Notes

The primary modality is text (prompts and model responses). The API Intercept can scan RAG data sources (vector database contents) for PII and poisoning. Database query outputs (SQL) are also inspected. No image/audio/video modality guardrailing is documented for the runtime security function; AI model security (pre-deployment scanning) covers model files in 20+ formats but that is a separate posture/scanning capability.

---

## 5. Constraint Types

- [x] Rule-based (regex and keyword-based DLP patterns — 1,000+ predefined patterns; URL categorization rules; SQL query type allowlists)
- [x] ML classifier (ML-based DLP patterns; AI-powered prompt injection detection; toxic content classifiers)
- [ ] LLM-as-judge (not documented)
- [ ] RLHF safety / aligned model (not applicable)
- [x] Static analysis (AI model file scanning: 25+ threat patterns, 20+ model formats — pre-deployment; agent code / MCP server artifact scanning — v3.0)

### Notes

The detection engine draws on Palo Alto Networks' Cloud-Delivered Security Services (CDSS) stack including:
- **Advanced URL Filtering** for malicious URL detection in prompts and responses
- **Advanced Threat Prevention** for known and zero-day application-layer attacks (>90% zero-day SQL injection prevention)
- **WildFire** threat intelligence for malware detection in model outputs (99% malware detection accuracy per datasheet)
- **Enterprise DLP** for data pattern matching (regex + ML hybrid)
- Proprietary ML classifiers for prompt injection (29+ attack categories, 8 languages) and toxic content (9 languages)
- Context-grounding validation against supplied RAG context (up to 100K character window, 8 languages)
- Custom topic guardrails (English only) — configurable organizational policy topics to block

---

## 6. Policy Language / Config Format

**Format:** GUI-driven AI Security Profile in Strata Cloud Manager (SCM); YAML for LiteLLM integration; JSON for Portkey; Python SDK for application-embedded scanning.

### API Intercept — Python SDK

```python
import aisecurity
from aisecurity.scan.inline.scanner import Scanner
from aisecurity.generated_openapi_client.models.ai_profile import AiProfile

aisecurity.init(api_key=os.getenv("PANW_AI_SEC_API_KEY"))
res = Scanner().sync_scan(
    ai_profile=AiProfile(profile_name="my-security-profile"),
    content={"prompt": user_prompt, "response": model_response},
    metadata={"app_user": "user123"}
)
# res.action == "block" | "allow"
# res.category == "malicious" | "benign"
```

### LiteLLM YAML Config

```yaml
guardrails:
  - guardrail_name: "panw-prisma-airs-guardrail"
    litellm_params:
      guardrail: panw_prisma_airs
      mode: "pre_call"          # pre_call | during_call | post_call | pre_mcp_call
      api_key: os.environ/PANW_PRISMA_AIRS_API_KEY
      profile_name: os.environ/PANW_PRISMA_AIRS_PROFILE_NAME
      api_base: "https://service.api.aisecurity.paloaltonetworks.com"
      timeout: 10.0
      fallback_on_error: "block"     # fail-closed (default) | "allow" fail-open
      mask_request_content: false    # mask instead of block for DLP hits
      mask_response_content: false
```

### pan-mcp-relay YAML Config

```yaml
mcpRelay:
  apiKey: ${PRISMA_AIRS_API_KEY}
  aiProfile: your-ai-profile-name-or-id

mcpServers:
  my-server:
    command: my-mcp-server-binary
    args: ["--port", "8080"]
```

### Network Intercept — SCM Security Profile

Policies are authored as "AI Security Profiles" within Strata Cloud Manager:
- Per-detection-type toggles: prompt injection, URL filtering (Basic/Advanced), DLP pattern sets, toxic content, malicious code, database query controls, custom topics
- Per-detection action: Block / Alert / Allow
- Profiles are attached to security policy rules pushed to firewall instances
- OWASP LLM Top 10 category coverage is tracked per-detection-type in the SCM dashboard

---

## 7. Extensibility

- **Custom DLP patterns:** Users can define custom regex data patterns alongside the 1,000+ predefined patterns.
- **Custom URL policies:** Domain-level allow/alert/block rules for URLs appearing in model input or output (fine-grained RAG web access control).
- **Custom topic guardrails:** Organizational policy topics can be defined (English only) to block off-topic or policy-violating LLM outputs.
- **Custom error responses:** API Intercept allows per-application custom error messages returned to end users when a violation is blocked (template supports `{category}` and `{action_type}` placeholders).
- **Per-user policies:** `app_user` metadata can be passed with each scan call, enabling user-specific policy enforcement.
- **Model-agnostic design:** API Intercept supports any LLM (public or private); no vendor lock-in on the model side. Network Intercept protects 40+ named cloud models on AWS/Azure/GCP plus direct OpenAI API traffic.
- **Third-party integrations:** Native integrations with LiteLLM proxy, Portkey AI gateway, and (announced) ServiceNow AI Control Tower. MCP relay is open-source (PyPI + GitHub).
- **Red-teaming extension:** Prisma AIRS 2.0+ includes an autonomous red-teaming module (500+ specialized attacks; NIST AI-RMF, OWASP, MITRE framework mappings) for pre-deployment or continuous adversarial assessment.
- **Agent artifact scanning:** v3.0 scans agent code, MCP server implementations, and skill artifacts for injection paths and unsafe permissions before deployment.

---

## 8. Observability & Auditability

**API Intercept scan logs:**
Every scan call produces a structured log record including: `timestamp`, `scan_id`, `report_id`, `app_name`, `verdict` (block/allow), `threat_type`, `severity`, `model_name`, `user_ip`, `session_URL`. Logs are forwarded to the **Strata Logging Service** (SaaS) and can be configured for SIEM forwarding (syslog, HTTPS, email, or third-party SIEM connectors) in multiple formats. SOC teams can receive alerts on critical threats via Strata Logging Service integrations.

**Network Intercept logs:**
Firewall generates AI security event logs (AI threat logs) through the standard PAN-OS logging pipeline. Logs flow to Strata Logging Service or to an on-premises Panorama log collector. From there, log forwarding to external SIEMs is supported.

**Dashboards:**
Strata Cloud Manager provides:
- AI ecosystem discovery map (applications, models, datasets, users, interconnections)
- Current threat posture dashboard (protected vs. unprotected AI apps, active threat alerts)
- Model view showing placement of AIRS instances and application-to-model connections
- OWASP LLM Top 10 coverage status per detection type

**Cortex XSIAM integration:** Documented as supported for unified SOC visibility across networks, clouds, endpoints, and AI workloads.

**Asynchronous scan results:** API Intercept supports async scan mode (`v1/scan/async/request`) with result polling (`v1/scan/results`) and full report retrieval (`v1/scan/reports`).

---

## 9. Performance Characteristics

| Metric | Value | Source |
|--------|-------|--------|
| API Intercept latency | <200 ms (vendor claim) | Search result summary from panw2025airs_docs |
| API Intercept SLA | 99.9% uptime | Search result summary from panw2025airs_docs |
| LiteLLM recommended timeout | 1–60 s (default 10 s) | panw2025airs_litellm |
| Network Intercept (overlay routing) | Eliminates traffic hairpinning, reduces double-inspection overhead | panw2025airs_docs release notes |
| Malware detection accuracy (CDSS/WildFire) | 99% | panw2025airs_datasheet |
| Zero-day SQL/command injection prevention | >90% | panw2025airs_datasheet |
| Data pattern coverage vs. other cloud DLP | 2x (vendor claim, no independent benchmark cited) | panw2025airs_datasheet |
| Prompt injection categories covered | 29+ attack categories | panw2025airs_mcp_blog |
| Network throughput | See VM-Series datasheet (referenced but not reproduced here) | panw2025airs_datasheet |

**Known limitations:**
- The sub-200 ms latency claim applies to the API Intercept cloud service; no independent third-party benchmark has been published.
- No `post_mcp_call` hook exists for scanning tool execution outputs in the LiteLLM integration; only pre/during-call MCP scanning is available.
- Custom topic guardrails are English-only.
- Contextual grounding supports 8 languages and is limited to a 100K-character context window.
- Network Intercept private cloud deployment (ESXi/KVM) requires minimum 2 vCPUs / 4.5 GB RAM; for high-throughput HSF clusters, resource requirements scale with VM-Series SKU selection.
- API Intercept backend is Palo Alto-operated SaaS; no self-hosted scanning engine variant is documented. Prompts and responses leave the customer's network to reach the scanning endpoint (regional endpoints provided to limit data residency concerns).
- The pan-mcp-relay is licensed under Polyform Internal Use License 1.0.0 (internal organizational use only; not for redistribution as a service).

---

## 10. Pricing / Licensing

**License:** Proprietary commercial; BYOL model on cloud marketplaces.

**Pricing model:**
- **Software NGFW Credits (FW-Flex / Flexible Credits):** The primary consumption model. Customers use existing or newly purchased Palo Alto Networks Software NGFW Flex Credits, which are fungible across VM-Series, Prisma AIRS, and other Software NGFW products. A Credit Estimator tool in the Customer Support Portal helps determine credit volume. Deployment profiles are created in the Customer Support Portal; API keys are then generated in Strata Cloud Manager.
- **AWS Marketplace:** BYOL — infrastructure costs billed by AWS; license purchased directly from Palo Alto Networks.
- **No public per-call or per-seat pricing** is disclosed. Pricing is managed through the Palo Alto Networks sales channel.
- The pan-mcp-relay (open-source repo) is free to use under Polyform Internal Use License but requires a valid Prisma AIRS API key (i.e., a paid license) to operate.

**Notes for financial sector:**
The BYOL + Flex Credits model allows organizations already holding Palo Alto Networks NGFW licensing to redeploy credits to AIRS without new procurement cycles. For on-premises private cloud deployment (ESXi/KVM + Panorama), no per-call cloud billing applies once deployed; the firewall appliance runs locally. However, licensing validation and telemetry may still require connectivity to Palo Alto Networks cloud services.

---

## 11. Sources

| Citation key | Title | URL | Retrieved |
|---|---|---|---|
| panw2025airs_datasheet | Prisma AIRS AI Runtime Security — Datasheet (Nov 2025) | https://cdn.blueally.com/paloguard/datasheets/ai-runtime-security.pdf | 2026-03-31 |
| panw2025airs_product | Prisma AIRS Product Page | https://www.paloaltonetworks.com/prisma/prisma-ai-runtime-security | 2026-03-31 |
| panw2025airs_docs | Prisma AIRS Documentation Hub | https://docs.paloaltonetworks.com/ai-runtime-security | 2026-03-31 |
| panw2025airs_pandev | Prisma AIRS API Intercept Developer Portal | https://pan.dev/airs/ | 2026-03-31 |
| panw2025airs_usecases | Use Cases: Prisma AIRS AI Runtime API Intercept | https://pan.dev/prisma-airs/api/airuntimesecurity/usecases/ | 2026-03-31 |
| panw2025airs_launch | Palo Alto Networks Introduces Prisma AIRS — Press Release (April 2025) | https://www.paloaltonetworks.com/company/press/2025/palo-alto-networks-introduces-prisma-airs--the-foundation-on-which-ai-security-thrives | 2026-03-31 |
| panw2025airs_2_0_blog | Prisma AIRS 2.0 — Blog (October 2025) | https://www.paloaltonetworks.com/blog/2025/10/prisma-airs-powering-secure-ai-innovation/ | 2026-03-31 |
| panw2025airs_2_0_press | Palo Alto Networks Secures the AI Agent Revolution with Prisma AIRS 2.0 | https://www.paloaltonetworks.com/company/press/2025/palo-alto-networks-secures-the-ai-agent-revolution-with-the-launch-of-prisma-airs-2-0 | 2026-03-31 |
| panw2026airs_3_0_blog | Securing the AI Enterprise — Introducing Prisma AIRS 3.0 | https://www.paloaltonetworks.com/blog/2026/03/prisma-airs-3-0-autonomous-ai/ | 2026-03-31 |
| panw2026airs_3_0_press | Palo Alto Networks Secures Agentic AI with Prisma AIRS 3.0 | https://www.prnewswire.com/news-releases/palo-alto-networks-secures-agentic-ai-with-prisma-airs-3-0--302722579.html | 2026-03-31 |
| panw2025airs_mcp_blog | Securing AI Agent Innovation with Prisma AIRS MCP Server (June 2025) | https://www.paloaltonetworks.com/blog/2025/06/securing-ai-agent-innovation-prisma-airs-mcp-server/ | 2026-03-31 |
| panw2025airs_mcp_docs | Prisma AIRS MCP Server for Centralized AI Agent Security | https://docs.paloaltonetworks.com/ai-runtime-security/activation-and-onboarding/prisma-airs-mcp-server-for-centralized-ai-agent-security | 2026-03-31 |
| panw2025airs_mcp_relay | pan-mcp-relay GitHub Repository | https://github.com/PaloAltoNetworks/pan-mcp-relay | 2026-03-31 |
| panw2025airs_private_cloud | Prisma AIRS AI Runtime Firewall for Private Clouds | https://docs.paloaltonetworks.com/ai-runtime-security/administration/deploy-airs-on-private-clouds | 2026-03-31 |
| panw2025airs_private_cloud_ext | Extend AI Security to Private Clouds (April 2025 What's New) | https://docs.paloaltonetworks.com/whats-new/new-features/april-2025/extend-ai-security-to-private-clouds | 2026-03-31 |
| panw2025airs_gcp_tutorial | Google Cloud AI Runtime Tutorial (GitHub) | https://github.com/PaloAltoNetworks/google-cloud-ai-runtime-tutorial | 2026-03-31 |
| panw2025airs_aws_marketplace | AI Runtime Security on AWS Marketplace (BYOL) | https://aws.amazon.com/marketplace/pp/prodview-pasyge5z56oiw | 2026-03-31 |
| panw2025airs_scan_logs | Prisma AIRS API Scan Logs | https://docs.paloaltonetworks.com/ai-runtime-security/administration/detect-and-alert-on-malicious-traffic/ai-runtime-security-intercept-scan-logs | 2026-03-31 |
| panw2025airs_litellm | PANW Prisma AIRS — LiteLLM Integration | https://docs.litellm.ai/docs/proxy/guardrails/panw_prisma_airs | 2026-03-31 |
| panw2025airs_portkey | Palo Alto Networks Prisma AIRS — Portkey Integration | https://portkey.ai/docs/integrations/guardrails/palo-alto-panw-prisma | 2026-03-31 |
| panw2025airs_helpnet_2_0 | Palo Alto Networks launches Prisma AIRS 2.0 — Help Net Security | https://www.helpnetsecurity.com/2025/10/29/palo-alto-networks-launches-prisma-airs-2-0-to-deliver-end-to-end-security-across-the-ai-lifecycle/ | 2026-03-31 |
| panw2026airs_helpnet_3_0 | Prisma AIRS 3.0 — Help Net Security | https://www.helpnetsecurity.com/2026/03/24/palo-alto-networks-prisma-airs-3-0-closes-visibility-gaps-in-autonomous-ai-systems/ | 2026-03-31 |
| panw2026airs_servicenow | Securing the Enterprise AI Ecosystem with ServiceNow and Prisma AIRS | https://www.paloaltonetworks.com/blog/2026/03/securing-enterprise-ai-ecosystem-servicenow-prisma-airs/ | 2026-03-31 |
