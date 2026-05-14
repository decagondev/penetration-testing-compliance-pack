# Letter 3 — Internal self-authorization memo (US, generic)

**Use this memo if:** You are conducting the test yourself, or your in-house team is conducting it. It records, at an officer level, that the testing was authorized by the appropriate owner of the asset — the document you produce if the activity is ever questioned by your hosting provider, a regulator, law enforcement, or in litigation.

**Sign and file this before testing begins.** Retain for at least the period required by any applicable framework (SOC 2: through the next audit; HIPAA: 6 years; SOX: 7 years; longer where state law or insurance carrier requires).

---

[Your letterhead]

**MEMORANDUM — INTERNAL**

**To:** File / Information Security record
**From:** [Officer name and title — e.g., Chief Technology Officer]
**Date:** [Date]
**Re:** Authorization of penetration testing — [Application name]

**1. Purpose**

This memorandum records the authorization by [Engaging Party legal name] (the "Company") of penetration testing of the application identified below, and constitutes the written authority under which the testing is conducted. It is intended to evidence consent for purposes of the Computer Fraud and Abuse Act (18 U.S.C. § 1030), applicable state computer-crime statutes, the Digital Millennium Copyright Act (17 U.S.C. § 1201), and any other applicable law that turns on authorization by the system owner.

**2. Hosting provider compliance**

The application is hosted on the following provider(s):

| Provider | Service(s) used | Account/Project ID | Provider pre-notification status |
|---|---|---|---|
| [e.g., AWS] | [EC2, RDS] | [Account ID] | [Not required for permitted services; recorded in file] |
| [e.g., Cloudflare] | [WAF] | [Account] | [Allowlist configured for tester IPs] |

The undersigned has reviewed Document 04 (Provider Policy Reference) and confirms that:

- Where the provider requires pre-notification or pre-authorization, this has been obtained and a copy is filed with this memorandum.
- Where the provider's Acceptable Use Policy prohibits specific techniques, these are excluded from the scope under section 5 of the Rules of Engagement.
- Where the provider's testing rules require specific channels for DDoS simulation, social-engineering simulation, or similar, those channels (and not application-layer testing) will be used.

**3. Application and assets covered**

The testing authorized by this memorandum is limited to assets owned and operated by the Company, namely:

- The application known as [Application name].
- The [provider project / subscription / account] `[staging name]` and all services, databases, and configurations within it.
- The domains [`staging.example.com`], [other staging hostnames].
- The test accounts listed in section 9 of the Rules of Engagement.

This memorandum does not, and cannot, authorize testing of:

- Any system, service, or asset operated by the hosting provider;
- Any other tenant of the hosting provider;
- Any third-party service in the application's call path (Stripe, Auth0, etc.);
- Any other system not listed above.

**4. Persons authorized**

The following persons are authorized to conduct the testing:

| Name | Title | Identifier |
|---|---|---|
| [Name] | [Title] | [Employee ID] |
| [Name] | [Title] | [Employee ID] |

[Or, if external: "The testing is conducted by [Tester firm] under the engagement letter dated [date], a fully executed copy of which is filed with this memorandum."]

**5. Period of authorization**

This authorization is effective from [start date and time] to [end date and time] inclusive, [timezone]. It may be extended only by a further memorandum signed by an officer of comparable authority, or revoked at any time by written notice to the authorized persons.

**6. Conditions**

The authorization is granted on condition that:

- Testing is conducted strictly in accordance with the Rules of Engagement dated [date], a copy of which is filed with this memorandum.
- No technique prohibited by the hosting provider's Acceptable Use Policy or testing rules (Document 04) is used.
- No personal information, protected health information, cardholder data, or other regulated data of any individual other than the named test accounts is accessed, processed, or exfiltrated.
- The authorized persons stop immediately and notify the undersigned if any of the stop conditions in section 12 of the Rules of Engagement arises, including any communication from the hosting provider regarding the testing.
- A written report is produced within the time specified in the Rules of Engagement.
- Any finding affecting the hosting provider's platform is reported to the provider's vulnerability disclosure or bug bounty channel rather than further explored.

**7. Authority of signatory**

The undersigned officer represents that they hold the position stated below within the Company, are duly authorized to commit the Company to the activities described in this memorandum, and have authority to authorize access to the systems described in section 3.

**8. Record retention**

This memorandum, together with the Rules of Engagement, the Provider Policy Reference (Document 04), and any associated engagement letter or provider correspondence, constitutes the written record of authorization for the testing. It will be retained by the Company in its information security records for a minimum of [the longer of (a) seven (7) years from the date of completion or (b) the period required by any framework under which the Company is certified or audited (e.g., SOC 2, PCI DSS, HIPAA)].

---

**Signed:**

Name: __________________________

Title: __________________________

Signature: __________________________

Date: __________________________

---

**Witness / countersignature (recommended for sole-proprietor, single-officer, or small-company engagements):**

Name: __________________________

Title: __________________________

Signature: __________________________

Date: __________________________
