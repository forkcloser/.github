# Forkcloser

![A fork, on a table](fork.jpg)

Friendly forks of upstream projects.

When an upstream project we depend on is unmaintained, slow to move, or simply missing
something we need, we fork it here rather than vendoring a patch and forgetting about it.
The fork stays transparent about what it is: upstream is credited, the licence stays
upstream's, and what differs from upstream is written down, change by change. Not every
change is upstream's to take: some align a fork with our own lint baseline or replace a
library, and those are likely to stay here.

**If you can get a fix upstream, do that first.** These forks exist because that was not
possible or not timely.

## Forks

Upstream's code with changes of ours. Each repository's `UPSTREAM.md` records the commit
it forked from, the upstream commit it was last reviewed against, what was taken from
upstream since, and every fix and feature upstream does not have.

- [**blake3**](https://github.com/forkcloser/blake3) — a fork of
  [lukechampine/blake3](https://github.com/lukechampine/blake3), a Go implementation of the
  BLAKE3 hash with AVX2/AVX-512 routines and the `bao` verified-streaming encoding.
- [**erofs**](https://github.com/forkcloser/erofs) — a fork of
  [erofs/go-erofs](https://github.com/erofs/go-erofs): a pure-Go library for reading and
  creating [EROFS](https://erofs.docs.kernel.org/en/latest/) filesystem images through the
  standard `fs.FS` interface, no CGO.
- [**git-validation**](https://github.com/forkcloser/git-validation) — a fork of
  [vbatts/git-validation](https://github.com/vbatts/git-validation), which checks a range
  of commits for a DCO sign-off, a subject under 90 characters and no trailing whitespace.
- [**go-graphviz**](https://github.com/forkcloser/go-graphviz) — a fork of
  [goccy/go-graphviz](https://github.com/goccy/go-graphviz): Go bindings for Graphviz with
  the real Graphviz compiled to WebAssembly and run on wazero, no C toolchain and no system
  Graphviz.
- [**go-licenses**](https://github.com/forkcloser/go-licenses) — a fork of
  [google/go-licenses](https://github.com/google/go-licenses), which reports and checks
  the licences of a Go package's dependency tree.
- [**grid-clock-screensaver**](https://github.com/forkcloser/grid-clock-screensaver) — a fork
  of [chrstphrknwtn/grid-clock-screensaver](https://github.com/chrstphrknwtn/grid-clock-screensaver),
  a word-clock screensaver for macOS, ported to modern macOS.
- [**vz**](https://github.com/forkcloser/vz) — a fork of
  [Code-Hex/vz](https://github.com/Code-Hex/vz), the Go binding for Apple's
  Virtualization.framework, for the hosts ossein and zerogravity run on (recent macOS with Apple silicon).
- [**xz**](https://github.com/forkcloser/xz) — a fork of
  [ulikunitz/xz](https://github.com/ulikunitz/xz).
- [**yamlfmt**](https://github.com/forkcloser/yamlfmt) — a fork of
  [google/yamlfmt](https://github.com/google/yamlfmt), a YAML formatter as a command line
  and a library.

## Packagings

Upstream code, our packaging: nothing rewritten, nothing forked, only built or
imported at an audited pin and published so the fleet can pin it by checksum
and attestation.

- [**curl**](https://github.com/forkcloser/curl) — one `curl` with dependable
  TLS 1.3 for every platform, from [curl-for-win](https://github.com/curl/curl-for-win),
  the curl project's own reproducible-build system.
- [**openssh**](https://github.com/forkcloser/openssh) — OpenSSH for macOS with
  security keys built in, which Apple's build lacks: OpenSSH portable built
  against LibreSSL's libcrypto, libfido2 and libcbor linked statically, Apple's
  clang, one relocatable package per release.
- [**testbin**](https://github.com/forkcloser/testbin) — test-only builds of C tools
  other repositories need in their tests (`mkfs.erofs`, FLAC encoders and decoders), each
  from pinned upstream sources under upstream's licence.

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

- [**website**](https://github.com/forkcloser/website) — the organisation's site,
  [www.forkcloser.org](https://www.forkcloser.org/).
- [**website-godolint**](https://github.com/forkcloser/website-godolint) — godolint's site,
  [godolint.forkcloser.org](https://godolint.forkcloser.org/).
- [**website-taciturn**](https://github.com/forkcloser/website-taciturn) — taciturn's site,
  [taciturn.forkcloser.org](https://taciturn.forkcloser.org/).
- [**infra**](https://github.com/forkcloser/infra) — the infrastructure behind the
  websites, as code: one zone per domain, one block per site; OpenTofu holds the
  Cloudflare Pages projects and their domains, and `dnsctl`, a Go program in the
  repository, makes the DNS and registrar accounts match `zones.yaml`, delegation and
  DNSSEC included. A pull request adds a site; the apply on `main` creates it.

## Contributing & support

Projects here are provided as-is, best-effort, without warranty.

- [Contributing guide](https://github.com/forkcloser/.github/blob/main/.github/CONTRIBUTING.md)
- [Security policy](https://github.com/forkcloser/.github/blob/main/.github/SECURITY.md)
- Bugs and ideas go to the issue tracker of the repository they concern.

The sibling organization [farcloser](https://github.com/farcloser) holds the tooling and
libraries these are built with.
