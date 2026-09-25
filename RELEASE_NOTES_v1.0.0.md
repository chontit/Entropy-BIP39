# v1.0.0: Initial release

**Entropy → BIP39 (Offline)** · `Entropy-to-BIP39.html`

## Highlights
- Single-file offline tool: Binary / HEX entropy → BIP39 12 or 24 words
- Embedded SHA-256 (FIPS 180-4) written from scratch; Thai explanation on every line
- CSP `default-src 'none'`: no network access possible
- Self-test on every load: NIST SHA-256 ×3, wordlist hash, Trezor BIP39 vectors ×5
- Real-time processing steps [1]–[5] for manual verification
- Input filter: only valid characters can be typed; HEX always uppercase
- Excess bits: only the first 128/256 bits are used, with an orange warning
- Clear button to wipe the screen after backup

## Review
- 400/400 random vectors match `python-mnemonic` reference implementation
- External review (Gemini) addressed:
  - Excess-bit truncation made more visible (orange status + backup warning)
  - Added Clear button
  - Separator whitespace restricted to space / newline (hidden Unicode stripped)

## ไทย
- เวอร์ชันแรก: แปลง Entropy แบบ Binary/HEX เป็น Seed Phrase 12/24 คำ ทำงานออฟไลน์
- SHA-256 เขียนเองในไฟล์ มีคำอธิบายภาษาไทยทุกบรรทัด
- ทดสอบตัวเองทุกครั้งที่เปิดไฟล์ 9 ชุด และทดสอบเทียบ python-mnemonic 400/400 ชุด

## Verify
```
SHA-256: b41e1215ddcbf25c53157e79d8fc5442c83c59444572bc7c2311d72144c51ef1  Entropy-to-BIP39.html
```
Signed with PGP key `EEFC F3F0 928D 0199 BA7E  56EC 2DB5 4085 AB23 3A47`
