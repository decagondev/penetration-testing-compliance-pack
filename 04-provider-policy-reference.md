# Provider Policy Reference (US)

**Version:** 1.1
**Last verified:** [Date]
**Maintained by:** [Owner]

**Changelog:**
- 1.1 — Separated "pre-authorization (legal)" from "operational configuration required" as distinct columns. Expanded Cloudflare section with current mandatory pre-test ruleset configuration. Added rows for AWS WAF/CloudFront/Shield and third-party WAF/CDN (Akamai/Fastly/Imperva).
- 1.0 — Initial version.

---

## Purpose

This document summarizes the penetration-testing rules of major hosting and infrastructure providers. It is **a reference, not a substitute** for reading the provider's current policy. Provider policies change. Always click through to the source and verify before relying on a summary.

## Two separate questions to answer for each provider

Before reading the matrix, separate these two questions — they often get conflated and they have very different consequences if you get them wrong:

**1. Pre-authorization (legal/contractual).** Does the provider's AUP or testing policy require you to ask permission before testing your own resources? If yes and you skip it, you breach the contract — and depending on the provider, may also create CFAA exposure since you no longer have unambiguous authorization for the access. Four possible values:

- **Not required** — you may test your own resources without notifying or asking permission, provided you comply with the Acceptable Use Policy.
- **Optional / encouraged** — testing is permitted without prior approval, but the provider offers a notification form that creates a paper trail and reduces the risk of abuse-detection issues.
- **Required for specific activities** — most testing is permitted without approval, but certain activities (DDoS simulation, network stress, C2, phishing simulation, etc.) require explicit pre-authorization.
- **Required for all testing** — written consent is required before any security testing of your resources begins. Proceeding without it breaches the provider's terms.

**2. Operational configuration.** Does the provider have technology in the request path (WAF, CDN, bot manager, DDoS shield) that will interfere with the test or flag your tester? If yes, configure it before testing begins — this is a separate question from authorization. Even providers with no authorization requirement (Cloudflare being the clearest example) often have specific operational requirements: rulesets that must be enabled, IP ranges to allowlist, endpoints to exclude, rate limits to respect.

A provider can be "no approval needed" *and* have substantial operational requirements. Don't read the first as implying the second is empty.

## Quick reference matrix

| Provider | Pre-authorization (legal) | Operational config required before testing | DoS / DDoS testing | Bug bounty / disclosure |
|---|---|---|---|---|
| **Amazon Web Services (AWS)** | Not required for permitted services; required for simulated DDoS, C2, network stress, phishing, malware testing | None for standard testing; allowlist AWS WAF if used; respect API rate limits | Prohibited as standard; permitted only via Simulated Events form / DDoS Simulation Testing policy | [email protected] |
| **Microsoft Azure / Microsoft Cloud** | Not required since 2017; notification form available and recommended | None for most tests; AI service tests have specific PoC-and-stop rules | Prohibited; approved DDoS-simulation partners only (BreakingPoint Cloud, Red Button, RedWolf) | Microsoft Security Response Center (MSRC) |
| **Google Cloud Platform (GCP)** | Not required; no formal notification process | None; respect API quotas to avoid unexpected billing | Prohibited under Acceptable Use Policy | Google Vulnerability Reward Program |
| **Railway** | Not required for your own application | None — no native WAF/CDN in front by default | Prohibited | [email protected] / Bug Bounty Program |
| **Render** | Not required for your own services; courtesy notification welcome | None by default | Prohibited | [email protected] |
| **Heroku (Salesforce)** | Notification required; specific instructions and form | Form submission and acknowledgement before testing | Prohibited | Salesforce Trust / Heroku security |
| **DigitalOcean** | **Required for all testing — written consent before testing begins** | Submit support ticket; obtain written approval; wait | Prohibited | [email protected] |
| **Linode (Akamai)** | Not required, but provider strongly recommends courtesy support ticket | Courtesy support ticket recommended | Prohibited (explicitly: no DDoS stress testing) | Akamai security |
| **Vultr** | Ambiguous / conservative reading required | Submit support ticket; treat as approval-required in practice | Prohibited | [email protected] |
| **Hetzner** | Notification strongly advised; aggressive abuse monitoring | Notify before any testing; obtain acknowledgement | Prohibited | [email protected] |
| **OVHcloud** | Not required for own resources; AUP applies | None | Prohibited | [email protected] |
| **Fly.io** | Not required for own apps | None | Prohibited | [email protected] |
| **Back4App** | Not clearly documented publicly; request authorization | Request explicit written consent | Prohibited | Contact support |
| **Cloudflare** (as WAF/CDN in front of your origin) | Not required | **Yes — substantial.** Deploy Managed Ruleset and OWASP Core Ruleset on the zone under test; throttle to a reasonable rate; exclude `/cdn-cgi/` endpoints; do not target `*.cloudflare.com`; allowlist tester IPs if origin-direct testing is desired | DDoS testing prohibited under standard terms; permitted via separate "Simulating test DDoS attacks" notification process | [email protected] / HackerOne bug bounty |
| **AWS WAF / CloudFront / Shield** (when used in front of an AWS origin) | Covered by AWS policy above | Allowlist tester IPs in web ACL; respect Shield Advanced if enabled | See AWS row | See AWS row |
| **Akamai / Fastly / Imperva** (third-party WAF/CDN) | Generally not required for testing your own origin; check each provider's AUP | Allowlist tester IPs; coordinate with the WAF/CDN account team for intensive engagements | Provider-specific; usually prohibited | Provider-specific |

## Per-provider detail

### Amazon Web Services

- **Policy URL:** https://aws.amazon.com/security/penetration-testing/
- **Permitted services for testing without approval:** Amazon EC2, Amazon RDS, Amazon CloudFront, Amazon Aurora, Amazon API Gateway, AWS Lambda and Lambda Edge functions, Amazon Lightsail, AWS Elastic Beanstalk.
- **Prohibited under all circumstances:** Security assessments of AWS infrastructure itself or AWS services. Attempting to access another customer's data. Testing Amazon Connect (specifically excluded).
- **Requires prior approval (Simulated Events form):** Simulated DoS/DDoS, port flooding, protocol flooding, DNS zone walking against Route 53 hosted zones, network stress testing, iPerf testing, simulated phishing, malware testing, Command and Control (C2) hosting and use.
- **Reporting platform issues:** [email protected]
- **AWS GovCloud:** see https://docs.aws.amazon.com/govcloud-us/latest/UserGuide/pen-testing.html for additional federal-specific rules.

### Microsoft Azure / Microsoft Cloud

- **Policy URL:** https://www.microsoft.com/en-us/msrc/pentest-rules-of-engagement
- **Pre-approval:** Not required since June 15, 2017. Azure-specific Penetration Testing Notification form is optional but encouraged for traffic-intensive engagements.
- **Covered services:** Azure, Microsoft 365, Dynamics 365, Power Platform, Microsoft Intune, Azure DevOps, all under the unified rules.
- **Permitted:** OWASP top 10 testing against your endpoints; DAST of your web apps and APIs; testing isolation of Azure Functions / App Service (with a single PoC and immediate stop on success); fuzzing within your VMs; tenant-monitoring tests using EICAR; CA/MAM policy testing; AI system boundary testing (with PoC and report).
- **Prohibited:** Any kind of DoS testing; network-intensive fuzzing of anything except your Azure VMs; automated testing generating significant traffic; access to Microsoft or other customer data beyond test accounts; phishing or social engineering against Microsoft employees; extracting AI training data, model weights, or architectures.
- **DDoS resilience testing:** Through approved partners only — BreakingPoint Cloud, Red Button, RedWolf.
- **Reporting:** Microsoft Security Response Center (MSRC).
- **Safe harbor:** Microsoft Bounty Legal Safe Harbor for good-faith testing.

### Google Cloud Platform

- **Policy:** https://support.google.com/cloud/answer/6262505
- **Pre-approval:** Not required. No formal notification process.
- **Requirements:** Comply with the GCP Acceptable Use Policy and Terms of Service. Tests must affect only your own projects.
- **Prohibited:** Affecting other customers' applications; activities that constitute DoS; activities prohibited by the AUP generally.
- **Reporting platform issues:** Google Vulnerability Reward Program.
- **Practical note:** Apigee (and similar managed services where the customer does not have full configuration control) may require a support ticket first. Check service-specific docs.

### Railway

- **Policy:** https://railway.com/legal/acceptable-use; https://railway.com/bug-bounty
- **Pre-approval:** Not required for testing your own application code and data.
- **Prohibited under bug-bounty rules** (which apply by extension to in-scope behavior on the platform): DDoS; brute-force; uploading shells/backdoors; social engineering; exfiltration of any data other than your own; modification of any account not yours; public disclosure before review.
- **Reporting platform issues:** [email protected] (Bug Bounty Program).

### Render

- **Policy:** https://render.com/security (verify current URL)
- **Pre-approval:** Generally not required for testing your own services; courtesy notification welcome.
- **Reporting platform issues:** [email protected]
- Treat similarly to Railway in operational posture.

### Heroku (Salesforce)

- **Policy:** https://devcenter.heroku.com/articles/pentest-instructions
- **Pre-approval / notification:** Historically required submission of a penetration testing form before any testing of Heroku-hosted apps. Verify current requirement before relying on this.
- **Reporting platform issues:** Salesforce Trust.

### DigitalOcean

- **Policy:** Their standard Terms of Service / AUP. The relevant clause (subject to change): "You may not attempt to probe, scan, penetrate, or test the vulnerability of a DigitalOcean system or network, or to breach the DigitalOcean security or authentication measures... without DigitalOcean's prior written consent."
- **Pre-approval:** **Required.** The clause has been read by their support team as applying both to testing of DigitalOcean's infrastructure and to testing conducted on or from DigitalOcean resources.
- **Practical implication:** Submit a support ticket describing the scope, dates, and targets. Wait for written authorization. Do not proceed without it.
- **Reporting platform issues:** [email protected]

### Linode (Akamai)

- **Policy:** Acceptable Use Policy plus Linode community guidance.
- **Pre-approval:** Technically the AUP requires "express written consent" for vulnerability testing of Linode or its services, but Linode support has clarified that testing of your own services from a Linode is permitted provided it does not affect Linode infrastructure, other customers, or fall outside an agreed scope. A support ticket as a heads-up is recommended.
- **Specifically prohibited:** DDoS stress testing.
- **Reporting platform issues:** Akamai security channels.

### Vultr

- **Policy:** Acceptable Use Policy. Key clause: port scanning "only permitted if explicitly authorized by the destination host."
- **Pre-approval:** Ambiguous on paper, restrictive in practice. Community reports include account suspensions from legitimate testing activity that "trips security parameters."
- **Recommendation:** Treat as **required for all testing** — submit a support ticket and obtain written confirmation before testing.
- **Reporting platform issues:** [email protected]

### Hetzner

- **Policy:** Strict AUP with aggressive abuse monitoring.
- **Pre-approval:** Strongly advised — community reports document account suspensions within 1–2 hours of port scanning starting.
- **Recommendation:** Notify before any testing; obtain written acknowledgement.
- **Reporting platform issues:** [email protected]

### OVHcloud

- **Pre-approval:** Not generally required for testing of your own resources; AUP applies.
- Among the more permissive providers, including for security tooling hosted on their infrastructure.

### Cloudflare (acting as WAF/CDN in front of another hosting provider)

- **Policy URL:** https://developers.cloudflare.com/fundamentals/reference/scans-penetration/
- **Pre-authorization (legal):** Not required. Cloudflare's published policy explicitly permits customers to conduct scans and penetration tests on their own assets (zones within their Cloudflare accounts) without seeking authorization, subject to the restrictions below.
- **Operational requirements before testing (these are not optional):**
  - Deploy the Cloudflare Managed Ruleset on the zone under test and enable all rules.
  - Deploy the Cloudflare OWASP Core Ruleset.
  - Create a custom rule blocking requests based on WAF attack score (e.g., score 1–20).
  - Create a custom rule for malicious upload detection.
  - On Pro/Business plans without Bot Management, enable Super Bot Fight Mode. On entitled zones, ensure Bot Management is enabled.
  - Throttle scan rate to avoid disruption.
  - Exclude `/cdn-cgi/` endpoints from scans (false positives).
  - Do not target `*.cloudflare.com` or other Cloudflare-owned destinations — those are bug-bounty-only.
- **Note on testing the origin directly:** Some engagements call for bypassing Cloudflare to test the origin server's true posture. If you do this, allowlist tester IPs at the origin (typically by IP firewall rules or by removing the Cloudflare-only allowlist for the duration of the test), and understand that you are now testing without the WAF in place — findings will not reflect production security.
- **DDoS testing:** Prohibited under the standard policy. Separate "Simulating test DDoS attacks" notification process exists for legitimate DDoS-resilience testing — see Cloudflare's docs.
- **Known false positives in customer tests:** Cloudflare lists ROBOT vulnerability findings as false positives for customers behind Cloudflare; non-standard HTTP ports may appear "open" via Netcat or similar tools but are open for Cloudflare's routing, not necessarily for the origin.
- **Reporting Cloudflare-platform issues:** [email protected] or via Cloudflare's HackerOne bug bounty program.
- **Customers can download Cloudflare's own pentest report** via the dashboard if you need it for your own audit package (SOC 2, etc.).

## How to use this reference

1. Identify every provider in your application's stack — hosting, CDN/WAF, DNS, identity, payments, email, monitoring, AI. Each is a separate provider.
2. For each, decide whether testing will touch it. Application-layer testing typically touches the hosting provider only. Cloud-config testing touches the hosting provider's APIs.
3. For each provider in scope, read the linked policy in full. Verify the rules in this document match what is currently published.
4. If pre-authorization is required, send the authorization request (Document 03a Letter 1B) and wait for written reply.
5. If notification is optional but recommended, send the courtesy notification (Document 03a Letter 1A).
6. Record the result in the table at the top of this document and in section 3 of the Rules of Engagement.

## Maintenance

This document should be re-verified before each engagement. Provider policies change without warning. The "Last verified" date at the top is a hard requirement, not a formality — if it is older than 90 days, re-check every link before relying on the document.

## Disclaimer

This is a working reference, not legal advice. Provider terms are binding contracts and their interpretation can be fact-specific. For high-stakes engagements, have counsel verify the current terms of every provider in scope before testing.
