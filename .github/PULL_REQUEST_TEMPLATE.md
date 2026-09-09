<!--
Thanks for the contribution. A few notes before you submit:

  * Every commit needs a sign-off trailer (`git commit -s`). Both the DCO
    check and the require-dco ruleset will block the merge otherwise.
    See CONTRIBUTING.md.
  * Rebase onto main and keep commit subjects in Conventional Commits form.
-->

## What this changes

<!-- One or two sentences. What behaviour is different after this merges? -->

## Why

<!-- The motivation. Link the issue if there is one: Fixes #123 -->

## How it was tested

<!-- Commands you ran, cases you covered, or why tests aren't applicable. -->

## Security impact

<!--
Delete this section only if the change cannot affect confinement. Otherwise say
what it does to the trust boundary: does it widen a capability profile, change
what is executed, change how a binary is verified, or add a dependency that runs
at build time? "Widens nothing" is a fine answer — write it down.
-->

## Checklist

- [ ] All commits are signed off (`git commit -s`)
- [ ] Commit subjects follow Conventional Commits
- [ ] `gate` is green
- [ ] Tests added or updated for the behaviour that changed
- [ ] Documentation updated if behaviour or configuration changed
- [ ] No secrets, credentials, or customer data in the diff
