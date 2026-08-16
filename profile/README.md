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
  [ulikunitz/xz](https://github.com/ulikunitz/xz), in two passes. First decoding speed: the
  per-operation allocations removed, a branchless range decoder, match copies by pattern
  doubling, and a `ParallelReader` that decodes the blocks of a multiblock archive
  concurrently. Then robustness against archives that were built to break a decoder —
  impossible sizes rejected, the stream index read incrementally and bounded so the
  backwards walk terminates, the dictionary grown on demand rather than up front, and
  `Close` able to cancel a read already blocked in a worker. Upstream appears inactive; the
  changes are deliberately kept in a shape that could be carried over if anyone has the time.
- [**erofs**](https://github.com/forkcloser/erofs) — a fork of
  [erofs/go-erofs](https://github.com/erofs/go-erofs): a pure-Go library for reading and
  creating [EROFS](https://erofs.docs.kernel.org/en/latest/) filesystem images through the
  standard `fs.FS` interface, no CGO. The work here is mostly about surviving images we did
  not write — allocations bounded by what the image can physically hold, cyclic directory
  graphs and oversized directory and symlink sizes rejected — alongside correctness fixes
  (`fs.FS` contract conformance, chunk extents mapped at block granularity with holes
  preserved, setuid/setgid/sticky bits kept on both the read and write paths) and additions
  to the writer.
- [**blake3**](https://github.com/forkcloser/blake3) — a fork of
  [lukechampine/blake3](https://github.com/lukechampine/blake3), a Go implementation of the
  BLAKE3 hash with AVX2/AVX-512 routines and the `bao` verified-streaming encoding. The
  changes come out of an audit of the tree: an XOF `Seek` to almost any buffer-unaligned
  offset returned output from the wrong position (latent since 2020), `bao` slice bounds
  could overflow past validation and the empty encoding was rejected, `New` now validates
  its key and size instead of panicking deep inside, and the allocation and parallelism
  behaviour is tuned so small XOF reads and streaming writes stop paying for work they do
  not use.
- [**grid-clock-screensaver**](https://github.com/forkcloser/grid-clock-screensaver) — a fork
  of [chrstphrknwtn/grid-clock-screensaver](https://github.com/chrstphrknwtn/grid-clock-screensaver),
  a word-clock screensaver for macOS, ported to modern macOS.

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
