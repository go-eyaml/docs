# The eyaml scheme

This page walks the engine's capabilities. Each is complete unless explicitly marked as a documented deferral.

## Token format

`IsToken` / `ParseToken` / `FormatToken` for the `ENC[PKCS7,<base64>]` tokens hiera-eyaml embeds in YAML, tolerant of whitespace-wrapped values.

## PKCS7 encryptor

A random AES-256 content key (AES-256-CBC, PKCS#7 padding) wrapped with RSA PKCS#1 v1.5 under an X.509 certificate, serialised as a CMS EnvelopedData `ContentInfo` — mirroring hiera-eyaml's `pkcs7` encryptor.

## GPG encryptor

A session key is public-key-encrypted to one or more recipient OpenPGP keys and encrypts the payload, exactly as `gpg --encrypt` does — mirroring hiera-eyaml's `gpg` (GPGME) encryptor. `NewGPG` builds the encryptor from armored or binary recipient/secret keyrings, with optional passphrase for protected secret keys; the resulting binary OpenPGP message is base64-wrapped into an `ENC[GPG,<base64>]` token, byte-for-byte what hiera-eyaml's GPGME encryptor emits.

## Key management

`CreateKeys` generates the RSA keypair and self-signed certificate that `eyaml createkeys` produces; `LoadPrivateKey` / `LoadCertificate` read PEM / PKCS8 material for PKCS7; `LoadGPGKeyRing` reads OpenPGP keyring material for GPG.

## Crypto dependencies

PKCS7 is built exclusively on `crypto/rsa`, `crypto/x509`, `crypto/aes`, `crypto/cipher`, `crypto/rand`, `encoding/pem` and `encoding/asn1` — the PKCS#7 / CMS structure is hand-assembled, no third-party crypto. GPG adds one dependency, the maintained pure-Go OpenPGP implementation `github.com/ProtonMail/go-crypto/openpgp`; it is still CGO-free and built from source, so the module as a whole needs no cgo.

## Pluggable schemes

`Encryptor` is a seam — `PKCS7` and `GPG` both implement it, and further hiera-eyaml schemes can be added the same way.
