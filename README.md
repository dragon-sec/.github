# .github

Organization-wide default community health files for
[dragon-sec](https://github.com/dragon-sec), plus the shared CI action every
repository here calls.

GitHub falls back to the files here for any repository in the organization that
does not ship its own copy. A repository's own file always wins.

| File | Applies to |
| --- | --- |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | Contribution process, sign-off, and how the merge gate works |
| [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) | Contributor Covenant 2.1 |
| [`SECURITY.md`](SECURITY.md) | Vulnerability reporting and disclosure |
| [`.github/PULL_REQUEST_TEMPLATE.md`](.github/PULL_REQUEST_TEMPLATE.md) | Default PR template |
| [`.github/ISSUE_TEMPLATE/`](.github/ISSUE_TEMPLATE) | Default issue forms |
| [`profile/README.md`](profile/README.md) | The organization landing page |

Licensing is not a community health file — GitHub cannot apply one org-wide, so
each repository carries its own `LICENSE`. Everything here is Apache-2.0.

## The `rust-gate` action

[`.github/actions/rust-gate`](.github/actions/rust-gate) is a composite action
running format, lint, build and test for a Rust workspace. Repositories call it
from a job named exactly `gate`:

```yaml
jobs:
  gate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1
      - uses: dragon-sec/.github/.github/actions/rust-gate@main
```

The job name matters. Each repository's `require-checks-on-default-branch`
ruleset requires a single status context named `gate`, so that steps can be
added inside the action without every repository's ruleset needing an edit. A
*composite action* rather than a reusable workflow for the same reason: a
reusable workflow reports as `gate / <inner job>`, which would not match.

Actions are pinned by commit SHA with the version in a trailing comment.
Renovate updates them. An organization whose product is supply-chain trust does
not pin CI by mutable tag.

This repository is public because GitHub only applies default community health
files from a public `.github` repository. It contains no code beyond the action
above, and nothing sensitive.
