# Why pure Go

go-eyaml is part of the **pure-Go Puppet stack** — a family of engines
(alongside [go-facter](https://github.com/go-facter),
[go-hiera](https://github.com/go-hiera), [go-pcore](https://github.com/go-pcore)
and [go-puppet](https://github.com/go-puppet)) that provide Puppet-ecosystem
capabilities as ordinary Go libraries, with **`CGO_ENABLED=0`** and no runtime
dependency on Ruby or on a C toolchain.

## Static, portable, embeddable

Because it is pure Go — the PKCS7 scheme on the standard library's crypto packages alone, the GPG scheme adding only
the pure-Go `ProtonMail/go-crypto/openpgp` implementation — go-eyaml compiles with cgo disabled,
cross-compiles to every 64-bit Go target (amd64, arm64, riscv64, loong64, ppc64le, s390x) and to WebAssembly, and links
into a single static binary. There is nothing to install alongside it — no shared
library, no interpreter, no external process it must shell out to.

## Testability by construction

Every interaction with the outside world — file access, the environment, the
network — flows through an **injectable seam**. That means both the happy path and
*every error branch* are exercised deterministically against in-memory fixtures,
which is how the project holds **100% coverage** across operating systems and
architectures from a single test suite, without special privileges.

## An engine, not a framework

go-eyaml exposes a small, stable Go API and does one thing well —
the eyaml scheme — as a dependency-light library you embed, not a
service or CLI you must stand up.
