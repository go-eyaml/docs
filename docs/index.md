# go-eyaml

**eyaml encryption in pure Go — PKCS7 and GPG encrypt/decrypt for hiera-eyaml data, CGO-free.**

go-eyaml is a pure-Go (CGO_ENABLED=0) implementation of the two encryption schemes used by Puppet's hiera-eyaml: the ENC[PKCS7,<base64>] and ENC[GPG,<base64>] token formats that carry an encrypted value inside otherwise-plaintext YAML. For PKCS7, a random 256-bit AES content key encrypts the plaintext with AES-256-CBC, the content key is wrapped for the recipient with RSA PKCS#1 v1.5 under an X.509 certificate, and the whole thing is serialised as a CMS EnvelopedData (RFC 5652) token — built exclusively on the standard library's crypto packages, with the PKCS#7 / CMS structure assembled by hand. For GPG, a session key is public-key-encrypted to one or more recipient OpenPGP keys exactly as `gpg --encrypt` does, using the maintained pure-Go `github.com/ProtonMail/go-crypto/openpgp` implementation. Both schemes stay CGO_ENABLED=0. 100% coverage, six arches and WebAssembly.

- **[Why pure Go](why.md)** — a static, cgo-free engine for the Puppet stack.
- **[The eyaml scheme](scheme.md)** — the capabilities in detail.
- **[Usage & API](api.md)** — the Go API and how to call it.
- **[Roadmap](roadmap.md)** — what is done and what is next.

## Guarantees

- **Pure Go, zero cgo.** PKCS7 imports the Go standard library's crypto packages only; GPG adds the pure-Go, CGO-free `ProtonMail/go-crypto/openpgp` implementation. Both cross-compile to the six 64-bit Go targets (amd64, arm64, riscv64, loong64, ppc64le, s390x) and WebAssembly, linking into a static binary.
- **Interoperable with hiera-eyaml's pkcs7 and gpg tokens.**
- **100% test coverage** including error branches, enforced as a CI gate.
