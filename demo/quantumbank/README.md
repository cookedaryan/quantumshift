# QuantumBank

A fictional bank we build ourselves with deliberately old cryptography. Because we know where every crypto use is, we can measure the tool's accuracy live. It contains no real personal data and no real private keys.

## Planted old cryptography

| Component | Language | Old cryptography planted inside | Built by |
|---|---|---|---|
| Banking API | Java (Spring Boot) | RSA-2048 wraps customer data keys; login tokens (JWT) signed with ES256 (ECDSA P-256) | role 5 |
| Notification service | Python | ECDH key exchange using the `cryptography` library | role 3 |
| ATM firmware | C++ | Stripped binary, statically linked with mbedTLS, verifies updates with ECDSA | role 1 |
| Web server | nginx config | ECDHE-RSA cipher suites and an RSA-2048 certificate | role 6 |
| Key storage | Java keystore (.jks) | Several RSA key pairs | role 5 |

## Decoys (traps for the scanner, not real problems)

- SHA-256 hashing and AES-256 encryption (crypto, but quantum-safe).
- A variable named `rsaCount` and a class named `EccReport`.
- Commented-out RSA code and crypto inside unit-test files.

Role 4 writes the decoy code.

## Ground truth

A ground-truth file will list every real crypto use and every decoy. The dashboard compares the tool's findings with it and shows the accuracy on screen. Status: not started.
