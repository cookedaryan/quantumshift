# RAG corpus

The documents the RAG planner retrieves from (project plan Section 5.8). Issue: M1-14. Consumers: M2-12 (ingestion) needs `doc_id` and `local_file`; M3-12 (section-aware chunking) needs `cite_as` and `format`; M5-09 (re-check the standards) re-downloads and compares `SHA-256`.

**Files live in `data/raw/` and are never committed** (the folder is gitignored). Some sources have licence terms that do not allow redistribution, and the PDFs are large. Anyone can rebuild the folder from the URLs below.

Checked on **2026-10-12**. Facts below come from the sources' own pages unless the notes say otherwise. Rows marked **pending** are not downloaded yet.

## Documents

| doc_id | cite_as | title | publisher | version / date | status | format |
|---|---|---|---|---|---|---|
| `fips-203` | FIPS 203 | Module-Lattice-Based Key-Encapsulation Mechanism Standard | NIST | 2024-08-13 | Final; errata pending (see notes) | pdf |
| `fips-204` | FIPS 204 | Module-Lattice-Based Digital Signature Standard | NIST | 2024-08-13 | Final; errata pending (see notes) | pdf |
| `fips-205` | FIPS 205 | Stateless Hash-Based Digital Signature Standard | NIST | 2024-08-13 | Final | pdf |
| `sp-800-208` | SP 800-208 | Recommendation for Stateful Hash-Based Signature Schemes | NIST | 2020-10-29 | Final | pdf |
| `nist-ir-8547-ipd` | NIST IR 8547 (draft) | Transition to Post-Quantum Cryptography Standards | NIST | 2024-11-12 | **Initial public draft**; no final version found | pdf |
| `cnsa-2.0-advisory` | CNSA 2.0 | Announcing the Commercial National Security Algorithm Suite 2.0 | NSA | September 2022, Ver. 1.0 (PP-22-1338) | Original CNSA 2.0 advisory; no newer edition found | pdf |
| `cnsa-2.0-faq` | CNSA 2.0 FAQ | Commercial National Security Algorithm Suite 2.0 and Quantum Computing Frequently Asked Questions (Cybersecurity Information Sheet) | NSA | October 2026, Ver. 2.2 (U/OO/6072712-26, PP-26-3194) | Current as of download (2026-10-14) | pdf |
| `rfc-9954` | RFC 9954 | Hybrid Key Exchange in TLS 1.3 | IETF (Stebila, Fluhrer, Gueron) | July 2026 | Informational RFC (was draft-ietf-tls-hybrid-design) | text |
| `rfc-10024` | RFC 10024 | Post-Quantum Traditional (PQ/T) Hybrid Key Agreement Mechanisms for TLS 1.3 | IETF (Kwiatkowski et al.) | August 2026 | Proposed Standard (was draft-ietf-tls-ecdhe-mlkem) | text |
| `openssl-evp-kem-ml-kem` | OpenSSL 3.5, EVP_KEM-ML-KEM(7) | EVP_KEM-ML-KEM: ML-KEM-512/768/1024 support | OpenSSL Project | OpenSSL 3.5 docs | Added in 3.5 | html |
| `jep-496` | JEP 496 | Quantum-Resistant Module-Lattice-Based Key Encapsulation Mechanism | OpenJDK | JDK 24 | Delivered; **comparison only** (the project uses Java 21) | html |
| `bc-interoperability` | Bouncy Castle: Specification & Interoperability | Specification & Interoperability (lists supported post-quantum algorithms) | Legion of the Bouncy Castle | version not established | pending | html |
| `bc-javadoc-mlkem` | Bouncy Castle Javadoc: ML-KEM | Package org.bouncycastle.pqc.crypto.mlkem | Legion of the Bouncy Castle | version not established | pending | html |

## Where each file comes from

| doc_id | URL | local_file | SHA-256 | licence | notes |
|---|---|---|---|---|---|
| `fips-203` | https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.203.pdf (page: https://csrc.nist.gov/pubs/fips/203/final, DOI 10.6028/NIST.FIPS.203) | `data/raw/fips-203.pdf` | `fe1f12f32a7e44ec9fdebbf400cda843a40b506dee676725234dc6f7923b6cac` | US government work per NIST's technical-series statement (see licence notes) | 1,252,341 bytes, 56 pages. The CSRC page has a planning note dated 2025-11-17: an issue "will be corrected in a future update/revision". Errata spreadsheet is linked from the page and is not in the corpus |
| `fips-204` | https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.204.pdf (page: https://csrc.nist.gov/pubs/fips/204/final, DOI 10.6028/NIST.FIPS.204) | `data/raw/fips-204.pdf` | `57239b9f84c03227eda3ca0991204dc7764c79af9ce2e6824eda774918d46b6b` | US government work per NIST statement | 3,291,746 bytes, 65 pages. The CSRC page has a planning note dated 2026-07-31 about several minor errata to be corrected in a future revision. The errata spreadsheet is linked from the page and is not in the corpus |
| `fips-205` | https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.205.pdf (page: https://csrc.nist.gov/pubs/fips/205/final, DOI 10.6028/NIST.FIPS.205) | `data/raw/fips-205.pdf` | `8ef34228276f3386d23cb0da8c14592b8cfb0db3358016bba64df7a004f8d13d` | US government work per NIST statement | 1,055,752 bytes, 61 pages |
| `sp-800-208` | https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-208.pdf (page: https://csrc.nist.gov/pubs/sp/800/208/final, DOI 10.6028/NIST.SP.800-208) | `data/raw/sp-800-208.pdf` | `9ac6bee3da878e883874847b40f80d2265d6268385d8201f866581437533465f` | US government work per NIST statement | 872,986 bytes, 59 pages |
| `nist-ir-8547-ipd` | https://nvlpubs.nist.gov/nistpubs/ir/2024/NIST.IR.8547.ipd.pdf (page: https://csrc.nist.gov/pubs/ir/8547/ipd, DOI 10.6028/NIST.IR.8547.ipd) | `data/raw/nist-ir-8547-ipd.pdf` | `6b551b4ff9858a19c1ab48d7deef2ee52d615066b7d187aaf992cb50c5ca0ed6` | US government work per NIST statement | 722,379 bytes, 29 pages. Cover reads "Initial Public Draft", November 2024; comment period closed 2025-01-10. A plan that cites this must say "draft". Tables on PDF pages 20-21 (printed 13-14): at the 112-bit security level, RSA, ECDSA, finite-field DH and ECDH are "Deprecated after 2030" and "Disallowed after 2035"; at 128 bits or more, RSA, ECDSA, EdDSA and ECDH are "Disallowed after 2035" with no deprecation date. So "deprecate after 2030, disallow after 2035" holds only for the 112-bit level (RSA-2048 is at that level). An earlier header-only check wrongly reported 404; see the note at the end |
| `cnsa-2.0-advisory` | NSA Post-Quantum Cryptography Resource Hub: https://www.nsa.gov/Cybersecurity/Post-Quantum-Cryptography-Resource-Hub/ (downloaded in a browser on 2026-10-14; exact file URL not recorded) | `data/raw/cnsa-2.0-advisory.pdf` | `5abe0a74a66d146b6e617ffad981ecf35bd608714aa8d1441c8f444e9c322c55` | unclear, check: the file has no copyright or licence statement, only a disclaimer of endorsement. US government publication; kept local only | 586,858 bytes, 10 pages. Cover reads "SEP 2022 Ver. 1.0". This is the 2022 original, not the May 2025 edition that a search summary had suggested, so earlier notes about a 2025 version were wrong. The old media.defense.gov links return 403 to scripts |
| `cnsa-2.0-faq` | NSA Post-Quantum Cryptography Resource Hub: https://www.nsa.gov/Cybersecurity/Post-Quantum-Cryptography-Resource-Hub/ (downloaded in a browser on 2026-10-14; exact file URL not recorded) | `data/raw/cnsa-2.0-faq.pdf` | `bc430341c4ae2ca9af723dc4425a05f2d6dfc8e401bf36c3827e17b01362c60d` | unclear, check: the file has no copyright or licence statement, only a disclaimer of endorsement. US government publication; kept local only | 386,784 bytes, 31 pages. Cover reads "October 2026 Ver. 2.2"; newer than the Ver. 2.1 (December 2024) noted earlier. Covers timeframes, CNSSP 15 and NSM-10, CSfC and NIAP, and hybrids and composites. The old media.defense.gov link returns 403 to scripts |
| `rfc-9954` | https://www.rfc-editor.org/rfc/rfc9954.txt (info: https://www.rfc-editor.org/info/rfc9954) | `data/raw/rfc-9954.txt` | `db5b9f76fa7182dd02e05e1e0640f095cb51ccc8dd035fca38d66f9189d55ad0` | IETF Trust Legal Provisions (BCP 78), © 2026 IETF Trust and the authors; code components under Revised BSD | 44,581 bytes. File starts with a UTF-8 byte-order mark |
| `rfc-10024` | https://www.rfc-editor.org/rfc/rfc10024.txt (info: https://www.rfc-editor.org/info/rfc10024) | `data/raw/rfc-10024.txt` | `5efcfacef53549ec4ff57450dfec28085b34cf3f34d829c541c0ddbe19230dfe` | IETF Trust Legal Provisions (BCP 78), © 2026 IETF Trust and the authors; code components under Revised BSD | 21,016 bytes. Starts with a byte-order mark. Section 7.1 defines `X25519MLKEM768` as 0x11EC (4588): checked in the file |
| `openssl-evp-kem-ml-kem` | https://docs.openssl.org/3.5/man7/EVP_KEM-ML-KEM/ | `data/raw/openssl-evp-kem-ml-kem.html` | `d8de2774631dac0fa21ee9df295148452381069d8a737c54a6a691b050c2fdfc` | Apache License 2.0 (page footer: © 2024-2026 The OpenSSL Project Authors) | 58,994 bytes. Sections are named, not numbered |
| `jep-496` | https://openjdk.org/jeps/496 | `data/raw/jep-496.html` | `830006ed037049dd6038bd6bdbeb0255af1fba3eb06bbaa6c716ca9ed14a927d` | Page footer reads "© 2026 Oracle Corporation and/or its affiliates", "License: GPLv2" (as reported by a page summary, not re-read in the file) | 24,513 bytes (a header-only check earlier the same day said 24,403: the page is live, so the hash can change). Java 21 has no built-in ML-KEM; this documents the JDK 24 feature for comparison |
| `bc-interoperability` | https://www.bouncycastle.org/documentation/interoperability/ | pending: `data/raw/bc-interoperability.html` | pending | unclear, check: https://www.bouncycastle.org/about/license/ was not read | **pending.** Two page summaries gave different paths for this page (`.../interoperability/` and `.../specification_interoperability/`). Open the Bouncy Castle Java documentation page, use whichever resolves, and fix this row |
| `bc-javadoc-mlkem` | https://downloads.bouncycastle.org/java/docs/bcprov-jdk18on-javadoc/org/bouncycastle/pqc/crypto/mlkem/package-summary.html | pending: `data/raw/bc-javadoc-mlkem.html` | pending | unclear, check | **pending.** URL came from a search result. A search summary said some classes here are deprecated in favour of `org.bouncycastle.crypto.kems`; confirm on the page and record which package the project should cite. Record the Bouncy Castle version shown on the page |

## Licence notes

- **NIST publications.** NIST's statement says works authored by NIST employees "are not subject to Copyright protection within the United States", and it grants the public a royalty-free right to reprint outside the US. It asks for the recommended citation and "Republished courtesy of the National Institute of Standards and Technology". That page describes technical reports and the Journal of Research; it does not name FIPS or IR by name, so this table says "per NIST statement" and does not claim more. Source: https://www.nist.gov/open/copyright-fair-use-and-licensing-statements-srd-data-software-and-technical-series-publications
- **IETF RFCs.** Subject to the IETF Trust's Legal Provisions. We keep local copies for research and do not redistribute them.
- **Everything is local only.** Nothing in `data/raw/` goes into the repo or the demo bundle. If a document is ever shown in the dashboard (the plan's "click a citation to open the standard"), link to the source URL instead of copying the file.

## Notes for the ingestion and chunking issues

- NIST PDFs and RFC text have numbered sections, so `cite_as` plus the section number gives citations like "FIPS 203, Section 6". OpenSSL and JEP pages have named headings only, so cite them by page and heading.
- The RFC text files start with a UTF-8 byte-order mark. Strip it before chunking.
- NIST IR 8547 is a draft. Plans and the report must say so.
- FIPS 203 and 204 will be revised for errata. If NIST publishes a revision, re-download, compare the new hash with the one here, and re-index (this is M5-09).

## Re-downloading and verifying

To check a file against this table (Git Bash, from the repo root):

```bash
sha256sum data/raw/fips-203.pdf
```

The output must match the SHA-256 in the table. A mismatch means the source changed, or the download is damaged.

**Checking a link:** `nvlpubs.nist.gov` answers header-only requests (`curl -I`) with 404 for some files that download fine with a normal GET. To test a link without downloading it, ask for one byte: `curl -s -o /dev/null -w '%{http_code}' -r 0-0 <url>` should print 206 or 200.
