# Usage & API

```go
import "github.com/go-eyaml/eyaml"
```

```go
// Generate the hiera-eyaml keypair + self-signed cert.
kp, _ := eyaml.CreateKeys(nil)   // *KeyPair{PrivateKeyPEM, PublicKeyPEM}

enc, _ := eyaml.NewPKCS7(kp.PublicKeyPEM, kp.PrivateKeyPEM)
token, _ := eyaml.Encrypt(enc, []byte("s3cret"))   // ENC[PKCS7,...]

if eyaml.IsToken(token) {
    plain, _ := eyaml.Decrypt(enc, token)          // "s3cret"
    _ = plain
}
```

`CreateKeys` generates the RSA keypair and self-signed certificate `eyaml createkeys` would produce, returned as a `*KeyPair`. `NewPKCS7` builds the default `Encryptor` from cert + key PEM; `Encrypt` / `Decrypt` are the high-level round trip. Token helpers `IsToken`, `ParseToken` and `FormatToken` handle the `ENC[PKCS7,<base64>]` wrapping, and `LoadPrivateKey` / `LoadCertificate` read existing PEM material. `Encryptor` is a pluggable interface for additional schemes.

## GPG

```go
pubKeyRing, _ := os.ReadFile("recipient_pub.asc")
privKeyRing, _ := os.ReadFile("recipient_priv.asc") // nil for encrypt-only

enc, _ := eyaml.NewGPG(pubKeyRing, privKeyRing, nil) // nil passphrase if unprotected
token, _ := eyaml.Encrypt(enc, []byte("s3cret"))     // ENC[GPG,...]

if eyaml.IsToken(token) {
    plain, _ := eyaml.Decrypt(enc, token)            // "s3cret"
    _ = plain
}
```

`NewGPG` builds a `GPG` `Encryptor` from armored or binary OpenPGP keyring material; either keyring may be empty for an encrypt-only or decrypt-only value, and `passphrase` may be `nil` when the private key is unprotected. Tokens round-trip byte-for-byte with hiera-eyaml's GPGME encryptor.

## Command line & builds

The library is `CGO_ENABLED=0` pure Go. Cross-compile it anywhere:

```sh
GOOS=linux   GOARCH=arm64    go build ./...
GOOS=js      GOARCH=wasm     go build ./...
```

It builds and tests on all six 64-bit Go targets (amd64, arm64, riscv64, loong64, ppc64le, s390x).
