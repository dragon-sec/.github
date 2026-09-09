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

- The repository and version, commit, or release tag affected.
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

These projects exist to confine other people's code, so the failures that matter
most are the ones where that confinement does not hold:

- **Sandbox escape** — a language server reaching a path, environment variable,
  socket or process outside the capability profile it was granted.
- **Profile bypass** — any input, workspace layout or LSP message that causes a
  server to be started with a profile other than the one that was resolved for
  it, or with none.
- **Verification bypass** — accepting a server binary whose provenance or hash
  does not match what was vetted, including downgrade and substitution.
- **Confused deputy** — persuading a trusted component to act on an untrusted
  workspace's behalf, for example a workspace file that changes what the shim
  executes.

A report in one of these classes is in scope even if exploiting it requires the
user to open a hostile repository. Opening a hostile repository is the threat
model, not an excuse.

## Scope

This policy covers the source code in dragon-sec repositories. Findings against
third-party dependencies should go to that project's maintainers — though we
appreciate a heads-up so we can pin or patch. Findings against a *language
server we ship a profile for* are in scope for us to the extent the profile
fails to contain them; the underlying bug still belongs upstream.

Out of scope: reports generated solely by automated scanners with no
demonstrated impact, social engineering, physical attacks, and denial of service
through sheer volume of traffic.

## Safe harbour

We will not pursue or support legal action against anyone who makes a good-faith
effort to comply with this policy. If a third party brings action against you
for research conducted in line with it, we will make that good faith known.
