# Penetration Test Documentation Pack (US, generic provider)

Provider-agnostic version of the US pack. Use this when:

- You host on something other than Railway (AWS, GCP, Azure, DigitalOcean, Vultr, Linode, Render, Heroku, Fly.io, OVHcloud, Hetzner, Back4App, etc.).
- You host on multiple providers.
- You want a single set of templates you can reuse across engagements.

## Files

| # | File | When to use |
|---|---|---|
| 01 | `01-pentest-planning-and-compliance.md` | The reference document. Read first. Edit once for your situation; refer back during the engagement; produce on request to auditors. |
| 02 | `02-rules-of-engagement.md` | The operational contract for the test itself. Edit per engagement. Signed by both parties before testing starts. |
| 03a | `03a-letter-to-provider.md` | Has two variants — **1A** is a courtesy notification (provider doesn't require approval but you want a paper trail); **1B** is a formal authorization request (provider requires consent before testing). Document 04 tells you which one to use. |
| 03b | `03b-engagement-letter-to-tester.md` | Use if hiring a third-party tester. Countersigned by the tester. |
| 03c | `03c-self-authorization-memo.md` | Use if testing yourself or with an in-house team. Signed by an officer and filed before testing starts. |
| 04 | `04-provider-policy-reference.md` | **The key document for a multi-provider world.** Summarizes each major provider's rules, with pre-authorization status, prohibited techniques, and disclosure channels. Read this before every engagement, and re-verify if it has not been checked in 90+ days. |

## Minimum set you actually need

- **Hosting on a "no approval required" provider (AWS, GCP, Azure, Railway, Render, GCP, OVHcloud, Linode), hiring a third party:** documents 01, 02, 03b, and 04. 03a Letter 1A is optional.
- **Hosting on a "approval required" provider (DigitalOcean, Vultr, Hetzner, Heroku):** documents 01, 02, **03a Letter 1B (signed reply on file)**, 03b or 03c, and 04. The signed authorization is non-negotiable.
- **Testing yourself or with in-house staff:** swap 03b for 03c.

## Order of operations

1. **Open Document 04 first.** Identify every provider in your stack. Read each provider's current testing policy. Mark each provider's pre-authorization status.
2. Edit Document 01 to reflect your application, sector, and any compliance overlay (SOC 2, PCI DSS, HIPAA, GLBA, SOX, FedRAMP, etc.).
3. Edit Document 02 with the engagement specifics, including the provider table in section 3.
4. For any provider requiring pre-authorization: send Document 03a Letter 1B, wait for written reply, file it.
5. For any provider where notification is optional and you want a record: send Document 03a Letter 1A, file the sent copy.
6. Sign whichever of 03b or 03c applies.
7. Run through the pre-test checklist in section 9 of Document 01.
8. Test.
9. Complete the post-test activities in section 10 of Document 01.

## Key things to set when editing

- **Providers in scope.** Document 04 gives you the table to fill in. Don't miss the CDN/WAF, DNS, identity, and payment providers — they each have their own policies.
- **Governing law / venue.** The templates default to "the State of [Delaware / California / Engaging Party's state]." Pick one — typically the state of incorporation for the engaging entity, or the state specified in your master services agreement with the tester.
- **Insurance amounts.** The engagement letter has dollar-amount placeholders. Typical minimums for a non-trivial engagement: professional liability $1–2M per claim / $2–5M aggregate; commercial general liability $1M per occurrence / $2M aggregate; cyber liability $1–5M depending on data sensitivity.
- **Regulated data overlays.** If the application handles PHI, cardholder data, financial-institution customer data, children's data, or material non-public information of a public company, the data-handling sections in documents 01 and 03b need to be tightened and a BAA, PCI scoping, or other framework-specific document attached.
- **Source IPs and WAF/CDN allowlisting.** Sections 8 of document 02 — the tester provides IPs; you allowlist them at every WAF, CDN, and DDoS-shield layer in front of the target.
- **Test accounts.** Section 9 of document 02 — create dedicated `pentest-*` accounts in both the application and the provider's IAM, deliver credentials out of band.

## Common multi-provider patterns

- **App on PaaS + Cloudflare in front:** PaaS provider (Railway/Render/Heroku/Fly) plus Cloudflare both in scope. Allowlist tester IPs at Cloudflare regardless of the PaaS rules. Heroku additionally requires notification.
- **App on AWS + Stripe + Auth0:** AWS testing fine without approval; Stripe and Auth0 endpoints are out of scope (test only the integration code, not their APIs).
- **App on AWS with CloudFront:** Both within AWS — single policy applies, but CloudFront-specific WAF rules may need to be loosened.
- **App on DigitalOcean:** **You need their written consent before testing.** Open a ticket, send 03a Letter 1B, wait.
