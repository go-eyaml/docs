# Roadmap

## Done

- **Token format** — `IsToken` / `ParseToken` / `FormatToken` for the `ENC[PKCS7,<base64>]` tokens hiera-eyaml embeds in YAML, tolerant of whitespace-wrapped values.
- **PKCS7 encryptor** — A random AES-256 content key (AES-256-CBC, PKCS#7 padding) wrapped with RSA PKCS#1 v1.5 under an X.509 certificate, serialised as a CMS EnvelopedData `ContentInfo` — mirroring hiera-eyaml's `pkcs7` encryptor.
- **Key management** — `CreateKeys` generates the RSA keypair and self-signed certificate that `eyaml createkeys` produces; `LoadPrivateKey` / `LoadCertificate` read PEM / PKCS8 material.
- **Standard-library crypto only** — Built on `crypto/rsa`, `crypto/x509`, `crypto/aes`, `crypto/cipher`, `crypto/rand`, `encoding/pem` and `encoding/asn1`; the PKCS#7 / CMS structure is hand-assembled — no third-party crypto, no cgo.

## Next

- **Pluggable schemes (gpg)** — `Encryptor` is a seam; the hiera-eyaml `gpg` scheme is intentionally deferred, as it would pull in OpenPGP machinery beyond the standard-library crypto surface.

Quality is a standing gate: 100% coverage including error branches, `gofmt` + `go vet` clean, CI green across the six 64-bit Go targets (amd64, arm64, riscv64, loong64, ppc64le, s390x).
