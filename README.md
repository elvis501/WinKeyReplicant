# OPIE / S/Key OTP Calculator

**Live:** https://elvis501.github.io/WinKeyReplicant/

A fully self-contained, single-file HTML implementation of the **RFC 2289** One-Time Password system (OPIE / S/Key). No dependencies, no build step — just open in a browser.

## Features

- Supports **MD4**, **MD5**, and **SHA1** algorithms
- Auto-detects algorithm from the challenge string (`otp-md4 / otp-md5 / otp-sha1`)
- Show/hide password toggle
- One-click copy of the 6-word result to clipboard
- Full **2048-word RFC 2289 Appendix D** dictionary embedded
- Verified against all official **Appendix C test vectors**

## Usage

1. Open `index.html` in any modern browser (or visit the [live version](https://elvis501.github.io/WinKeyReplicant/))
2. Paste the server challenge (e.g. `otp-md5 499 host99373`)
3. Enter your passphrase
4. Click **Calculate** — the 6-word OTP appears in the Result field
5. Copy it with the clipboard icon

## Challenge format

```
otp-{md4|md5|sha1} <sequence> <seed>
```

Example: `otp-md5 99 TeSt` with passphrase `This is a test.` → `BAIL TUFT BITS GANG CHEF THY`

## Algorithm

Implements RFC 2289 exactly:

- **Iteration**: `1 + N` total hash applications (initial `hash(seed + passphrase)` then N more)
- **Folding to 64 bits**:
  - MD4/MD5 (128-bit): XOR bytes 0–7 with bytes 8–15
  - SHA1 (160-bit): XOR-fold per RFC 2289 Appendix A (`d0 ^= d2 ^ d4; d1 ^= d3`), output in little-endian
- **Word encoding**: 2-bit checksum appended, 66 bits split into 6 × 11-bit dictionary indices

## Implementation

| Component | Method |
|-----------|--------|
| MD4 | Pure JavaScript (RFC 1320) |
| MD5 | Pure JavaScript (RFC 1321) |
| SHA1 | Web Crypto API (`crypto.subtle`) |

## References

- [RFC 2289 — A One-Time Password System](https://www.rfc-editor.org/rfc/rfc2289)
- [RFC 1321 — The MD5 Message-Digest Algorithm](https://www.rfc-editor.org/rfc/rfc1321)
- [RFC 1320 — The MD4 Message-Digest Algorithm](https://www.rfc-editor.org/rfc/rfc1320)
