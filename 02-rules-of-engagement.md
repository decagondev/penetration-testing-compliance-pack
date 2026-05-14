# Penetration Test — Rules of Engagement (US, generic provider)

**Engagement reference:** [REF-YYYY-NNN]
**Version:** 1.0
**Date:** [Date]
**Classification:** Confidential — restricted to named parties

---

## 1. Parties

**Client (the "Engaging Party"):**
[Legal entity name, e.g., Acme, Inc.]
[State of incorporation]
[Principal place of business]
Primary contact: [Name, role, email, phone]

**Tester (the "Service Provider"):**
[Legal entity name or individual]
[State of incorporation, if applicable]
[Principal place of business]
[Relevant credentials — e.g., OSCP, OSCE, GIAC GPEN/GWAPT/GXPN, CCSP, CISSP]
[Professional liability insurer and policy number]
Primary contact: [Name, role, email, phone]

If the test is conducted by the Engaging Party's own staff rather than a third party, the "Tester" section identifies the named individual(s) authorized to conduct the test, and the engagement letter is replaced by the internal self-authorization memo.

## 2. Engagement window

- **Start:** [Date, time, timezone]
- **End:** [Date, time, timezone]
- **Testing hours:** [e.g., 09:00–18:00 PT, Monday–Friday]
- **Out-of-hours testing:** [permitted / not permitted] [with notice of X hours]
- **Reporting deadline:** [Date — typically end of window + 10 business days]

## 3. Hosting provider(s) and provider-specific rules

The application is hosted on the following provider(s):

| Provider | Service(s) | Account/Project ID | Pre-notification status |
|---|---|---|---|
| [e.g., AWS] | [EC2, RDS, S3] | [Account ID] | [Not required for permitted services] |
| [e.g., Cloudflare] | [WAF, CDN] | [Account] | [Allowlist configured] |

The Tester represents that they have reviewed the Provider Policy Reference (Document 04) and confirms that no technique permitted under section 5 of this ROE conflicts with any provider's Acceptable Use Policy or testing rules. The Tester will pause and consult the Engaging Party before any activity not clearly permitted under those policies.

## 4. Targets — in scope

The following are explicitly authorized as test targets:

| Target | Type | Environment | Notes |
|---|---|---|---|
| `staging.[domain]` | Web application | Staging | Primary target |
| `api.staging.[domain]` | REST API | Staging | All documented endpoints |
| [Provider resource: e.g., EC2 instances tagged `env=staging`] | Compute | Staging | Application-layer only |
| [Provider resource: e.g., RDS instance `staging-db`] | Database | Staging | Application-layer access only |
| [Provider resource: e.g., S3 bucket `acme-staging-assets`] | Object storage | Staging | Configuration review only |

All targets are operated by the Engaging Party. Authority to test derives from ownership; no third-party authorization is implied or extended to the provider's underlying platform.

## 5. Targets and activities — out of scope

The following are excluded from the engagement, regardless of any vulnerability discovered:

- The hosting provider's platform infrastructure, control plane, management consoles, provider APIs, container/VM runtime, host operating system, hypervisor, and any other component operated by the provider.
- Other tenants of the provider, including any attempt to enumerate or interact with them.
- Production environment at `[production domain]` — unless explicitly added below.
- Third-party services in the application's call path, including but not limited to: [list — e.g., Stripe, Auth0, SendGrid, Sentry, OpenAI, Cloudflare]. Configuration of the application's integration with these services is in scope; the services themselves are not.
- Corporate email, identity provider accounts, and any system not listed in section 4.
- Physical premises, staff, and any social engineering target.
- Source code repositories (unless a separate code review engagement is agreed).
- Provider-specific exclusions listed in Document 04 (e.g., AWS Route 53 DNS zone walking; Azure OpenAI model weight extraction; Amazon Connect testing).

## 6. Authorized techniques

The Tester is authorized to use the following techniques against the in-scope targets:

- Automated and manual vulnerability scanning, subject to rate limits in section 8.
- Web application testing per the OWASP Web Security Testing Guide and NIST SP 800-115.
- Cloud configuration review and IAM testing per the OWASP Cloud Security Testing Guide and the MITRE ATT&CK Cloud Matrix, limited to resources the Engaging Party operates.
- Authentication and session management testing using test accounts provided in section 9.
- Authorization testing including IDOR, privilege escalation between provided test accounts, and IAM role/policy analysis for the Engaging Party's cloud resources.
- Input validation testing including injection (SQL, NoSQL, command, template, etc.), XSS, XXE, deserialization.
- Business logic testing.
- API testing including parameter manipulation, rate limit testing within bounds, schema fuzzing.
- Configuration review of application-layer settings the Tester can observe.
- Dependency analysis of any package manifests exposed by the application.
- Storage object enumeration limited to buckets/blobs/objects owned by the Engaging Party.
- Metadata service interaction (e.g., AWS IMDS, GCP metadata server) from within Engaging Party compute resources, for the purpose of testing IMDS hardening.

## 7. Prohibited techniques

The Tester must not, under any circumstances:

- Conduct denial of service or distributed denial of service attacks, or generate load that has equivalent effect. Where DDoS-resilience testing is required, it will be conducted separately through the provider's approved DDoS-simulation channel (see Document 04) and is not part of this engagement.
- Brute-force credentials. Password-attack testing is limited to confirming that rate limiting, lockout, and complexity rules are in place — not to obtaining access.
- Upload, install, or leave behind any web shell, backdoor, implant, scheduled task, or persistence mechanism beyond the test window.
- Attempt to escape the container/VM, interact with the underlying host, or probe other tenants. Where the engagement permits proof-of-concept of a container/VM escape (some providers permit a single, immediately-reported PoC), the Tester must stop on initial demonstration and not exploit further.
- Attempt to access, modify, or test the provider's management console, control plane API, or any provider-operated system.
- Acquire personal information of any individual other than the test accounts in section 9. Where a vulnerability would, if exploited, expose such information, proof-of-concept stops at demonstrating the access path without retrieving the data.
- Modify the password, email, or other credentials of any account not provided to them.
- Engage in social engineering of any party.
- Conduct DNS zone walking, port flooding, protocol flooding, or any technique enumerated as prohibited in Document 04 for the provider(s) in scope.
- Publicly disclose any finding before the agreed disclosure window in section 14 has elapsed.
- Subcontract any part of the engagement without prior written agreement.

## 8. Source IPs and rate limits

- **Tester source IP(s):** [List, with reverse DNS where possible. Pre-shared so the Engaging Party can allowlist and so the Tester's activity can be distinguished from real attackers in logs.]
- **Maximum request rate:** [e.g., 10 requests per second per endpoint] sustained, with bursts not to exceed [e.g., 50 rps] for no longer than [e.g., 30 seconds].
- **Provider API rate limits:** Where the engagement involves enumeration of provider resources via provider APIs (e.g., AWS API calls during cloud-config review), the Tester will respect documented rate limits and use backoff. Unexpected API throttling is to be reported and not bypassed.
- **WAF/CDN/DDoS-shield allowlisting:** [Describe — e.g., "Cloudflare WAF rule `pentest-allowlist` permits Tester IPs to `staging.[domain]` for the duration of the engagement; AWS WAF web ACL `pentest-staging` similarly configured."]

## 9. Test accounts and credentials

The following accounts are provided for the Tester's use:

| Username | Role | Notes |
|---|---|---|
| `pentest-admin@[domain]` | Application administrator | [Password delivered separately via [channel]] |
| `pentest-user-a@[domain]` | Standard user | |
| `pentest-user-b@[domain]` | Standard user | For horizontal access testing |
| `pentest-readonly@[domain]` | Read-only | |
| [Provider IAM user/role] | [Read-only or scoped role for cloud-config review] | Where in scope |

Credentials will be delivered via [Signal / encrypted email / 1Password share / similar] separately from this document. All test accounts (application and provider) will be disabled within 24 hours of test completion.

## 10. Data handling

- Synthetic data only is present in the test environment. No real personal information, protected health information, cardholder data, or other regulated data is in scope.
- Any data extracted as proof of vulnerability must be limited to the minimum necessary to demonstrate the issue and must not include personal information of third parties.
- All test artifacts (screenshots, request/response logs, extracted data, cloud configuration dumps) must be stored encrypted at rest, transmitted only over encrypted channels, and destroyed within [30 / 60 / 90] days of report acceptance.
- If the Tester encounters data that appears to be real personal information, protected health information, cardholder data, or any other regulated data not expected to be present, the Tester must stop immediately, not retain copies, and notify the Engaging Party's primary contact within 2 hours.
- The Tester's data handling obligations are further defined in the engagement letter and, where applicable, in a Business Associate Agreement (for PHI) or other regulatory-specific agreement.

## 11. Communication and escalation

- **Primary channel:** [Signal group / Slack channel / email distribution list]
- **Daily check-in:** [Time] — short status, blockers, anything noisy.
- **Emergency contact (Engaging Party):** [Name, mobile, available hours]
- **Emergency contact (Tester):** [Name, mobile, available hours]
- **Out-of-band channel** (if primary is compromised): [Phone numbers]

The Tester must notify the Engaging Party immediately upon:

- Discovery of a critical-severity finding (CVSS ≥ 9.0).
- Discovery of any evidence of an actual, in-progress intrusion by a third party.
- Discovery of personal information, PHI, cardholder data, or other regulated data not expected to be in the test environment.
- Any indication that testing has affected the provider's platform or other tenants.
- Any test action that causes unexpected disruption.
- Receipt of any communication from the hosting provider regarding the activity (abuse report, suspension threat, etc.).
- Any indication that testing has affected systems outside the agreed scope.

## 12. Stop conditions

Testing pauses immediately if any of the following occur, and resumes only with written confirmation from the Engaging Party's primary contact:

- Production environment becomes affected, intentionally or otherwise.
- A platform-level vulnerability is suspected.
- A live intrusion by a third party is detected.
- An unanticipated outage of the staging environment.
- The hosting provider issues an abuse report, suspension notice, or other communication concerning the testing.
- The Engaging Party invokes a stop for any reason; no justification is required.

## 13. Deliverables

The Tester will produce:

1. **Executive summary** — non-technical overview suitable for sharing with leadership, customers, auditors (SOC 2, PCI QSA), or regulators.
2. **Technical report** — for each finding: title, description, affected component, reproduction steps, evidence, CVSS 3.1 score and vector, business impact, recommended remediation, references (CWE, NIST, MITRE ATT&CK technique IDs where applicable).
3. **Remediation appendix** — prioritized list with suggested timelines.
4. **Out-brief meeting** — one hour, technical, within [5 business days] of report delivery.
5. **Retest** of all critical and high findings within [30 days] of remediation, included in the fee.
6. **Attestation letter** — short signed statement suitable for sharing with customers or auditors as evidence the test was performed.

## 14. Disclosure and confidentiality

- The Tester treats all findings and any information learned in the engagement as confidential.
- Public disclosure of findings is permitted only with the Engaging Party's prior written consent.
- The Engaging Party may share the report with customers, auditors (SOC 2, PCI, HITRUST), regulators, and prospective business partners under appropriate NDA.
- If a finding affects the hosting provider's platform, the Engaging Party (not the Tester) will disclose it to the provider through the channel specified in Document 04.
- If the test reveals or causes a security incident potentially triggering state breach-notification statutes, HIPAA, GLBA, the SEC four-business-day rule (for public companies), or contractual notification obligations, the Engaging Party (not the Tester) is responsible for the notification.
- Confidentiality obligations survive termination of the engagement for [5 years].

## 15. Indemnification, insurance, and liability

The Tester represents and warrants that:

- Activities will be conducted in accordance with this document.
- Professional liability (errors and omissions) insurance of not less than $[amount] per claim and $[amount] aggregate, and commercial general liability insurance of not less than $[amount] per occurrence, are in force for the duration of the engagement. Certificates of insurance will be provided on request, naming the Engaging Party as additional insured where commercially reasonable.
- Staff conducting the engagement are appropriately qualified and supervised.
- Cyber liability insurance covering data handling activities is in force at not less than $[amount].

The Engaging Party indemnifies and holds the Tester harmless from third-party claims arising from activities conducted within the scope and rules of this document, except to the extent such claims arise from the Tester's negligence, willful misconduct, or breach of this document.

Liability caps and exclusions are as set out in the master services agreement between the parties; this document does not modify them.

## 16. Termination

Either party may terminate the engagement immediately upon written notice if the other party materially breaches this document. The Engaging Party may also terminate without cause; in that event, fees for work performed up to termination remain payable.

## 17. Governing law and venue

This document and the engagement it governs are governed by the laws of the State of [Delaware / California / state of Engaging Party's incorporation], without regard to conflict of laws principles. The parties consent to the exclusive jurisdiction of the state and federal courts located in [County, State] for any dispute arising under this document.

---

**Sign-off**

| Party | Name | Role | Signature | Date |
|---|---|---|---|---|
| Engaging Party | | | | |
| Tester | | | | |
