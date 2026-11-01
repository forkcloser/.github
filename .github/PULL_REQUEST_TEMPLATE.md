<!--
Thanks for the pull request.

The title is the release note: it says what lands, for someone reading the release page.
The description is read by someone who was not in the review, a year on: what changed and
why in the terms of the code, every reference resolving from the page alone. Leave out a
section with nothing to say.
-->

🎯 **Why**

<!-- The defect, the gap or the requirement, in the system's terms, and what it costs. -->

🛠️ **What**

<!-- What changes, by file, package or component, as a reader checks it against the diff. -->

🧪 **How**

<!--
How the change answers the why, and how that was verified: the test, the command, the
document. "just lint && just test green" is a fine answer; "should work" is not.
-->

💡 **Follow-ups**

<!-- What this leaves for later, and why it was not done here. -->

---

<!--
The links below are absolute on purpose. This template is COPIED into the pull
request body of whatever repository inherits it, so a relative link would
resolve against that repository — which does not carry these files, since
inheriting them is the whole point.
-->

- [ ] Commits are signed off (`git commit -s`) — see
      [CONTRIBUTING.md](https://github.com/forkcloser/.github/blob/main/.github/CONTRIBUTING.md)
- [ ] Commits are cryptographically signed (`commit.gpgsign`), and my key is in
      `.lint-signers`
- [ ] CI is green
- [ ] Branch is rebased on `main` (no merge commits)
- [ ] This is not a security fix — those go through
      [private vulnerability reporting](https://github.com/forkcloser/.github/blob/main/.github/SECURITY.md),
      never a public PR
