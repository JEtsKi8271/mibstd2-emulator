# Owner-supplied experimental accessory identity

CarPlay requires an authenticated accessory. The repository owner supplied and
explicitly requested publication of these two files, together with `DiPlay.apk`:

```
identity/offline-mfi/identity.pk8       # PKCS#8 EC P-256 private key
identity/offline-mfi/certificate.p7b    # accessory certificate (Apple Accessories CA)
```

Only these supplied artifacts and this README are allowlisted; other files under
`identity/` remain ignored. To keep a different local identity elsewhere, start the server with
`--identity-dir /path/to/identity`, or use a remote MFi service with `--mfi-server`.

Background and limitations are documented in the upstream
[README](../README.md#accessory-identity-required). The supplied material is
experimental and is not an identity issued to this PlayPort integration. Apple
can revoke it. This publication is an explicit choice by this repository's owner;
the upstream project normally excludes accessory credentials from its source tree.
Personal iPhone pairing records and generated session tokens are not included.

Verify a pair signs correctly with:

```sh
./gradlew :server:test --tests '*MfiIdentityTest*'
```
