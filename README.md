# .github

Organization-wide default community health files for
[dragon-sec](https://github.com/dragon-sec), plus the shared CI action its
repositories call.

GitHub falls back to the files here for any repository in the organization that
does not ship its own copy. A repository's own file always wins.

| File | Applies to |
| --- | --- |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | Contribution process, sign-off, and the required checks |
| [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) | Contributor Covenant 2.1 |
| [`SECURITY.md`](SECURITY.md) | Vulnerability reporting and disclosure |
| [`.github/PULL_REQUEST_TEMPLATE.md`](.github/PULL_REQUEST_TEMPLATE.md) | Default PR template |
| [`.github/ISSUE_TEMPLATE/`](.github/ISSUE_TEMPLATE) | Default issue forms |
| [`profile/README.md`](profile/README.md) | The organization landing page |

Licensing is not a community health file — GitHub cannot apply one org-wide, so
each repository carries its own `LICENSE`. Everything here is Apache-2.0.

## The `go-gate` action

[`.github/actions/go-gate`](.github/actions/go-gate) is a composite action that
builds, lints and tests a Go service, and the Vite UI it embeds when it has one.
Repositories call it from a job named exactly `gate`:

```yaml
jobs:
  gate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1
      - uses: dragon-sec/.github/.github/actions/go-gate@main
        with:
          ui-directory: ui
          generate: go run . generate sdk --ui-only
```

In order, it installs the UI's dependencies, runs `generate`, builds and lints
the UI, then checks `gofmt`, `go mod tidy`, `go vet`, `go build` and `go test`.
The UI is built before the Go steps because the binary embeds it, so the tests
run against what a release ships. Leave `ui-directory` and `generate` out for a
service with neither.

The job name matters. A repository's ruleset requires a single status context
named `gate`, so that steps can be added inside the action without every
repository's ruleset needing an edit. A *composite action* rather than a
reusable workflow for the same reason: a reusable workflow reports as
`gate / <inner job>`, which would not match. It also means anything the tests
need — a PostgreSQL service container, the environment variables that point
tests at it — is declared on the calling job, where the action's steps can see
it.

Actions are pinned by commit SHA with the version in a trailing comment.
Renovate updates them. Security tooling should not run CI pinned to a tag
anyone can move.

This repository is public because GitHub only applies default community health
files from a public `.github` repository. It contains no code beyond the action
above, and nothing sensitive.
