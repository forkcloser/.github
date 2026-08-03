# Forkcloser

Friendly forks of upstream projects.

When an upstream project we depend on is unmaintained, slow to move, or simply missing
something we need, we fork it here rather than vendoring a patch and forgetting about it.
The fork stays honest about what it is: upstream keeps the credit, the licence stays
upstream's, and every change is one we would happily see merged back.

**If you can get a fix upstream, do that first.** These forks exist because that was not
possible or not timely — not because we think we own the code.

## Forks

- [**xz**](https://github.com/forkcloser/xz) — a fork of
  [ulikunitz/xz](https://github.com/ulikunitz/xz) with substantial decoding performance work:
  roughly double the serial decode speed, and about 20× on multiblock archives via parallel
  block decoding. Upstream appears inactive; the changes are deliberately kept in a shape
  that could be carried over if anyone has the time.
- [**grid-clock-screensaver**](https://github.com/forkcloser/grid-clock-screensaver) — a fork
  of [chrstphrknwtn/grid-clock-screensaver](https://github.com/chrstphrknwtn/grid-clock-screensaver),
  a word-clock screensaver for macOS, ported to modern macOS.
- [**erofs**](https://github.com/forkcloser/erofs) — a fork of
  [dmcgowan/go-erofs](https://github.com/dmcgowan/go-erofs): a pure-Go library for reading and
  creating [EROFS](https://erofs.docs.kernel.org/en/latest/) filesystem images through the
  standard `fs.FS` interface, no CGO. Our work here is mostly hardening — bounding
  allocations driven by untrusted image fields — plus writer and metadata additions.

## Contributing & support

Projects here are provided as-is, best-effort, without warranty.

- [Contributing guide](https://github.com/forkcloser/.github/blob/main/.github/CONTRIBUTING.md)
  — upstream first, then sign-off (DCO), commit signing and the pull request flow.
- [Security policy](https://github.com/forkcloser/.github/blob/main/.github/SECURITY.md) —
  **never** report a vulnerability in a public issue, and note that a flaw in forked code is
  usually upstream's to fix.
- Bugs and ideas go to the issue tracker of the repository they concern.

The sibling organization [farcloser](https://github.com/farcloser) holds the tooling and
libraries these are built with.
