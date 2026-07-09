# Usage & API

```go
import "github.com/go-eyaml/eyaml"
```

```go
// Generate the hiera-eyaml keypair + self-signed cert.
certPEM, keyPEM, _ := eyaml.CreateKeys(nil)

enc, _ := eyaml.NewPKCS7(certPEM, keyPEM)
token, _ := eyaml.Encrypt(enc, []byte("s3cret"))   // ENC[PKCS7,...]

if eyaml.IsToken(token) {
    plain, _ := eyaml.Decrypt(enc, token)          // "s3cret"
    _ = plain
}
```

`CreateKeys` generates the RSA keypair and self-signed certificate `eyaml createkeys` would produce. `NewPKCS7` builds the default `Encryptor` from cert + key PEM; `Encrypt` / `Decrypt` are the high-level round trip. Token helpers `IsToken`, `ParseToken` and `FormatToken` handle the `ENC[PKCS7,<base64>]` wrapping, and `LoadPrivateKey` / `LoadCertificate` read existing PEM material. `Encryptor` is a pluggable interface for additional schemes.

## Command line & builds

The library is `CGO_ENABLED=0` pure Go. Cross-compile it anywhere:

```sh
GOOS=linux   GOARCH=arm64    go build ./...
GOOS=js      GOARCH=wasm     go build ./...
```

It builds and tests on all six 64-bit Go targets (amd64, arm64, riscv64, loong64, ppc64le, s390x).
