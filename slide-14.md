# Chapter 14: Enterprise Deployment and Governance

## Deploy via Anthropic Cloud, Bedrock, Vertex AI, or Microsoft Foundry

---

Adopting Claude Code is not just about developer productivity — it requires enterprise-grade governance. The following sections cover the five-step governance pipeline and the available deployment options.

### The Governance Pipeline

#### Step 1: Identity

**SSO** with Okta, Azure AD, or any SAML 2.0 provider. Engineers log into Claude Code with the same credentials they use for everything else. No separate accounts, no password sprawl.

**SCIM auto-provisioning** means when someone joins the engineering team in the identity provider, they automatically get Claude Code access. When they leave, access is revoked automatically. No manual account management.

#### Step 2: Permissions

**Managed policies** let IT centrally enforce what Claude can and cannot do. For example:
- Claude cannot execute shell commands on production servers
- Claude cannot access files outside the project directory
- Claude must use the approved MCP servers only

These policies are configured centrally and pushed to all users. Individual developers cannot override them.

#### Step 3: Implement (Spend Controls)

**Per-org and per-user spend caps** prevent runaway costs. Set a monthly budget at the organization level, and optionally per-team or per-user limits.

API-rate billing means you pay for what you use, with full visibility into who is using how much. No surprises at the end of the month.

#### Step 4: Validate (Observability)

**OpenTelemetry traces** give full visibility into what Claude Code is doing — which tools it called, which files it read, how long each operation took.

**Analytics API** provides aggregated usage data — number of sessions, tokens consumed, tasks completed, models used.

**Full audit logs** record every action for compliance. Who asked what, when, and what Claude did in response.

#### Step 5: Commit (Security)

**No training on your data.** This is Anthropic's commitment — code sent to Claude is used only for inference, never for model training.

**TLS 1.3** encryption in transit. **AES-256** encryption at rest. **VPC isolation** for enterprise deployments.

### Deployment Options

There are four choices, depending on infrastructure and compliance requirements:

| Option | Best For |
|--------|----------|
| **Anthropic Cloud (SaaS)** | Fastest setup, fully managed, ideal for teams starting out |
| **AWS Bedrock** | VPC isolation, IAM integration, AWS-native organizations |
| **Google Vertex AI** | GCP-native, data residency requirements in GCP regions |
| **Microsoft Foundry** | Azure-native, enterprise policies, Microsoft ecosystem |

For organizations with strict data residency requirements, Bedrock, Vertex, and Foundry keep data within the cloud provider's infrastructure. The Claude models run inside the VPC — no data leaves the environment.

### Enterprise Best Practices

**Pin model versions** on Bedrock, Vertex, or Foundry. When a new model version is released, test it in staging before rolling it to production. This prevents unexpected behavior changes from affecting the team.

**Use LLM gateways** for centralized routing. Tools like LiteLLM or a custom gateway can route Claude Code requests through a single point, enabling centralized logging, rate limiting, and failover.

**Configure managed settings via plist (macOS) or Registry (Windows).** IT can push Claude Code configuration to all machines through MDM, ensuring consistent settings across the organization.

**Check `.mcp.json` into repos.** Every repository should have its MCP configuration committed so that team integrations are consistent and version-controlled.

**Route through corporate proxy** using the `HTTPS_PROXY` environment variable. Claude Code respects proxy settings, ensuring all traffic goes through the network security infrastructure.

### Security Commitments

| Certification | Status |
|--------------|--------|
| SOC 2 Type II | Certified |
| No data training | Contractual commitment |
| TLS 1.3 + AES-256 | All data in transit and at rest |
| VPC isolation | Available via Bedrock/Vertex/Foundry |

---

## Key Message for Leadership

Claude Code meets enterprise security and governance requirements out of the box. SSO, audit logs, spend controls, data residency, and zero training on your data. The question is not "is it secure enough?" — it is "how fast can we deploy it?"
