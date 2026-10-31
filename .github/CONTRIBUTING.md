# Contributing

Got ideas? Found a bug you can fix? Go for it.

## First: does this belong upstream?

Every repository here is a **fork**. Before opening a pull request, ask where the change
really belongs:

- **The bug or feature exists in the upstream project too** → send it upstream. It helps
  everyone using the original, not just us, and we will happily take their release instead of
  maintaining our own divergence. Tell us you did, and we will track it.
- **Upstream is unresponsive, or has declined it** → then it belongs here. Say so in the pull
  request; that context is exactly why the fork exists.
- **It only makes sense for our fork** — it depends on changes we have already made, or on how
  we use the project → it belongs here.

Each repository's README says what it is a fork of and why. This is not bureaucracy: a change
that lands upstream is a change we eventually delete from the fork, and that is a good day.

The rest of this document applies once you have decided the change belongs here. Everything
below is **mechanically enforced** — CI or a branch ruleset will reject the pull request
otherwise. Reading it first saves you a round trip.

## Sign your commits off (DCO)

Every commit must carry a `Signed-off-by` trailer. Git writes it for you:

```
git commit -s
```

Forgot? Fix the last commit with `git commit -s --amend`, or a whole branch with
`git rebase --signoff origin/main`.

The trailer must match the author identity of the commit, and it is a real statement —
by adding it you certify the [Developer Certificate of Origin 1.1](https://developercertificate.org/):

```
Developer's Certificate of Origin 1.1

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

Edits made through the GitHub web UI are signed off automatically — the organization
requires it.

## Commit messages

Three rules are checked, per commit, on the range your pull request adds:

| Rule | What it means |
|------|---------------|
| `DCO` | The `Signed-off-by` trailer above. |
| `short-subject` | The subject line is **strictly under 90 characters** — GitHub ellipsizes there. Aim for 72 or less. |
| `dangling-whitespace` | No trailing whitespace anywhere in the message. |

Write the subject in the imperative ("Fix the checksum drift", not "Fixed" or "Fixes"), and
use the body to explain *why* — the diff already says what.

## Pull requests

- **Always a pull request.** Nobody pushes to `main`, including maintainers. The pull
  request is the audit trail and the CI gate, and it merges with one approval and green
  checks; an author cannot approve their own.
- **Your branch stays linear.** Rebase onto `main` rather than merging `main` into your
  branch: a pull request is read commit by commit, one commit per concern, and a merge
  commit in it says nothing. The maintainer merges the approved pull request with a merge
  commit; its title is the release note, so write it as one.
- **CI must be green.** Required status checks block the merge button. Do not ask for an
  exception; fix the check or explain why the check is wrong.
- **Force-pushing your own branch is fine** and often the right move after review; use
  `git push --force-with-lease`. Force-push and deletion are blocked on `main` and on the
  release tags (`v*`), nowhere else.
- **New to this?** [GIT.md](https://github.com/farcloser/.github/blob/main/.github/GIT.md)
  is the companion: amending, rebasing, editing a commit in the middle of a branch,
  force-pushing safely.

## Before you open it

Run the checks CI runs. Every repository here is governed by
[`limen`](https://github.com/farcloser/limen): `just lint` and `just test` run the same
pinned tooling CI runs, so green locally means green in CI, and `just --list` shows the rest.
The tools are pinned in the repository's `.hermetic/hermetic.yaml` and fetched on first use
by [hermetic](https://github.com/forkcloser/hermetic), which
[limen-install](https://github.com/farcloser/limen-install) installs; nothing else is set up
by hand.

If a check fails on code you did not touch, say so in the pull request; that is our bug, not
yours.

## Sign your commits (mandatory)

**Every commit must be cryptographically signed.** This is not the same thing as the
`Signed-off-by` trailer above — the DCO trailer is a statement of provenance you type, a
signature is proof the commit came from your key. We require both.

We sign with SSH keys, preferably backed by a hardware token (YubiKey and other `sk-`
keys); why one hardware-bound key for both authentication and signing, and what to do
when the token is lost, is the book's
[Identity and keys](https://github.com/farcloser/limen/blob/main/book/identity.md).
Configure git once:

```
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519_sk.pub
git config --global commit.gpgsign true
git config --global tag.gpgsign true
```

With `commit.gpgsign` set, every commit is signed without you thinking about it. To sign a
single commit explicitly, use `git commit -S -s`; to sign a branch you already wrote,
`git rebase --exec 'git commit --amend --no-edit -S' origin/main`.

Repositories ship a `.lint-signers` file mapping identities to public keys, so
`git log --show-signature` and `git tag -v` resolve locally. Add your key to it in the same
pull request as your first contribution:

```
you@example.com namespaces="git" sk-ssh-ed25519@openssh.com AAAA... you@example.com
```

Release tags are always signed — the release tooling refuses to reuse an unsigned tag.

## Security issues

Do not open a pull request or a public issue for a vulnerability. See
[SECURITY.md](./SECURITY.md) — use private vulnerability reporting.
