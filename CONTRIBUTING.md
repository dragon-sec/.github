# Contributing to dragon-sec projects

This is the organization-wide default. Individual repositories may add their own
`CONTRIBUTING.md` covering toolchain setup and test commands — when they do,
that file wins and this one describes only the parts that apply everywhere.

## Developer Certificate of Origin

Every commit merged into a dragon-sec repository must carry a `Signed-off-by`
trailer. There is no CLA to sign and no account to create; the trailer *is* the
agreement.

Signing off certifies that you wrote the patch, or otherwise have the right to
submit it under the repository's license. The full text is the
[Developer Certificate of Origin 1.1](https://developercertificate.org/),
reproduced at the bottom of this file.

Add the trailer with `-s`:

```sh
git commit -s -m "fix(token): reject a refresh token replayed after rotation"
```

which appends a trailer built from your own `user.name` and `user.email`. Those
must match your git config, and the email must be one attached to your GitHub
account — anonymous or `noreply` addresses will fail the check.

Two things enforce this, and they cover different holes:

- A **DCO status check** runs on every pull request and blocks the merge until
  every commit in the branch is signed off.
- A **`require-dco` ruleset** enforces the same trailer at the ref, on every
  branch. The status check only ever sees pull requests; the ruleset is what
  covers a direct push.

### Signing off automatically

Git has no `commit.signoff` config, so use a hook. `git commit -s` on every
commit works; a `prepare-commit-msg` hook that appends the trailer when it is
missing works better, because it cannot be forgotten. Derive the name and email
from `git config` inside the hook rather than hard-coding them, so the same hook
is correct in every clone and for every contributor.

Note that `core.hooksPath` is global — if a repository ships its own hooks, set
`core.hooksPath` locally in that clone instead and copy your hook alongside the
repository's own.

### Fixing a branch you forgot to sign

For the most recent commit:

```sh
git commit --amend -s --no-edit && git push --force-with-lease
```

For every commit on the branch:

```sh
git rebase --signoff main && git push --force-with-lease
```

Always `--force-with-lease` rather than `--force`; it refuses to overwrite work
you haven't seen.

## Pull requests

Changes reach `main` through a pull request, merged by squash or rebase. A
ruleset blocks force-pushes to and deletion of the default branch. A repository
still in its initial construction may accept direct pushes from maintainers
until its ruleset adds the pull request requirement; the ruleset is the source
of truth for which repositories that is.

- Branch from `main`. Naming: `<type>/<short-slug>`, e.g. `feat/tenant-audit-export`.
- Rebase onto `main` before opening the PR — squash merges keep the log linear.
- Commit messages follow [Conventional Commits](https://www.conventionalcommits.org/):
  `feat`, `fix`, `chore`, `test`, `ci`, `docs`, `refactor`. Subject under 72 chars.
- Explain the *why* in the body. The diff already shows the *what*.
- Resolve every review thread before merging.

## Required checks

Two checks decide whether a pull request can merge, and they answer different
questions.

**`gate` — does it build and pass its tests?** One aggregate context rather than
a list of individual checks, so steps can be added inside it without editing
every repository's ruleset. For a Go service, with or without an embedded Vite
UI, it is the [`go-gate`](.github/actions/go-gate) action here: UI install, code
generation, UI build and lint, then `gofmt`, `go mod tidy`, `go vet`, build and
test. A repository adds `gate` to its ruleset once it has a CI job that runs it.

**`DragonGuard` — is it safe to merge?**
[DragonGuard](https://github.com/DragonSecurity/dragonguard), through the
DragonSecurity CI app, scans each pull request for committed secrets, vulnerable
dependencies, disallowed licences and insecure code patterns, and fails the check
when the repository's policy says the finding blocks. Rulesets pin the check to
that app, so a status of the same name posted by anything else does not satisfy
it. Run the same scan before you push with `dragon scan`.

If a required check is red, the PR does not merge — including Renovate's. That
is the point: the DCO check passes as soon as the trailer is present, so on its
own it gates nothing.

## Dependency updates

Renovate manages dependency bumps org-wide from the shared preset in
[`DragonSecurity/renovate-presets`](https://github.com/DragonSecurity/renovate-presets).
Its commits are signed off automatically. Please don't open manual version-bump
PRs — adjust the preset instead so the change applies everywhere.

CI actions are pinned by commit SHA, with the version in a trailing comment.
Renovate reads the comment and updates both. Do not replace a SHA with a tag.

## Security issues

Do not open a public issue for a vulnerability. See [SECURITY.md](SECURITY.md).

---

## Developer Certificate of Origin 1.1

```
By making a contribution to this project, I certify that:

(a) The contribution was created in whole or in part by me and I
    have the right to submit it under the open source license
    indicated in the file; or

(b) The contribution is based upon previous work that, to the best
    of my knowledge, is covered under an appropriate open source
    license and I have the right under that license to submit that
    work with modifications, whether created in whole or in part
    by me, under the same open source license (unless I am
    permitted to submit under a different license), as indicated
    in the file; or

(c) The contribution was provided directly to me by some other
    person who certified (a), (b) or (c) and I have not modified
    it.

(d) I understand and agree that this project and the contribution
    are public and that a record of the contribution (including all
    personal information I submit with it, including my sign-off) is
    maintained indefinitely and may be redistributed consistent with
    this project or the open source license(s) involved.
```
