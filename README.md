# .github

This is the provisioning profile for the Forkcloser organization on GitHub — the home of the
friendly forks we maintain.

It contains:

- `profile/README.md` — the organization profile page shown on https://github.com/forkcloser
- `.github/ISSUE_TEMPLATE/` — shared issue templates (bug, feature request, question) and their config
- `.github/PULL_REQUEST_TEMPLATE.md` — the default pull request body
- community health files applied org-wide:
  - `CODE_OF_CONDUCT.md`
  - `CONTRIBUTING.md` — upstream-first, then the DCO terms, mandatory commit signing, and commit rules
  - `GOVERNANCE.md`
  - `SECURITY.md` — where a flaw goes when the code is upstream's, and private vulnerability reporting when it is ours
  - `SUPPORT.md`
  - `FUNDING.yml`

These defaults apply to every repository in the organization that does not provide its own.
GitHub only resolves them from a **public** `.github` repository, and only from the root,
`.github/` or `docs/`.

## What is different from farcloser

This repository is a port of [`farcloser/.github`](https://github.com/farcloser/.github), and
the machinery is deliberately identical. The *content* differs wherever being a fork
organization changes the answer:

- **`CONTRIBUTING.md` leads with "does this belong upstream?"** A change that lands upstream
  is one we eventually delete from the fork, so it is the outcome we want.
- **`SECURITY.md` splits by whose code it is.** A flaw inherited from upstream goes upstream
  first — their fix protects everyone, and we would rather take their patch than ship a
  divergent one. A flaw in our changes uses private vulnerability reporting here.
- **The bug template asks whether the bug reproduces upstream.** It is the single most useful
  triage question in a fork.
- **`CONTRIBUTING.md` does not claim every repository runs `just`.** Only enrolled
  repositories do; a fork keeps upstream's build precisely so patches stay easy to send back.

## Caveats worth knowing

- The org profile README is read from `profile/README.md` only — that path is not
  configurable and has no fallback.
- Issue templates are inherited **all or nothing**: GitHub's own wording is that if a
  repository has any files in its own `.github/ISSUE_TEMPLATE` folder, *none* of the
  contents of the default folder are used.
- `dependabot.yml`, `CODEOWNERS` and workflows are **not** inherited org-wide. Renovate
  covers dependency updates; `limen` distributes the canonical workflows by pinning them
  into each repository.
- A default `LICENSE` cannot be inherited either — the one here covers this repository only.
  Forks carry upstream's licence, which is not ours to change.

## Working on it

This repository is [`limen`](https://github.com/farcloser/limen)-enrolled, so the interface is
the usual one:

```
just lint      # limen, just, aqua, links, yaml, shell, dockerfile, commits, issue-forms
just fix
```

`just do lint <recipe>` runs a single check. The toolchain is pinned through aqua and
installed on first use; nothing needs to be set up by hand. Note that most *other*
repositories in this organization are not enrolled — see `CONTRIBUTING.md` for why.

### issue-forms

The one project-specific recipe. `do lint yaml` proves the templates parse and are
formatted, and `do lint links` (lychee) proves the links resolve — but neither can tell
that a well-formed YAML file is a *valid issue form*. GitHub fails that silently: it does
not reject the file, it just refuses to render the form. Since these templates are the
org-wide fallback, a broken one breaks issue creation in every repository that has none of
its own. `.github/workflows/validate-issue-forms.sh` checks them against the form schema —
shell plus the aqua-pinned `yq`, so it introduces no interpreter the toolchain does not
already pin, and `do lint shell` lints it like any other script we ship.

### If you ever list a private repository

`profile/README.md` currently lists only public repositories, so there is no root
`.lychee.toml`. If a private one is ever added, mark it 🔒 in the profile *and* exclude it in
a new root `.lychee.toml`: lychee runs unauthenticated — that is the point, it sees what an
anonymous visitor sees — so the link would otherwise 404 and fail the run. Only ever exclude
a repository that is private **on purpose**; an entry there silences a real check.
