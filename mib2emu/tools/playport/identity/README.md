# Accessory identity (not in git)

CarPlay requires an authenticated accessory. PlayPort needs two files that are deliberately
**not** included in this repository:

```
identity/offline-mfi/identity.pk8       # PKCS#8 EC P-256 private key
identity/offline-mfi/certificate.p7b    # accessory certificate (Apple Accessories CA)
```

`identity/` is ignored by Git. To keep the files elsewhere, start the server with
`--identity-dir /path/to/identity`, or use a remote MFi service with `--mfi-server`.

Where to get a pair, and the caveats that come with it, are documented in the main
[README](../README.md#accessory-identity-required). In short: the same experimental pair DiPlay
bundles can be extracted from a public DiPlay release APK, or recovered from public Carlinkit
firmware. It is shared, not issued to PlayPort, and Apple can revoke it at any time — do not
redistribute it.

Verify a pair signs correctly with:

```sh
./gradlew :server:test --tests '*MfiIdentityTest*'
```
