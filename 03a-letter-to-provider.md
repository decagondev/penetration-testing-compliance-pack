# Letter 1 — Notification / authorization request to hosting provider (US, generic)

**Use this letter if:** Your hosting provider requires pre-notification or pre-approval (see Document 04). Some providers explicitly require it (e.g., DigitalOcean, Vultr in practice, Linode as a courtesy, AWS for simulated DDoS/C2/phishing/malware/network stress). Others welcome optional notification (Azure has a notification form). Others have no formal process (GCP, Railway, Render).

**Whether to send this depends on your provider — Document 04 tells you.**

**Two flavors:** the first half (1A) is a notification used when no approval is required but you want a record on file. The second half (1B) is a formal authorization request used when the provider requires written consent before any testing.

---

# 1A — Courtesy notification (no approval required, record-on-file)

**Use this version if:** The provider permits testing without approval but you want documented evidence of notification — e.g., for a SOC 2 / ISO 27001 / PCI audit package, or because your engagement will be intensive enough to plausibly trigger abuse detection.

**Subject:** Notification of planned penetration test — [Account email / Project name]

To the [Provider] team,

I am writing to notify you of a planned penetration test of an application I operate on [Provider], in advance of the engagement, so that any unusual traffic patterns originating from the engagement can be correlated with this notification should your abuse-detection systems flag them.

**Account / project details**

- [Provider] account email: [your account email]
- Account / Subscription / Project ID: [identifier]
- Target environment: dedicated staging [project / subscription / account] (`[staging name]`), which is logically separated from the production environment.

**Engagement details**

- Test window: [start date and time] to [end date and time], [timezone].
- Conducted by: [internal team / external firm — name and relevant credentials, e.g., OSCP, GIAC, CISSP].
- Source IP(s) for the test traffic: [list].
- Anticipated traffic profile: standard web application and cloud configuration testing — manual and automated vulnerability scanning, authenticated session testing, business logic testing, IAM analysis — at rates not exceeding [N] requests per second per endpoint.

**Scope confirmation**

The engagement is strictly limited to the application code, configuration, and data we have deployed to the staging environment listed above. Our Rules of Engagement explicitly exclude:

- [Provider]'s platform, control plane, management console, provider APIs, hypervisor, host operating system, and network infrastructure;
- Other [Provider] tenants and any attempt to enumerate or interact with them;
- Denial of service or equivalent load testing;
- Any technique prohibited by [Provider]'s Acceptable Use Policy or testing rules.

Should the engagement incidentally surface any issue affecting the [Provider] platform, it will be reported separately through [Provider]'s vulnerability disclosure / bug bounty channel rather than further pursued.

**Contact during the engagement**

If your team observes activity from our test that you would like us to clarify or pause, please contact me at [phone] or [email] and we will respond within [stated SLA].

A copy of our Rules of Engagement is available on request under a customary NDA.

Thank you,

[Your name]
[Your title]
[Engaging Party legal name]
[Contact details]

---

# 1B — Authorization request (approval required before testing)

**Use this version if:** The provider's terms require pre-approval or written consent (e.g., DigitalOcean's standard ToS prohibits testing without prior written consent; some smaller VPS providers similarly). Send this and **wait for written confirmation** before testing begins. Note: this is fundamentally different from 1A — without a response in writing, you do not have authority to proceed.

**Subject:** Request for authorization — penetration testing — [Account email / Project name]

To the [Provider] team,

I am writing to request your written authorization to conduct a penetration test against an application I operate on [Provider]. I understand that [Provider]'s Acceptable Use Policy [or specific clause reference] requires prior written consent for security testing activities, and I am submitting this request for your review.

**Engaging Party details**

- Legal entity: [Engaging Party legal name]
- State of incorporation: [State]
- [Provider] account email: [your account email]
- Account / customer ID: [identifier]
- Primary technical contact: [Name, role, email, phone]
- Primary legal/business contact: [Name, role, email, phone]

**Target details**

- Target environment: dedicated staging [project / subscription / account] (`[staging name]`).
- Target hostnames / IPs: [list].
- Target services: [e.g., one VM running [application name] and one managed database].
- This environment is operated by the Engaging Party and contains no production data. All data in scope is synthetic.

**Engagement details**

- Proposed test window: [start date and time] to [end date and time], [timezone].
- Conducted by: [Tester firm name], a [State]-registered [entity type] with [CREST / OSCP-staffed / equivalent] credentials. Lead tester: [Name, qualifications].
- Source IP(s) for the test traffic: [list].
- Methodology: OWASP Web Security Testing Guide and NIST SP 800-115, manual and tool-assisted.
- Traffic profile: standard web application and cloud-configuration testing at rates not exceeding [N] requests per second per endpoint.

**Scope and constraints**

The engagement will be conducted under the Rules of Engagement document attached / available on request. Explicitly excluded:

- Any [Provider] system, service, or asset not listed above;
- Other [Provider] tenants;
- Denial of service, stress testing, or load generation;
- Brute-force credential attacks;
- Social engineering of [Provider] staff;
- Any technique prohibited by [Provider]'s Acceptable Use Policy.

Should we incidentally identify a vulnerability affecting [Provider]'s platform during the engagement, we will halt the relevant testing immediately and report through [Provider]'s vulnerability disclosure or bug bounty channel.

**Request**

Please confirm in writing whether you authorize the engagement on the terms above, or let us know what additional information you require. We will not commence any testing activity on [Provider] until we have received written authorization from you, and we will conduct the engagement strictly within any conditions you impose.

A copy of our certificate of insurance and our Rules of Engagement document are available on request under a customary NDA.

Thank you for your time.

Yours sincerely,

[Your name]
[Your title]
[Engaging Party legal name]
[Contact details]
[Date]
