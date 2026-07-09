# go-eyaml

**eyaml encryption in pure Go — PKCS7 encrypt/decrypt for hiera-eyaml data, stdlib crypto only.**

go-eyaml is a pure-Go (CGO_ENABLED=0) implementation of the encryption scheme used by Puppet's hiera-eyaml: the ENC[PKCS7,<base64>] token format that carries an encrypted value inside otherwise-plaintext YAML. A random 256-bit AES content key encrypts the plaintext with AES-256-CBC, the content key is wrapped for the recipient with RSA PKCS#1 v1.5 under an X.509 certificate, and the whole thing is serialised as a CMS EnvelopedData (RFC 5652) token. It is built exclusively on the standard library's crypto packages — the PKCS#7 / CMS structure is assembled by hand — so it needs no third-party crypto and no cgo. 100% coverage, six arches and WebAssembly.

- **[Why pure Go](why.md)** — a static, cgo-free engine for the Puppet stack.
- **[The eyaml scheme](scheme.md)** — the capabilities in detail.
- **[Usage & API](api.md)** — the Go API and how to call it.
- **[Roadmap](roadmap.md)** — what is done and what is next.

## Guarantees

- **Pure Go, zero cgo.** Imports the Go standard library's crypto packages only; cross-compiles to the six 64-bit Go targets (amd64, arm64, riscv64, loong64, ppc64le, s390x) and WebAssembly, linking into a static binary.
- **Interoperable with hiera-eyaml's pkcs7 tokens.**
- **100% test coverage** including error branches, enforced as a CI gate.
