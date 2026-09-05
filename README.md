# 🔐 Steganography and Hidden Data Detection

## Computer and Digital Forensics

This repository documents a practical laboratory exercise involving the creation, concealment, extraction, recovery, and forensic verification of hidden information using steganographic techniques.

The exercise was conducted in an authorised laboratory environment using a designated carrier image and controlled evidence files created specifically for the practical.

The laboratory demonstrates practical use of:

* Steganography
* Substitution-based data hiding
* Least Significant Bit (LSB) concepts
* Steghide
* StegSeek
* SHA-256 cryptographic hashing
* `diff`
* `xxd`
* `strings`
* Controlled password dictionary testing
* Linux command-line forensic utilities

The purpose of the practical was to understand how information can be concealed within an apparently ordinary image, how the concealed information can subsequently be recovered, and how cryptographic hashing can be used to verify the integrity of recovered evidence.

---

# 📌 1. Lab Overview

Steganography is a technique used to conceal information within another file or digital medium. Unlike encryption, which primarily transforms information into an unreadable form, steganography focuses on hiding the existence of the information.

In this practical, a BMP image was used as the carrier for controlled evidence files.

The laboratory followed a forensic-style workflow:

```text
Carrier Image
      ↓
Evidence Preservation
      ↓
Controlled Secret File Creation
      ↓
SHA-256 Hashing
      ↓
Steghide Embedding
      ↓
Stego Image Creation
      ↓
Steghide Extraction
      ↓
SHA-256 Verification
      ↓
File Comparison
      ↓
StegSeek Dictionary Recovery
      ↓
Independent Student Exercise
      ↓
Integrity Verification
```

The original carrier image was retained separately and was not overwritten during the embedding process.

---

# 🎯 2. Objectives

The objectives of this laboratory were to:

1. Explain the difference between steganography and encryption.
2. Understand insertion and substitution-based hiding techniques.
3. Understand the Least Significant Bit (LSB) method of data hiding.
4. Prepare and preserve an authorised carrier image.
5. Create and hash a controlled evidence file.
6. Embed the evidence file into a new image using Steghide.
7. Extract the concealed evidence using the correct password.
8. Verify the integrity of extracted evidence using SHA-256.
9. Compare original and stego images using file size, hashes, hexadecimal output and readable strings.
10. Perform a controlled dictionary password-recovery test using StegSeek.
11. Complete an independent steganography exercise using a student-selected password.
12. Apply forensic documentation and integrity-verification principles.

---

# 🧪 3. Authorised Laboratory Environment

All activities were performed within an authorised laboratory environment.

The carrier image was supplied for the practical and was preserved as the original evidence item.

The additional files used during the exercise were created specifically for the laboratory.

No unauthorised systems, devices, organisational data or third-party personal data were intentionally examined.

The original carrier image was preserved before any steganographic operation was performed.

A separate output image was created for each embedding exercise.

---

# 🧠 4. Steganography Concepts

## 4.1 What is Steganography?

Steganography is the practice of concealing information within another file, medium or communication channel so that the existence of the hidden information is not immediately apparent.

For example, information can be embedded within an image while allowing the image to continue functioning as an ordinary image.

In digital forensics, steganography is relevant because concealed information may potentially contain documents, communications, credentials, commands, malware or other evidence.

---

## 4.2 Steganography vs Encryption

Steganography and encryption provide different forms of information protection.

**Encryption** transforms plaintext into ciphertext using a cryptographic algorithm and key. The existence of the information is generally apparent, but its contents are protected from unauthorised reading.

**Steganography** focuses on concealing the existence of information by embedding it within another file or medium.

| Technique                  | Primary Purpose                                            |
| -------------------------- | ---------------------------------------------------------- |
| Encryption                 | Makes information unreadable without the appropriate key   |
| Steganography              | Conceals the existence of information                      |
| Steganography + Encryption | Can conceal information while also protecting its contents |

---

## 4.3 Substitution and LSB

Substitution is one approach used in steganographic techniques.

In substitution-based steganography, selected elements of a carrier are replaced or modified so that they represent information being concealed.

The Least Significant Bit (LSB) is useful for this purpose because changing the least significant bit of a numerical value results in only a very small numerical change.

For example:

```text
Original:  10110110
Modified:  10110111
                    ↑
               LSB changed
```

The numerical difference is small, which can help preserve the apparent visual characteristics of an image.

However, even a small modification to a file can result in a different cryptographic hash.

> **Practical note:** LSB and substitution concepts were studied as part of the laboratory theory. The actual embedding operations in this exercise were performed using Steghide rather than through a manually implemented LSB algorithm.

---

# 🔬 5. Part A - Evidence Preparation and Preservation

A dedicated laboratory directory was created in Kali Linux:

```text
~/ICDFA_Lab4/
```

The directory was organised into separate locations for original evidence, working files, extracted files, evidence records and screenshots.

The repository structure used during the practical was:

```text
ICDFA_Lab4/
├── original/
├── working/
├── extracted/
├── evidence/
└── screenshots/
```

The authorised carrier image was preserved in the `original/` directory.

### Original Carrier

```text
tower_original_image_for_lab.bmp
```

The file was identified as a BMP image with the following characteristics:

* Format: BMP
* Dimensions: 1365 × 2265 pixels
* Colour depth: 24-bit
* File size: 9,277,494 bytes

The original SHA-256 hash was:

```text
6508bebb94bd8ec0833b9f85580cda4261d89632ddc89e7f9f7093b5e653c4db
```

The hash was preserved in:

```text
evidence/original_carrier_sha256.txt
```

The original carrier was retained separately and was not overwritten.

---

# 📝 6. Part B - Controlled Evidence File Creation

A controlled evidence file named:

```text
secret.txt
```

was created specifically for the laboratory.

The file contained the student's name and a controlled evidence message for the practical.

The SHA-256 hash of the original controlled evidence file was calculated before embedding.

```text
30a303f20e00e2f615555541a0446cb1685ccebf4fe36b3693c5182dae59dece
```

The corresponding evidence record was stored as:

```text
evidence/secret_sha256.txt
```

The original evidence file was retained in:

```text
working/secret.txt
```

This file was subsequently used as the payload for the Steghide embedding operation.

---

# 🔐 7. Part C - Steghide Embedding

The original carrier image was preserved and used as the source carrier.

The controlled evidence file was embedded into a new BMP image using Steghide.

The laboratory password was:

```text
1234
```

The embedding operation produced:

```text
tower_stego_lab4.bmp
```

The operation used the following command:

```bash
steghide embed \
-ef secret.txt \
-cf ../original/tower_original_image_for_lab.bmp \
-sf tower_stego_lab4.bmp \
-p 1234
```

The resulting stego image was stored in:

```text
working/tower_stego_lab4.bmp
```

The original carrier remained preserved separately.

### Stego Image Properties

The resulting stego image retained the same basic BMP characteristics:

* Format: BMP
* Dimensions: 1365 × 2265 pixels
* Colour depth: 24-bit
* File size: 9,277,494 bytes

The SHA-256 hash of the stego image was:

```text
edcb2d6b27465546ddc0c303fc4f362f26da13fa0974b518bcec17af48936dd2
```

The hash was recorded in:

```text
evidence/stego_image_sha256.txt
```

---

# 📤 8. Part D - Extraction and Integrity Verification

The concealed evidence was extracted from the stego image using Steghide and the correct laboratory password.

The extraction command was:

```bash
steghide extract \
-sf tower_stego_lab4.bmp \
-xf ../extracted/extracted_secret.txt \
-p 1234
```

The recovered file was:

```text
extracted/extracted_secret.txt
```

The SHA-256 hash of the recovered file was:

```text
30a303f20e00e2f615555541a0446cb1685ccebf4fe36b3693c5182dae59dece
```

The result was recorded in:

```text
evidence/extracted_secret_sha256.txt
```

The original and extracted files were also compared using:

```bash
diff secret.txt ../extracted/extracted_secret.txt
```

The comparison confirmed that the files were identical.

### Integrity Result

```text
Original secret.txt:
30a303f20e00e2f615555541a0446cb1685ccebf4fe36b3693c5182dae59dece

Extracted extracted_secret.txt:
30a303f20e00e2f615555541a0446cb1685ccebf4fe36b3693c5182dae59dece
```

The matching SHA-256 values and successful `diff` comparison support the conclusion that the recovered evidence matched the original controlled evidence file.

---

# 🔎 9. Part E - Original and Stego Image Comparison

The original carrier and resulting stego image were examined using several basic forensic comparison techniques.

The comparison included:

* File size
* SHA-256 hash
* Initial hexadecimal bytes
* Readable strings

---

## 9.1 File Size

Both images were 9,277,494 bytes.

```text
Original carrier:
9,277,494 bytes

Stego image:
9,277,494 bytes
```

Therefore, in this particular experiment, embedding the hidden evidence did not change the overall file size.

---

## 9.2 SHA-256 Comparison

The original carrier produced:

```text
6508bebb94bd8ec0833b9f85580cda4261d89632ddc89e7f9f7093b5e653c4db
```

The stego image produced:

```text
edcb2d6b27465546ddc0c303fc4f362f26da13fa0974b518bcec17af48936dd2
```

The hashes are different.

This demonstrates that the original carrier and stego image are not digitally identical even though they have the same file size and image dimensions.

---

## 9.3 Initial Bytes Using `xxd`

The beginning of each image was examined using:

```bash
xxd -l 64 tower_original_image_for_lab.bmp
```

and:

```bash
xxd -l 64 tower_stego_lab4.bmp
```

The first 64 bytes were observed to be identical.

The BMP header began with:

```text
42 4d 36 90 8d 00 00 00 00 00 36 00 00 00 28 00
```

This indicates that the initial BMP file structure remained unchanged.

The fact that the initial bytes were identical does not establish that the complete files were identical. The SHA-256 comparison demonstrated that the complete files differed.

---

## 9.4 Readable Strings

The `strings` utility was used to examine printable character sequences:

```bash
strings tower_original_image_for_lab.bmp
```

and:

```bash
strings tower_stego_lab4.bmp
```

The output primarily consisted of printable sequences derived from the binary/image data.

No obvious readable copy of the concealed evidence message was identified through this basic strings examination.

This demonstrates a limitation of relying solely on string extraction when investigating steganographic content.

---

## 9.5 Interpretation

The comparison demonstrated that:

1. The original and stego files had the same file size.
2. The original and stego files had the same image dimensions.
3. The initial 64 bytes examined using `xxd` were identical.
4. The complete SHA-256 hashes were different.
5. Basic `strings` analysis did not directly reveal the concealed evidence.
6. Therefore, simple file inspection techniques alone were insufficient to identify the hidden payload.
7. Steganography can preserve the apparent structure and usability of a carrier while modifying underlying digital content.

---

# 🔑 10. Part F - Controlled StegSeek Dictionary Test

A controlled wordlist was created containing the known laboratory password:

```text
1234
```

The wordlist was saved as:

```text
working/lab4_wordlist.txt
```

The SHA-256 hash of the wordlist was recorded in:

```text
evidence/lab4_wordlist_sha256.txt
```

The recorded hash was:

```text
a883dafc480d466ee04e0d6da986bd78eb1fdd2178d04693723da3a8f95d42f4
```

StegSeek was used to test the wordlist against the stego image:

```bash
stegseek tower_stego_lab4.bmp lab4_wordlist.txt
```

The recovery process successfully identified the laboratory password:

```text
Forensic recovery result:
Found passphrase: "1234"
```

StegSeek also identified the original embedded filename:

```text
secret.txt
```

The recovered output was subsequently compared with the original evidence file.

Both files produced the same SHA-256 value:

```text
30a303f20e00e2f615555541a0446cb1685ccebf4fe36b3693c5182dae59dece
```

The files were also confirmed identical using `diff`.

### Interpretation

This was a deliberately controlled dictionary-recovery exercise. The wordlist contained the correct training password, so the exercise demonstrates the workflow rather than the practical difficulty of recovering a strong password.

---

# 🧪 11. Part G - Independent Student Exercise

An independent steganography exercise was completed using a student-selected password.

A personal hidden evidence file was created:

```text
kafayat_animashawun_hidden_note.txt
```

The actual contents of the file were:

```text
Name: Kafayat Omolara Animashawun
```

The original note was hashed using SHA-256.

The recorded hash was:

```text
266d1f3723c809d90989f081b2bcee15281c93d2bc9e3dae52066937cee237ed
```

The hash was recorded in:

```text
evidence/independent_note_sha256.txt
```

---

## 11.1 Independent Stego Image

The independent note was embedded into a new carrier image using Steghide.

The student-selected password was:

```text
Forensics@2026
```

The resulting stego image was:

```text
kafayat_animashawun_stego.bmp
```

Its SHA-256 hash was:

```text
4bb04ef724409df9e2f620f2fdbee1fcf7f1d6a9532d3009e11b9c2da9f8ca1c
```

The hash was recorded in:

```text
evidence/independent_stego_sha256.txt
```

---

## 11.2 Independent Extraction

The independent hidden note was extracted using:

```bash
steghide extract \
-sf kafayat_animashawun_stego.bmp \
-xf ../extracted/kafayat_animashawun_extracted.txt \
-p 'Forensics@2026'
```

The recovered file was:

```text
extracted/kafayat_animashawun_extracted.txt
```

The extracted file contained:

```text
Name: Kafayat Omolara Animashawun
```

The original and extracted files produced the same SHA-256 hash:

```text
266d1f3723c809d90989f081b2bcee15281c93d2bc9e3dae52066937cee237ed
```

The files were also compared using `diff`, which confirmed that they were identical.

---

## 11.3 Independent StegSeek Recovery

A custom wordlist was created containing:

```text
Forensics@2026
```

The wordlist was saved as:

```text
working/independent_wordlist.txt
```

Its SHA-256 hash was:

```text
241f2c73c03fb2e8db423a9ccc2b09947ba4ec19da77341e27fdcdfc9a87ebdd
```

The hash was recorded in:

```text
evidence/independent_wordlist_sha256.txt
```

StegSeek was then used:

```bash
stegseek kafayat_animashawun_stego.bmp independent_wordlist.txt
```

The password was successfully recovered:

```text
Found passphrase: "Forensics@2026"
```

StegSeek also identified the original embedded filename:

```text
kafayat_animashawun_hidden_note.txt
```

The recovered output was verified using SHA-256 and `diff`.

---

# 🔐 12. Evidence Integrity

SHA-256 hashing was used throughout the practical to document file integrity.

The hashes recorded during the laboratory were:

| Evidence Item              | SHA-256                                                            |
| -------------------------- | ------------------------------------------------------------------ |
| Original carrier image     | `6508bebb94bd8ec0833b9f85580cda4261d89632ddc89e7f9f7093b5e653c4db` |
| `secret.txt`               | `30a303f20e00e2f615555541a0446cb1685ccebf4fe36b3693c5182dae59dece` |
| `tower_stego_lab4.bmp`     | `edcb2d6b27465546ddc0c303fc4f362f26da13fa0974b518bcec17af48936dd2` |
| `extracted_secret.txt`     | `30a303f20e00e2f615555541a0446cb1685ccebf4fe36b3693c5182dae59dece` |
| Independent hidden note    | `266d1f3723c809d90989f081b2bcee15281c93d2bc9e3dae52066937cee237ed` |
| Independent stego image    | `4bb04ef724409df9e2f620f2fdbee1fcf7f1d6a9532d3009e11b9c2da9f8ca1c` |
| Independent extracted note | `266d1f3723c809d90989f081b2bcee15281c93d2bc9e3dae52066937cee237ed` |
| `lab4_wordlist.txt`        | `a883dafc480d466ee04e0d6da986bd78eb1fdd2178d04693723da3a8f95d42f4` |
| `independent_wordlist.txt` | `241f2c73c03fb2e8db423a9ccc2b09947ba4ec19da77341e27fdcdfc9a87ebdd` |

The matching SHA-256 values between the original and recovered evidence files support the conclusion that the extraction process successfully recovered the original content without alteration.

---

# 📁 13. Repository Structure

The laboratory artifacts are organised as follows:

```text
Steganography-and-Hidden-Data-Detection/
│
├── evidence/
│   ├── extracted_secret_sha256.txt
│   ├── independent_note_sha256.txt
│   ├── independent_stego_sha256.txt
│   ├── independent_wordlist_sha256.txt
│   ├── lab4_wordlist_sha256.txt
│   ├── original_carrier_sha256.txt
│   ├── secret_sha256.txt
│   └── stego_image_sha256.txt
│
├── extracted/
│   ├── extracted_secret.txt
│   └── kafayat_animashawun_extracted.txt
│
├── original/
│   └── tower_original_image_for_lab.bmp
│
├── working/
│   ├── independent_wordlist.txt
│   ├── kafayat_animashawun_hidden_note.txt
│   ├── kafayat_animashawun_stego.bmp
│   ├── kafayat_animashawun_stego.bmp.out
│   ├── lab4_wordlist.txt
│   ├── secret.txt
│   ├── tower_stego_lab4.bmp
│   └── tower_stego_lab4.bmp.out
│
└── README.md
```

The screenshots captured during the practical were retained separately in the local laboratory environment and were not included in the GitHub repository at this stage.

A formal PDF report may subsequently be added under:

```text
report/
```

---

# 📊 14. Evidence Collection Summary

| Evidence Item                         | Purpose                                       |
| ------------------------------------- | --------------------------------------------- |
| `tower_original_image_for_lab.bmp`    | Preserved original carrier                    |
| `secret.txt`                          | Controlled hidden evidence                    |
| `secret_sha256.txt`                   | SHA-256 record for original secret            |
| `tower_stego_lab4.bmp`                | Steghide output containing concealed evidence |
| `extracted_secret.txt`                | Recovered controlled evidence                 |
| `extracted_secret_sha256.txt`         | SHA-256 record for recovered evidence         |
| `lab4_wordlist.txt`                   | Controlled password dictionary                |
| `kafayat_animashawun_hidden_note.txt` | Independent student-created evidence          |
| `kafayat_animashawun_stego.bmp`       | Independent steganographic output             |
| `kafayat_animashawun_extracted.txt`   | Recovered independent evidence                |
| `independent_wordlist.txt`            | Independent password dictionary               |
| SHA-256 evidence files                | Integrity documentation                       |

---

# 🔍 15. Forensic Observations

The practical demonstrated the following:

1. Information can be concealed within a digital image without necessarily producing an obvious visual difference.
2. The original carrier should be preserved before performing any modification or embedding operation.
3. Steghide can be used to embed password-protected information into a supported image carrier.
4. The resulting stego image can retain the same file size and dimensions as the original carrier.
5. Identical file size does not mean that two files are digitally identical.
6. SHA-256 hashes can demonstrate whether two files contain identical digital content.
7. The original and stego images produced different SHA-256 values.
8. The first 64 bytes examined with `xxd` were identical, demonstrating that the difference was not necessarily visible in the initial file header.
9. Basic `strings` analysis did not directly reveal the concealed evidence.
10. Successful extraction using Steghide recovered the original controlled evidence.
11. Matching SHA-256 hashes and successful `diff` comparisons verified the recovered files.
12. StegSeek successfully recovered the deliberately supplied laboratory passwords using controlled wordlists.
13. The independent exercise demonstrated that the same methodology could be applied using a student-selected password.
14. Forensic conclusions should be based on recorded evidence, hashes and reproducible commands rather than assumptions.

---

# ⚠️ 16. Limitations

This exercise was conducted within a controlled training environment.

The password-recovery demonstrations used deliberately small wordlists containing the correct laboratory passwords:

```text
1234
```

and:

```text
Forensics@2026
```

The results therefore demonstrate the mechanics of dictionary-based password recovery rather than the difficulty of recovering a strong randomly generated password.

The `strings` and `xxd` examinations were basic file-level inspection techniques and were not intended to constitute a comprehensive steganalysis methodology.

The analysis was limited to the authorised carrier image and controlled files created for the practical.

---

# 🛠️ 17. Tools Used

The following tools and utilities were used:

* **Steghide** - steganographic embedding and extraction
* **StegSeek** - controlled steganographic password recovery
* **SHA-256 / `sha256sum`** - cryptographic integrity verification
* **`diff`** - content comparison
* **`xxd`** - hexadecimal file inspection
* **`strings`** - printable string extraction
* **Linux command-line utilities** - file and directory management
* **Kali Linux** - laboratory operating environment
* **Git / GitHub** - evidence organisation and version control

---

# 🧾 18. Reproducibility

The principal commands used during the practical included:

### File Identification

```bash
file tower_original_image_for_lab.bmp
```

### SHA-256 Hashing

```bash
sha256sum filename
```

### Steghide Embedding

```bash
steghide embed \
-ef secret.txt \
-cf ../original/tower_original_image_for_lab.bmp \
-sf tower_stego_lab4.bmp \
-p 1234
```

### Steghide Extraction

```bash
steghide extract \
-sf tower_stego_lab4.bmp \
-xf ../extracted/extracted_secret.txt \
-p 1234
```

### File Comparison

```bash
diff secret.txt ../extracted/extracted_secret.txt
```

### Hexadecimal Inspection

```bash
xxd -l 64 tower_original_image_for_lab.bmp
xxd -l 64 tower_stego_lab4.bmp
```

### String Analysis

```bash
strings tower_original_image_for_lab.bmp
strings tower_stego_lab4.bmp
```

### StegSeek Dictionary Testing

```bash
stegseek tower_stego_lab4.bmp lab4_wordlist.txt
```

### Independent StegSeek Testing

```bash
stegseek kafayat_animashawun_stego.bmp independent_wordlist.txt
```

---

# 🎓 19. Academic Integrity Statement

All evidence presented in this repository was generated or handled as part of the authorised computer and digital forensics laboratory exercise.

The practical was performed within a controlled training environment using an authorised carrier image and files created specifically for the exercise.

No unauthorised personal, organisational or third-party data was intentionally examined or used.

The passwords used during the laboratory were controlled training credentials created solely for demonstrating steganographic embedding, extraction and dictionary-based recovery.

The evidence files, cryptographic hashes and documented commands are intended to provide a reproducible record of the practical activities performed.

---

# ✅ 20. Conclusion

This laboratory provided practical experience in the identification, creation, extraction and verification of concealed information using steganographic techniques.

The exercise demonstrated a complete workflow beginning with preservation of an original carrier image, followed by creation and hashing of controlled evidence, Steghide embedding, extraction, SHA-256 verification, file comparison, hexadecimal and string inspection, and controlled StegSeek password-recovery testing.

The independent exercise further demonstrated the ability to apply the same methodology using a student-created evidence file and a student-selected password.

The matching hashes between the original and extracted evidence files demonstrated successful recovery and content integrity.

The practical reinforces several important forensic principles:

* Preserve original evidence.
* Work from copies rather than modifying originals.
* Record cryptographic hashes.
* Use reproducible commands.
* Verify recovered evidence independently.
* Distinguish between observation and assumption.
* Document limitations.
* Maintain an organised evidence structure.

Overall, the exercise demonstrated how steganographic techniques can be investigated using a combination of specialised forensic tools and standard Linux analysis utilities.

---

## 📚 Repository

This repository contains the practical laboratory artifacts, including:

```text
original/
working/
extracted/
evidence/
```

The formal forensic report can be added to the repository after completion of the report-generation stage.

---

**Laboratory:** SBT-DF202 - Computer and Digital Forensics
**Practical:** Lab 4 - Steganography and Hidden Data Detection
**Environment:** Kali Linux
**Focus:** Steganographic data hiding, extraction, integrity verification and controlled password recovery
