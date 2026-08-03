# Security

## Reporting a vulnerability

**Do not open a public issue for a security vulnerability.**

Most repositories here are **forks**, so the code you found the flaw in may not be ours.
Which it is changes who can actually fix it, and how many people benefit.

### If the flaw is in the upstream project

Report it **upstream first**, through their security policy. Their fix protects every user of
the original, not just ours, and we would far rather carry their patch than ship a divergent
one.

Tell us as well once you have — a private advisory here is enough — so we can track it and
pull the fix in. If upstream is unresponsive, say so, and we will treat it as ours.

### If the flaw is in our changes, or in code that is ours

Use GitHub private vulnerability reporting on the affected repository:

1. Go to the **Security** tab.
2. **Report a vulnerability** — or go straight to
   `https://github.com/forkcloser/<repository>/security/advisories/new`.

That opens a private advisory visible only to you and the maintainers. It keeps the report
confidential until a fix exists, and it is where the advisory — and a CVE, if one is
warranted — is published from.

If the repository has no Security tab (it should — that would be an error on our side),
report it privately against
[`forkcloser/erofs`](https://github.com/forkcloser/erofs/security/advisories/new) instead and
say which repository you meant.

**Not sure which it is?** Report it to us and say so. Working out whether a flaw predates our
fork is our job, not yours — never let that uncertainty turn into a public issue.

## What to expect

An acknowledgement, and a fix on a **best-effort** basis.

There is no service-level agreement and no warranty of a timely patch — these projects are
provided as-is, see [SUPPORT.md](./SUPPORT.md). We will credit you in the advisory unless you
ask us not to.

Where the flaw is inherited from upstream, our fix is usually to take theirs, so expect our
timeline to track the upstream one.

## Scope

In scope: the code in this organization's repositories, including the changes we have made on
top of upstream.

Out of scope: findings against third-party dependencies — report those to their maintainers,
though we do want to hear about it if we are shipping a vulnerable pin — and reports that
consist only of an automated scanner's output with no demonstrated impact.
