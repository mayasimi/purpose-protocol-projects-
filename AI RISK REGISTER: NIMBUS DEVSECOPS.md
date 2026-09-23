# AI Risk Register: Nimbus DevSecOps

> Grounded in: "Five AI Security Breaches That Should Keep You Up at Night"  
> Worked Example, Version 1.0

---

## Executive Summary

Nimbus DevSecOps builds AI-augmented developer tooling, including an AI coding agent, a customer-facing support chatbot, and an autonomous incident-response agent. All three systems rely on MCP-based tool integrations to remain useful, which is the same design pattern behind every incident reviewed in our team's Threat Landscape research. This register identifies eight risks across data leakage, prompt injection, excessive agent autonomy, and governance gaps, each grounded in a real, publicly documented incident. The highest-priority finding is **R-08**: the broad tool permissions granted to both of Nimbus's agents amplify the impact of every other risk in this register, and should be the first area addressed.

---

## Company Profile

| Attribute | Detail |
|-----------|--------|
| **Company Name** | Nimbus DevSecOps (fictional) |
| **Industry** | AI-augmented developer tooling and DevSecOps platform vendor |
| **Size** | Approximately 60 employees, early-growth-stage startup |
| **AI System 1** | AI coding agent used internally by engineers, integrated via MCP with repositories, terminals, and deployment tooling |
| **AI System 2** | Customer-facing support chatbot on Nimbus's website and product portal |
| **AI System 3** | Semi-autonomous incident-response agent that triages alerts from monitoring platforms for both internal and customer environments |
| **Data Involved** | Proprietary source code, deployment credentials, customer support conversations, customer monitoring/error data |
| **Users** | Internal engineers (coding agent, incident-response agent) and external customers (support chatbot) |

---

## Risk Register

Each risk below is traced directly to an incident documented in the team's AI Threat Landscape Report submission.

---

### R-01: MCP Trust Boundary Exploitation via Malicious Tool Server

| Field | Detail |
|-------|--------|
| **Risk ID** | R-01 |
| **Risk Name** | MCP Trust Boundary Exploitation via Malicious Tool Server |
| **Affected AI System** | AI Coding Agent (MCP-integrated) |
| **Description** | Nimbus's engineers use an AI coding agent connected to internal tools (repos, terminals, deployment scripts) via MCP. A malicious or compromised MCP server, or crafted STDIO input, could trigger unintended command execution on engineering machines. |
| **Related Incident** | OX Security's April 2026 MCP research, showing 200,000+ potentially vulnerable instances across MCP-integrated frameworks and STDIO transport exploitation leading to OS command execution. |
| **Framework Mapping** | OWASP LLM Top 10, LLM03: Supply Chain Vulnerabilities; NIST AI RMF, Govern function |
| **Likelihood** | Medium. Requires a compromised or malicious MCP server to reach an engineer's agent, but MCP adoption is new and poorly vetted industry-wide. |
| **Impact** | High. Successful exploitation could lead to code execution on developer machines with access to source code and deployment credentials. |
| **Priority** | **High** |
| **Recommended Mitigation** | Maintain an allowlist of approved MCP servers, disable unreviewed STDIO adapters by default, and log every tool call an agent makes for later audit. |

---

### R-02: Prompt Injection Leading to Unauthorized Commitments by Support Chatbot

| Field | Detail |
|-------|--------|
| **Risk ID** | R-02 |
| **Risk Name** | Prompt Injection Leading to Unauthorized Commitments by Support Chatbot |
| **Affected AI System** | Customer-Facing Support Chatbot |
| **Description** | A customer could manipulate Nimbus's support chatbot into agreeing to terms, discounts, or commitments outside company policy by instructing it to treat its own responses as binding. |
| **Related Incident** | The December 2023 Chevrolet dealership chatbot incident, where a user got the bot to agree to sell a vehicle for one dollar by instructing it to treat its answers as legally binding. |
| **Framework Mapping** | OWASP LLM Top 10, LLM01: Prompt Injection |
| **Likelihood** | Medium. This is a low-effort, publicly known technique any customer could attempt. |
| **Impact** | Medium. Financial exposure is likely limited per incident, but reputational damage from a viral screenshot could be significant. |
| **Priority** | **Medium** |
| **Recommended Mitigation** | Constrain the chatbot's output to pre-approved response templates for anything involving pricing, discounts, or contractual language, and require human approval before any commitment is finalized. |

---

### R-03: Hallucinated Support Guidance Creating Legal or Financial Liability

| Field | Detail |
|-------|--------|
| **Risk ID** | R-03 |
| **Risk Name** | Hallucinated Support Guidance Creating Legal or Financial Liability |
| **Affected AI System** | Customer-Facing Support Chatbot |
| **Description** | The chatbot could provide incorrect guidance about refund policies, SLAs, or support processes that customers rely on, creating liability if Nimbus later disputes what the bot said. |
| **Related Incident** | The Air Canada bereavement fare case, where a tribunal held the airline responsible for its chatbot's inaccurate guidance and ordered damages, rejecting the argument that the chatbot was a separate legal entity. |
| **Framework Mapping** | OWASP LLM Top 10, LLM09: Misinformation; NIST AI RMF, Measure function |
| **Likelihood** | Medium. Any customer-facing chatbot answering policy questions carries this risk continuously. |
| **Impact** | Medium. Individual claims are likely small, but the legal precedent means Nimbus cannot disclaim responsibility for what the bot says. |
| **Priority** | **Medium** |
| **Recommended Mitigation** | Retain full, tamper-resistant chatbot conversation logs, restrict the bot to citing verified policy documents rather than generating policy language freely, and route ambiguous questions to a human. |

---

### R-04: Autonomous Agent Misuse for Large-Scale Malicious Activity

| Field | Detail |
|-------|--------|
| **Risk ID** | R-04 |
| **Risk Name** | Autonomous Agent Misuse for Large-Scale Malicious Activity |
| **Affected AI System** | Internal AI Incident-Response Agent |
| **Description** | Nimbus's incident-response agent has broad access to monitoring tools and can act semi-autonomously. If compromised or misdirected, it could be used to conduct reconnaissance or exfiltrate data at a scale and speed no human insider threat could match. |
| **Related Incident** | Anthropic's November 2025 disclosure of a Chinese state-sponsored group using Claude Code and a custom MCP-based framework to perform an estimated 80 to 90 percent of a cyber-espionage campaign's tactical work autonomously. |
| **Framework Mapping** | NIST AI RMF, Manage function; OWASP LLM Top 10, LLM08: Excessive Agency |
| **Likelihood** | Low. Requires a sophisticated attacker to first gain a foothold, but the consequence class is now proven to be real. |
| **Impact** | High. An autonomous agent with broad tool access could cause damage far faster than a human-driven attack before detection. |
| **Priority** | **High** |
| **Recommended Mitigation** | Require human confirmation at defined checkpoints for high-impact agent actions, and build agent-specific behavioral telemetry separate from standard user activity logs. |

---

### R-05: Poisoned Monitoring Data Triggering Unintended Privileged Actions (Agentjacking)

| Field | Detail |
|-------|--------|
| **Risk ID** | R-05 |
| **Risk Name** | Poisoned Monitoring Data Triggering Unintended Privileged Actions (Agentjacking) |
| **Affected AI System** | AI Coding Agent (MCP-integrated with monitoring tools) |
| **Description** | Nimbus's coding agent consumes error reports from a monitoring platform as trusted input. An attacker could inject a malicious error event that the agent interprets as an instruction, causing it to take unintended, privileged actions using the developer's own access. |
| **Related Incident** | Tenet Security's June 2026 Agentjacking research, which achieved an 85 percent success rate across 100+ tested targets by injecting malicious events into a public Sentry DSN, undetected by EDR, WAF, IAM, or firewall monitoring. |
| **Framework Mapping** | OWASP LLM Top 10, LLM01: Prompt Injection (indirect); LLM02: Insecure Output Handling |
| **Likelihood** | Medium. The attack requires only a publicly accessible monitoring endpoint, which is common in real deployments. |
| **Impact** | High. Traditional controls did not detect this class of attack in testing, meaning it could persist undetected. |
| **Priority** | **High** |
| **Recommended Mitigation** | Treat all tool output, including from internal monitoring platforms, as untrusted input requiring validation before the agent acts on it, and restrict which tool outputs can trigger privileged actions. |

---

### R-06: Source Code and Credential Leakage via AI Coding Agent

| Field | Detail |
|-------|--------|
| **Risk ID** | R-06 |
| **Risk Name** | Source Code and Credential Leakage via AI Coding Agent |
| **Affected AI System** | AI Coding Agent (MCP-integrated) |
| **Description** | The coding agent has read access to Nimbus's proprietary source code and, in some workflows, deployment credentials. A misconfigured MCP tool or an overly broad agent permission could expose this data to an external service or an unintended party. |
| **Related Incident** | Grounded in the same class of risk raised by OX Security's MCP research (Risk R-01): once an AI agent has broad tool access, the boundary between 'internal' and 'external' becomes difficult to enforce. |
| **Framework Mapping** | OWASP LLM Top 10, LLM06: Sensitive Information Disclosure |
| **Likelihood** | Medium. No incident in the sourced article confirms this exact outcome, but it follows directly from the access patterns described in R-01. |
| **Impact** | High. Nimbus's source code and credentials are its core intellectual property and access mechanism. |
| **Priority** | **High** |
| **Recommended Mitigation** | Scope agent credentials to the minimum required per task, rotate credentials regularly, and never grant a coding agent standing access to production deployment secrets. |

---

### R-07: Unapproved Shadow AI Tools Bypassing Governance

| Field | Detail |
|-------|--------|
| **Risk ID** | R-07 |
| **Risk Name** | Unapproved Shadow AI Tools Bypassing Governance |
| **Affected AI System** | N/A (organization-wide) |
| **Description** | Engineers frustrated with approved tooling limitations may adopt unapproved AI coding assistants or MCP servers outside of Nimbus's governance process, reintroducing every risk above without any of the mitigations in place. |
| **Related Incident** | No single incident in the sourced article documents this directly, but it is the underlying condition that made the OX Security MCP findings possible at scale: over 32,000 dependent repositories relying on frameworks with limited vetting. |
| **Framework Mapping** | NIST AI RMF, Govern function |
| **Likelihood** | High. Shadow IT and shadow AI adoption is well documented across the industry and requires no special access to occur. |
| **Impact** | Medium. Impact depends entirely on what the unapproved tool has access to, but it removes visibility as a baseline. |
| **Priority** | **High** |
| **Recommended Mitigation** | Provide an easy, fast-tracked approval path for new AI tools so engineers are not incentivized to go around governance, paired with network-level detection of unapproved AI tool traffic. |

---

### R-08: Excessive Agent Permissions Enabling Unintended Actions

| Field | Detail |
|-------|--------|
| **Risk ID** | R-08 |
| **Risk Name** | Excessive Agent Permissions Enabling Unintended Actions |
| **Affected AI System** | AI Coding Agent and Incident-Response Agent |
| **Description** | Both of Nimbus's autonomous agents are granted broad tool permissions to remain useful across many tasks. This same design choice is what allowed each incident in the sourced article to escalate from a model behavior issue into a real-world action with consequences. |
| **Related Incident** | This risk is the common thread across all five incidents in the sourced article: in each case, the AI system's ability to act, not merely to answer, is what turned a model failure into an operational or financial incident. |
| **Framework Mapping** | OWASP LLM Top 10, LLM08: Excessive Agency |
| **Likelihood** | Medium. This is a design-level risk present by default unless deliberately constrained. |
| **Impact** | High. This risk amplifies the impact of every other risk in this register. |
| **Priority** | **Critical** |
| **Recommended Mitigation** | Apply least-privilege permissioning per agent task rather than per agent overall, and require periodic review of what each agent is actually authorized to do versus what it needs. |

---

## Note on Method

Risks **R-01** through **R-05** each map directly to one of the five incidents in the sourced Threat Landscape article. Risks **R-06** through **R-08** are *derived risks*: they are not individually documented incidents, but follow logically from the access patterns and design choices described across the sourced incidents. This distinction is called out explicitly in each entry's **Related Incident** field, consistent with the protocol's requirement to state clearly when a risk is inferred rather than directly evidenced.

---

*This document is a fictional worked example for educational and illustrative purposes.*
