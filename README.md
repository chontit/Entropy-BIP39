# Entropy → BIP39 (Offline)

**Chollatis Bitcoiner — _Don't Trust, Verify._**

A minimal, single-file, offline tool that converts **your own entropy** (coin flips or d16 rolls) into a **BIP39 seed phrase** (12 or 24 words). Every line of code carries a Thai explanation, so anyone can audit the whole file before use.

เครื่องมือแปลง **Entropy ที่คุณสร้างเอง** (โยนเหรียญ / ทอยลูกเต๋า d16) ให้เป็น **BIP39 Seed Phrase** 12 หรือ 24 คำ เป็นไฟล์ HTML ไฟล์เดียว ทำงานออฟไลน์ทั้งหมด และมีคำอธิบายภาษาไทยทุกบรรทัด ให้ตรวจสอบโค้ดได้เองก่อนใช้งาน

---

## Features / คุณสมบัติ

| | English | ภาษาไทย |
|---|---|---|
| 📄 | One self-contained HTML file, zero dependencies | ไฟล์เดียว ไม่พึ่งไลบรารีภายนอก |
| 🔒 | CSP `default-src 'none'`: the browser blocks every network request | CSP สั่งเบราว์เซอร์บล็อกการเชื่อมต่อออกนอกไฟล์ทุกช่องทาง |
| 🎲 | No `Math.random`, no browser crypto: all randomness comes from you | ไม่สุ่มเอง ความสุ่มทั้งหมดมาจากผู้ใช้ |
| ⌨️ | Input: Binary (0/1) or HEX (0-9, A-F), invalid characters can't be typed | รับเฉพาะ Binary หรือ HEX ตัวอักษรอื่นพิมพ์ไม่ติด |
| ⚡ | Real-time output + "roll N more bits" hint when input is short | แสดงผลทันที และบอกว่าต้องทอยเพิ่มอีกกี่บิต |
| 🔍 | Shows every step: entropy → SHA-256 → checksum → 11-bit groups → words | แสดงค่าระหว่างทางทุกขั้นตอน ให้ตรวจทานได้ |
| 🧮 | SHA-256 (FIPS 180-4) written from scratch, fully commented | SHA-256 เขียนเองในไฟล์ มีคำอธิบายครบ |
| ✅ | Self-test on every load (9 vectors) | ทดสอบตัวเองทุกครั้งที่เปิดไฟล์ |
| 🧹 | "Clear" button to wipe the screen after writing down | ปุ่มล้างข้อมูลหลังจดเสร็จ |

## Self-test (runs on every load)

| Test | Reference |
|---|---|
| SHA-256 of `""`, `"abc"`, 448-bit message | NIST FIPS 180-2 test vectors |
| Wordlist: 2048 words + SHA-256 = `2f5eed53…3b24dbda` | Official `bip-0039/english.txt` |
| BIP39: `00×16`, `80×16`, `FF×16`, `00×32`, `FF×32` | Trezor official BIP39 vectors |

If any test fails, the page shows a red warning: **do not use the file.**
ถ้าไม่ผ่านแม้แต่ชุดเดียว หน้าจอจะขึ้นเตือนสีแดง **ห้ามใช้งาน**

Development testing: 400 random entropies (128 & 256 bit) matched the reference implementation `python-mnemonic` 400/400.

---

## Usage on Tails OS / วิธีใช้บน Tails

1. Copy `Entropy-to-BIP39.html` to a USB drive and **verify the SHA-256 and PGP signature** (see below).
2. Boot Tails **without connecting to any network**.
3. Tor Browser → Security Level **Standard** (the "Safest" level disables JavaScript).
4. Open the file. Confirm the green ✔ Self-test line.
5. Choose **Binary** or **HEX**, choose **12 or 24 words**, then enter your entropy.
6. Write down the words, press **ล้างข้อมูล (Clear)**, then **shut down Tails** (Tails wipes RAM on shutdown).

ขั้นตอนภาษาไทย:
1. คัดลอกไฟล์ลง USB แล้ว**ตรวจ SHA-256 และลายเซ็น PGP** ก่อน
2. บูต Tails โดย**ไม่ต่อเครือข่าย**
3. ตั้ง Security Level ของ Tor Browser เป็น **Standard** (ระดับ Safest จะปิด JavaScript)
4. เปิดไฟล์ แล้วดูว่า Self-test ขึ้น ✔ สีเขียว
5. เลือกรูปแบบ Input และจำนวนคำ แล้วกรอก Entropy
6. จดคำเสร็จแล้วกด **ล้างข้อมูล** จากนั้น **Shutdown Tails** เพื่อล้าง RAM

### Entropy required / จำนวนที่ต้องทอย

| Words | Entropy | Coin flips (Binary) | d16 rolls (HEX) |
|---|---|---|---|
| 12 | 128 bits | 128 | 32 |
| 24 | 256 bits | 256 | 64 |

> Extra input is allowed, but **only the first 128/256 bits are used**. The page turns orange and shows the ignored bits. If you back up the entropy, back up only the bits actually used.
>
> กรอกเกินได้ แต่ระบบ**ใช้เฉพาะ 128/256 บิตแรก**เท่านั้น หน้าจอจะขึ้นสีส้มและแสดงบิตส่วนเกินแยกไว้ ถ้าจะจดสำรอง Entropy ให้จดเฉพาะบิตที่ใช้จริง

---

## Verify the file / ตรวจสอบไฟล์

```
SHA-256: b41e1215ddcbf25c53157e79d8fc5442c83c59444572bc7c2311d72144c51ef1
```

```bash
# 1. Hash
sha256sum -c SHA256SUMS.txt

# 2. Signature (import the public key first)
gpg --import chollatis-bitcoiner-pubkey.asc
gpg --verify SHA256SUMS.txt.asc SHA256SUMS.txt
```

PGP key: **Chollatis Maneewong - Bitcoiner** (ed25519)
Fingerprint: `EEFC F3F0 928D 0199 BA7E  56EC 2DB5 4085 AB23 3A47`

Always compare the fingerprint through a second channel before trusting it.
ควรเทียบ fingerprint จากช่องทางอื่นอีกทางก่อนเชื่อถือ

### Cross-check a result by hand / ตรวจผลลัพธ์ด้วยตัวเอง

- Step **[2]** shows entropy as HEX bytes. Hash it with any other SHA-256 tool in **hex input** mode; the result must match step **[3]**.
- The **ลำดับในรายการ (1-2048)** column lets you look up each word in a printed BIP39 wordlist.

---

## How it works / หลักการทำงาน

```
Entropy (128/256 bits)
   └─ SHA-256(entropy bytes) → first ENT/32 bits = checksum (4 or 8 bits)
Entropy + Checksum (132/264 bits)
   └─ split into 11-bit groups → DEC 0–2047 → wordlist index → 12/24 words
```

## File structure (code sections)

| Section | Content |
|---|---|
| 1 | SHA-256 (FIPS 180-4) |
| 2 | Hex / binary / byte helpers |
| 3 | BIP39 English wordlist (16 words per line, index in comment) |
| 4 | BIP39 core: entropy → mnemonic |
| 5 | Self-test |
| 6 | UI: input filter, real-time render, clear button |

---

## ⚠️ Disclaimer

- This tool **does not generate randomness**. Seed security depends entirely on the quality of your coin flips or dice rolls. Use fair dice and never make up numbers in your head.
- Use only on an **air-gapped / offline** machine. Never enter a real seed on an internet-connected device.
- Educational and self-custody tool, provided **as is**, without warranty.

เครื่องมือนี้**ไม่ได้สร้างความสุ่ม** ความปลอดภัยของ Seed ขึ้นกับคุณภาพการทอยของคุณทั้งหมด ใช้ลูกเต๋าที่เที่ยงตรงและห้ามคิดตัวเลขเอง ใช้บนเครื่องออฟไลน์เท่านั้น

## License

MIT © 2026 Chollatis Bitcoiner

---

**© 2026 Chollatis Bitcoiner. | Don't Trust, Verify. Powered by Claude AI**
🌐 [learning.chontit.win](https://learning.chontit.win)
