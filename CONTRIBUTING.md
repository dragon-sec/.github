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
git commit -s -m "fix(profile): keep the cargo registry read-only"
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

`main` takes no direct pushes. A ruleset requires a pull request, allows only
squash and rebase merges, and blocks force-pushes and deletion of the default
branch. That applies to maintainers too.

- Branch from `main`. Naming: `<type>/<short-slug>`, e.g. `feat/profile-schema`.
- Rebase onto `main` before opening the PR — squash merges keep the log linear.
- Commit messages follow [Conventional Commits](https://www.conventionalcommits.org/):
  `feat`, `fix`, `chore`, `test`, `ci`, `docs`, `refactor`. Subject under 72 chars.
- Explain the *why* in the body. The diff already shows the *what*.
- Resolve every review thread before merging; the ruleset requires it.

## The merge gate

Each repository runs one required check named **`gate`**. It is a single
aggregate context rather than a list of individual checks, so steps can be added
inside it without editing every repository's ruleset. What it runs for a Rust
repository lives in [`.github/actions/rust-gate`](.github/actions/rust-gate)
here, and today that is format, lint (`-D warnings`), build and test.

If `gate` is red, the PR does not merge — including Renovate's. That is the
point of it: the DCO check passes as soon as the trailer is present, so on its
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
