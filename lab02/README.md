# PortSwigger Web Security Academy — Web LLM Attacks

## Overview

Completed the **Web LLM Attacks** learning path on [PortSwigger Web Security Academy](https://portswigger.net/web-security/learning-paths/llm-attacks).

This learning path explores vulnerabilities that arise when web applications integrate Large Language Models (LLMs) with application APIs, external data sources, user information, and automated tools. These integrations introduce attack surfaces that may allow attackers to influence model behavior, access sensitive information, invoke unintended functionality, or affect other users and systems.

The learning path combines foundational concepts, vulnerability analysis, practical exploitation scenarios, and defensive security principles.

### Completion Status

| Attribute        | Details                                                           |
| ---------------- | ----------------------------------------------------------------- |
| Platform         | PortSwigger Web Security Academy                                  |
| Learning path    | Web LLM Attacks                                                   |
| Difficulty level | Practitioner                                                      |
| Progress shown   | 30/30 items completed                                             |
| Completion date  | 21 October 2026                                                   |
| Primary focus    | LLM application security                                          |
| Practical areas  | Prompt injection, API abuse, AI-agent security, data exfiltration |
| Evidence         | Completion screenshot; individual lab screenshots to be added     |

## Learning Objectives

The main objectives of this learning path were to understand how LLM integrations expand a web application's attack surface and how to assess these systems systematically.

- Understand LLM behavior and common web LLM attack vectors.
- Identify direct and indirect prompt-injection opportunities.
- Map the APIs, functions, and plugins accessible to an LLM.
- Evaluate excessive agency and unsafe tool invocation.
- Understand how attacker-controlled content can influence AI agents.
- Investigate unauthorized access to sensitive information.
- Explore risks associated with AI-powered web application scanners.
- Understand routing-based SSRF risks in LLM-integrated systems.
- Identify defensive controls that reduce the impact of LLM attacks.

## Topics Covered

### 1. Large Language Model Fundamentals

Studied the role of LLMs in web applications and how prompts influence model responses.

**Concepts covered:**

- LLM fundamentals and prompt processing.
- Direct prompt injection.
- Common LLM vulnerability patterns.
- Identifying inputs that may influence model behavior.
- Understanding the difference between intended model behavior and unsafe actions.

### 2. Exploiting LLM APIs, Functions, and Plugins

Explored how an LLM can interact with application functionality through API integrations and tool calls.

**Concepts covered:**

- How LLM API integrations work.
- Function calling and plugin-based workflows.
- Mapping the LLM API attack surface.
- Excessive agency and overprivileged tools.
- Chaining weaknesses across APIs.
- Risks caused by insufficient validation of LLM-generated output.

**Security takeaway:** An LLM's available tools should be treated as part of the application's attack surface. Authorization must be enforced by the application and API, not left to the model's judgment.

### 3. Indirect Prompt Injection

Studied how instructions embedded in external content can influence an LLM when the model retrieves or processes that content.

**Concepts covered:**

- Direct versus indirect prompt injection.
- Attacker-controlled web content.
- Trust boundaries between instructions and retrieved data.
- Manipulation of downstream model actions.
- Training-data poisoning concepts.

**Security takeaway:** Retrieved content must be treated as untrusted input, even when it appears within a legitimate page, document, review, or other data source.

### 4. Leaking Sensitive Training Data

Explored the risk of sensitive information being exposed through model responses.

**Concepts covered:**

- Sensitive information leakage.
- Data exposure through prompts and generated responses.
- Risks associated with poorly sanitized training data.
- Information disclosure caused by insufficient output filtering.

**Security takeaway:** Sensitive information should be minimized or removed from model-accessible data wherever possible. Output filtering alone is not a reliable substitute for proper data handling.

### 5. Defending Against LLM Attacks

Reviewed security practices for applications that expose APIs and sensitive data to LLMs.

**Concepts covered:**

- Treating LLM-accessible APIs as externally reachable attack surfaces.
- Enforcing authentication and authorization at the application layer.
- Minimizing sensitive information supplied to models.
- Limiting the permissions of tools and plugins.
- Avoiding reliance on system prompts as a security boundary.

**Security takeaway:** Prompt instructions are not access controls. Security-critical decisions must be enforced by deterministic application logic.

### 6. AI-Powered Scanner Vulnerabilities

Investigated how an AI-powered web application scanner can be influenced by attacker-controlled content discovered during a scan.

**Concepts covered:**

- Architecture of AI-powered web application scanners.
- Traditional scanners versus AI-powered scanners.
- Indirect prompt injection through website content.
- Crafting effective injection prompts.
- Excessive agency in automated security tools.
- Risks of unintended actions by an AI agent.

**Practical lab completed:**

- Exploiting AI agents to perform destructive actions — Apprentice.

**Security takeaway:** Automated security agents need restricted permissions, validated tool calls, and safeguards against instructions embedded in untrusted pages.

### 7. Data Exfiltration via AI-Powered Scanners

Studied how an AI scanner with access to sensitive information may be manipulated into disclosing that information.

**Concepts covered:**

- Sensitive-data exposure through agent workflows.
- Indirect prompt injection in scanner-accessible content.
- API authorization weaknesses.
- Unintended disclosure of account information.
- Risks of allowing agents to publish or transmit retrieved data.

**Practical lab completed:**

- Exploiting AI agents to exfiltrate sensitive information — Apprentice.

**Security takeaway:** AI agents should not have unrestricted access to secrets or permission to publish retrieved sensitive data. Tool access, data access, and output destinations must all be restricted.

### 8. Routing-Based SSRF and Secondary Vulnerabilities

Explored the relationship between LLM-driven requests and server-side request forgery (SSRF).

**Concepts covered:**

- Routing-based SSRF concepts.
- LLM-generated requests to application endpoints.
- Risks of accessing internal systems through automated agents.
- Chaining an LLM weakness with a secondary application vulnerability.
- The importance of request destination validation.

**Security takeaway:** Requests initiated by an AI agent must be subject to server-side network restrictions, destination allowlists, and appropriate validation.

### 9. Defending Against AI-Powered Scanner Vulnerabilities

Reviewed defensive design principles for AI-powered scanners and other agentic security tools.

**Concepts covered:**

- Least-privilege tool permissions.
- Server-side access-control enforcement.
- Separation of trusted instructions from untrusted content.
- Minimization of sensitive information available to agents.
- Validation of tool arguments and requested actions.
- Human approval for destructive or high-impact operations.
- Restrictions on outbound requests and data transfers.

## Practical Labs

The following labs are listed in the official Web LLM Attacks learning path. The two Apprentice labs below were completed as part of my practical work.

| Lab                                                               | Level        | Status      |
| ----------------------------------------------------------------- | ------------ | ----------- |
| Exploiting LLM APIs with excessive agency                         | Apprentice   | To document |
| Exploiting vulnerabilities in LLM APIs                            | Practitioner | To document |
| Indirect prompt injection                                         | Practitioner | To document |
| Exploiting AI agents to perform destructive actions               | Apprentice   | Completed   |
| Exploiting AI agents to exfiltrate sensitive information          | Apprentice   | Completed   |
| Bypassing AI scanner defenses to exfiltrate sensitive information | Practitioner | To document |
| Exploiting AI agents to trigger secondary vulnerabilities         | Practitioner | To document |

_Note: The overall learning path is complete at 30/30 items. The table above highlights practical labs from the path; individual statuses should be updated only after verifying each lab's completion._

## Practical Skills Developed

### LLM Application Security

- Identify direct and indirect prompt-injection vectors.
- Analyze the trust boundaries between prompts, retrieved content, and tool outputs.
- Assess how application functionality exposed to an LLM can be abused.

### API and Access-Control Testing

- Investigate insecure direct object reference (IDOR) and broken access-control scenarios.
- Understand the importance of object-level authorization.
- Evaluate the consequences of excessive permissions in AI-integrated applications.

### AI-Agent Security

- Analyze how untrusted content may influence agent decisions.
- Understand the risks associated with autonomous tool invocation.
- Assess unintended destructive actions and sensitive-data disclosure.
- Identify safeguards for tool permissions and sensitive operations.

### Security Analysis and Reporting

- Interpret scanner findings and tool-call logs.
- Distinguish vulnerability identification from demonstrated exploitation.
- Document attack conditions, impact, and mitigations.
- Connect practical lab observations to secure design principles.

## Key Security Lessons

1. **LLM integrations expand the attack surface.** Every API, plugin, data source, and tool exposed to an LLM introduces additional security considerations.

2. **Indirect prompt injection is a trust-boundary problem.** Instructions embedded in retrieved content may compete with the application's intended instructions.

3. **Authorization must be enforced outside the model.** The application must verify the user's identity, permissions, and access to each requested resource.

4. **Excessive agency increases impact.** An agent with broad permissions can cause greater damage when its behavior is manipulated.

5. **Sensitive data should be minimized.** Do not expose secrets to an LLM unless the application genuinely requires them.

6. **AI scanners require their own security controls.** A tool designed to find vulnerabilities can itself become an attack vector if untrusted content influences its actions.

7. **Prompt-based restrictions are insufficient.** System prompts should not replace access controls, output validation, network restrictions, or human approval for high-impact actions.

## Completion Evidence

The learning path dashboard shows **30 of 30 items completed** on 21 October 2026.

### 1. Learning Path Completion

<!-- Add the overall 30/30 completion screenshot here. -->

`Screenshot: web-llm-attacks-learning-path.png`

### 2. Practical Lab Evidence

<!-- Add individual lab completion screenshots here. -->

- Exploiting AI agents to perform destructive actions.
- Exploiting AI agents to exfiltrate sensitive information.

### 3. Additional Notes

<!-- Add sanitized scan results, observations, or remediation notes where appropriate. -->

## References

- [PortSwigger Web LLM Attacks Learning Path](https://portswigger.net/web-security/learning-paths/llm-attacks)
- [PortSwigger Web LLM Attacks Topic](https://portswigger.net/web-security/llm-attacks)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)

---

**Learning outcome:** Completed the Web LLM Attacks learning path and developed a practical understanding of prompt injection, LLM API abuse, excessive agency, sensitive-data exfiltration, AI-powered scanner vulnerabilities, and the defensive controls required to secure LLM-integrated web applications.
