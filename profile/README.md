# Forkcloser

![A fork, on a table](fork.jpg)

Friendly forks of upstream projects.

When an upstream project we depend on is unmaintained, slow to move, or simply missing
something we need, we fork it here rather than vendoring a patch and forgetting about it.
The fork stays honest about what it is: upstream keeps the credit, the licence stays
upstream's, and what differs from upstream is written down, change by change. Not every
change is upstream's to take: some align a fork with our own lint baseline or replace a
library, and those are ours to carry.

**If you can get a fix upstream, do that first.** These forks exist because that was not
possible or not timely — not because we think we own the code.

## Forks

Upstream's code with changes of ours. Each repository's `UPSTREAM.md` records the commit
it forked from, the upstream commit it was last reviewed against, what was taken from
upstream since, and every fix and feature upstream does not have.

- [**blake3**](https://github.com/forkcloser/blake3) — a fork of
  [lukechampine/blake3](https://github.com/lukechampine/blake3), a Go implementation of the
  BLAKE3 hash with AVX2/AVX-512 routines and the `bao` verified-streaming encoding. The
  changes come out of an audit of the tree: an XOF `Seek` to almost any buffer-unaligned
  offset returned output from the wrong position (latent since 2020), `bao` slice bounds
  could overflow past validation and the empty encoding was rejected, `New` now validates
  its key and size instead of panicking deep inside, and the allocation and parallelism
  behaviour is tuned so small XOF reads and streaming writes stop paying for work they do
  not use.
- [**erofs**](https://github.com/forkcloser/erofs) — a fork of
  [erofs/go-erofs](https://github.com/erofs/go-erofs): a pure-Go library for reading and
  creating [EROFS](https://erofs.docs.kernel.org/en/latest/) filesystem images through the
  standard `fs.FS` interface, no CGO. The work here is mostly about surviving images we did
  not write — allocations bounded by what the image can physically hold, cyclic directory
  graphs and oversized directory and symlink sizes rejected — alongside correctness fixes
  (`fs.FS` contract conformance, chunk extents mapped at block granularity with holes
  preserved, setuid/setgid/sticky bits kept on both the read and write paths) and additions
  to the writer.
- [**git-validation**](https://github.com/forkcloser/git-validation) — a fork of
  [vbatts/git-validation](https://github.com/vbatts/git-validation), which checks a range
  of commits for a DCO sign-off, a subject under 90 characters and no trailing whitespace.
  Here it is released as binaries, with its dependencies trimmed and its checks unchanged;
  long flags take two dashes (`--run`, `--range`).
- [**go-graphviz**](https://github.com/forkcloser/go-graphviz) — a fork of
  [goccy/go-graphviz](https://github.com/goccy/go-graphviz): Go bindings for Graphviz with
  the real Graphviz compiled to WebAssembly and run on wazero, no C toolchain and no system
  Graphviz. The changes are about trusting the blob: Graphviz and expat are built from
  pinned, digest-checked sources with a pinned wasi-sdk and binaryen, reproducibly, so two
  builders produce the same bytes and CI proves it on every change; the bridge's failures
  are typed errors a caller can match; the `dot` binary of upstream's nested module is
  replaced by [dot](https://github.com/forkcloser/dot) below.
- [**go-licenses**](https://github.com/forkcloser/go-licenses) — a fork of
  [google/go-licenses](https://github.com/google/go-licenses), which reports and checks
  the licences of a Go package's dependency tree. The fork drops the `report` and `csv`
  commands and the vendored pkgsite behind them, so the tool makes no HTTP request, vendors
  the licence classifier without its tracing, recognises vendored modules on Windows, and
  is released as binaries.
- [**grid-clock-screensaver**](https://github.com/forkcloser/grid-clock-screensaver) — a fork
  of [chrstphrknwtn/grid-clock-screensaver](https://github.com/chrstphrknwtn/grid-clock-screensaver),
  a word-clock screensaver for macOS, ported to modern macOS.
- [**vz**](https://github.com/forkcloser/vz) — a fork of
  [Code-Hex/vz](https://github.com/Code-Hex/vz), the Go binding for Apple's
  Virtualization.framework, for the hosts ossein and zerogravity run on: Apple silicon,
  the current macOS, darwin only. The fork gives each virtual machine its own vmnet
  network (upstream's unmerged pull request 205), a virtio socket that closes without
  deadlocking and can half-close, a build that fails on the wrong SDK instead of booting
  a VM with no network, the macOS-guest provisioning bindings upstream does not have yet,
  and only the devices its consumers call. Each change with its reason is in `FORK.md`.
- [**xz**](https://github.com/forkcloser/xz) — a fork of
  [ulikunitz/xz](https://github.com/ulikunitz/xz), in two passes. First decoding speed: the
  per-operation allocations removed, a branchless range decoder, match copies by pattern
  doubling, and a `ParallelReader` that decodes the blocks of a multiblock archive
  concurrently. Then robustness against archives that were built to break a decoder —
  impossible sizes rejected, the stream index read incrementally and bounded so the
  backwards walk terminates, the dictionary grown on demand rather than up front, and
  `Close` able to cancel a read already blocked in a worker. Upstream's releases are
  reviewed as they come and taken where they apply; the command built on the library is
  [compress](https://github.com/forkcloser/compress)'s `xz`.
- [**yamlfmt**](https://github.com/forkcloser/yamlfmt) — a fork of
  [google/yamlfmt](https://github.com/google/yamlfmt), a YAML formatter as a command line
  and a library: LF line endings by default on every platform, `exclude` matching paths
  rather than strings, line comments kept at their source column, and a run of fixes to
  what retaining line breaks does to block, folded and multi-line scalars.

## Packagings

Upstream code, our packaging: nothing rewritten, nothing forked, only built or
imported at an audited pin and published so the fleet can pin it by checksum
and attestation.

- [**curl**](https://github.com/forkcloser/curl) — one `curl` with dependable
  TLS 1.3 for every platform, from [curl-for-win](https://github.com/curl/curl-for-win),
  the curl project's own reproducible-build system: the Windows packages are
  the official builds, imported and sigstore-verified against a vendored key;
  linux (static musl) and macOS (arm64) are built by curl-for-win's scripts at
  the same commit pin. Exists because no single upstream channel covers all
  the platforms the toolchain runs on.
- [**openssh**](https://github.com/forkcloser/openssh) — OpenSSH for macOS with
  security keys built in, which Apple's build lacks: OpenSSH portable built
  against LibreSSL's libcrypto, libfido2 and libcbor linked statically, Apple's
  clang, one relocatable package per release. A small patch makes the binaries
  find their configuration and helpers beside themselves, so the package runs
  from wherever aqua unpacks it; it ships a strict client configuration, the
  agent as a user LaunchAgent, and the server off by default, user-scoped when
  on.

## Own projects

Not forks: our own code, our own licence. Two follow the interface of a project that could
not be consumed as pinned, versioned tooling, with credit to it; the rest exist because
nothing upstream did the job.

- [**compress**](https://github.com/forkcloser/compress) — compression and archiving in
  pure Go: a streaming library for zstd, gzip, xz, lz4 and bzip2, and the `tar`, `xz`,
  `gzip`, `zstd`, `zip` and `unzip` commands built on it, released for every platform the
  toolchain runs on. Exists because builds took those commands from whatever the host had,
  none of it pinned.
- [**dot**](https://github.com/forkcloser/dot) — a Graphviz `dot` command as one static Go
  binary, on [go-graphviz](https://github.com/forkcloser/go-graphviz)'s WebAssembly build of
  Graphviz: `-K`, `-T` (dot, svg, png, jpg) and `-o`, file or standard input, file or
  standard output. goccy/go-graphviz's own `dot` lives in a nested module that has never
  been tagged and pins the library five releases back, so a tool directive on it cannot
  move; this one is tagged and tracks the library like any other dependency.
- [**godolint**](https://github.com/forkcloser/godolint) — a Dockerfile linter in Go that
  follows [hadolint](https://github.com/hadolint/hadolint), rule for rule: the rule
  metadata and the test cases are generated from hadolint's Haskell sources at a pinned
  release, so a hadolint bump shows exactly which rules moved, and the pragmas, the shell
  analysis of `RUN` lines and the optional shellcheck integration are ported to match. One
  static binary and an SDK, where hadolint needs a Haskell toolchain to build and a
  container to run. Site: [godolint.forkcloser.org](https://godolint.forkcloser.org/).
- [**hermetic**](https://github.com/forkcloser/hermetic) — a sealed aqua for the machine
  toolset: aqua as a Go library behind one binary, with a root, a project directory and a
  `bin` of its own, so the pinned tools never depend on the shell's aqua.
- [**taciturn**](https://github.com/forkcloser/taciturn) — an ssh-agent for keys in the
  Mac's Secure Enclave, with a per-key policy and an audit log, for unattended sessions:
  the key cannot leave the Mac, it signs only for the hosts, users and namespaces its
  policy names, and every request is recorded. Site:
  [taciturn.forkcloser.org](https://taciturn.forkcloser.org/).
- [**xos**](https://github.com/forkcloser/xos) — the standard library's `os` with
  `FILE_SHARE_DELETE` on Windows: files another process can rename or delete while this
  one holds them, and a rename that replaces a file readers hold open; call for call the
  same otherwise.

## Sites and infrastructure

The organisation's own, not forks: what serves and what provisions.

- [**website**](https://github.com/forkcloser/website) — the repository where
  forkcloser.org, the organisation's own page, is being built; empty until it lands.
- [**website-godolint**](https://github.com/forkcloser/website-godolint) — the website
  for godolint at [godolint.forkcloser.org](https://godolint.forkcloser.org/): two static
  pages, no build step, no third-party requests, the sample Dockerfile on the landing page
  annotated by the pinned `godolint` itself.
- [**website-taciturn**](https://github.com/forkcloser/website-taciturn) — the website for
  taciturn at [taciturn.forkcloser.org](https://taciturn.forkcloser.org/): one static page,
  no build step, every command on it tested against the pinned release.
- [**infra**](https://github.com/forkcloser/infra) — the infrastructure behind the
  websites, as code: one zone per domain, one block per site; OpenTofu holds the
  Cloudflare Pages projects and their domains, and `dnsctl`, a Go program in the
  repository, makes the DNS and registrar accounts match `zones.yaml`, delegation and
  DNSSEC included. A pull request adds a site; the apply on `main` creates it.

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
