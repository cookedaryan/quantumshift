# scanner/ (C++17 binary scanner)

Finds cryptography inside compiled programs (ELF and PE), including stripped and statically linked ones.

- Three detection methods: constant signatures (works on stripped binaries), imported symbols (imports only), strings (usually).
- Write fingerprints as YARA rules (libyara). Read binaries with LIEF.
- Search every byte layout: crypto libraries often store curve constants as little-endian 32- or 64-bit words, so big-endian-only matching misses them.
- Must detect the P-256 prime and the `mbed TLS 2.28` string in the stripped QuantumBank ATM firmware.
- Baseline to beat: findcrypt-yara. Compare on the same test set.
- Output is `Finding` messages from proto/findings.proto (`location` is the binary offset, `evidence` says why).
- Tests: ctest. Every fingerprint gets a test with a small known binary, in each endianness.
