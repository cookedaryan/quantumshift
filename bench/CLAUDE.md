# bench/ (C++17 PQC benchmark suite)

Measures what the new algorithms cost, so migration plans rest on real numbers.

- Libraries: liboqs through liboqs-cpp for post-quantum algorithms, OpenSSL (3.5 for hybrid X25519MLKEM768) for classical ones.
- Measure per operation: key generation, encapsulation, decapsulation, sign, verify. Also public key, ciphertext and signature sizes, peak memory, and TLS handshake time and bytes (X25519 versus X25519MLKEM768).
- Run each operation thousands of times after discarded warm-up runs. Report median and p95.
- Tag every result with a hardware profile: laptop (x86 server class) or Raspberry Pi (ARM, standing in for IoT devices).
- Results are written to PostgreSQL; the Java RAG planner reads them from there. Never hard-code a measured number.
- Sizes are fixed by the standards (FIPS 203/204/205); a test checks measured sizes against them.
- Reproducibility target: repeated runs on one device agree within a few percent.
- Set up the Raspberry Pi early (64-bit OS); if liboqs or OpenSSL 3.5 fail to build there, fall back to Docker images.
