# Security Policy

## Reporting a vulnerability

Please do not report security vulnerabilities through public GitHub issues,
discussions, or pull requests.

Report privately through **GitHub Security Advisories** — open the affected
repository, go to the **Security** tab, and choose **Report a vulnerability**.
This creates a private advisory visible only to you and the maintainers.

If the repository has advisories disabled, or you would rather not use GitHub,
email **alanis@dragondev.cc**.

Please include:

- The product, and the version, commit, or release tag affected.
- A description of the issue and its impact.
- Steps to reproduce, ideally a minimal proof of concept.
- Any suggested remediation, if you have one.

## What to expect

- **Acknowledgement** within 3 working days.
- An initial assessment, including whether we accept the report and a rough
  severity, within 10 working days.
- Progress updates at least every 14 days while we work on a fix.
- Credit in the advisory and release notes, unless you ask otherwise.

We ask that you give us a reasonable opportunity to ship a fix before disclosing
publicly. We aim to publish an advisory within 90 days of the report, sooner
where the fix is straightforward.

## What we are most interested in

These products hold the credentials for other people's infrastructure and the
controls that change it, so the failures that matter most are the ones where a
boundary does not hold:

- **Tenant isolation** — one tenant reading, changing, or authenticating as
  anything that belongs to another: users, sessions, tokens, signing keys, audit
  records, or the infrastructure a tenant manages.
- **Authentication bypass** — obtaining a session or token without the
  credentials and factors the policy requires. That includes flaws in the
  authorization code and PKCE flows, a refresh token that still works after
  rotation, and accepting a token whose signature, issuer, audience or expiry
  should have been rejected.
- **Privilege escalation** — a user, API token or integration acting beyond the
  roles and scopes it was granted, including through an admin API or
  impersonation.
- **Secret exposure** — recovering stored secrets, tenant connection details or
  signing keys, or causing one to appear in a log, error message or response.
- **Unapproved infrastructure change** — getting a product to apply a firewall
  rule, cluster change or deployment that skipped the approval or policy it
  should have passed.
- **Audit evasion** — performing a sensitive action without the audit record
  that should exist, or altering or removing one that does.
- **Release integrity** — a published artifact whose checksum or provenance does
  not match the source it claims to be built from.

A report in one of these classes is in scope even if exploiting it requires an
account. A low-privilege user in another tenant is the threat model, not an
excuse.

## Scope

This policy covers the source code and published releases of dragon-sec
repositories. Test against an instance you run yourself; findings from testing
someone else's deployment without their permission are out of scope, and so is
that testing. Findings against third-party dependencies should go to that
project's maintainers — though we appreciate a heads-up so we can pin or patch.

Out of scope: reports generated solely by automated scanners with no
demonstrated impact, social engineering, physical attacks, and denial of service
through sheer volume of traffic.

## Safe harbour

We will not pursue or support legal action against anyone who makes a good-faith
effort to comply with this policy. If a third party brings action against you
for research conducted in line with it, we will make that good faith known.
