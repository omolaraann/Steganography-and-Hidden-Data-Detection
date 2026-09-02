## 1. Lab Overview

This practical laboratory focuses on the identification, creation, extraction, and verification of concealed information using steganographic techniques.

The exercise was conducted within an authorised laboratory environment using the designated carrier image and files personally created for the exercise.

The practical demonstrates the use of:

* Steganography concepts
* Substitution-based data hiding
* Least Significant Bit (LSB) techniques
* Steghide
* StegSeek
* SHA-256 cryptographic hashing
* `diff`
* `xxd`
* `strings`
* Controlled password dictionary testing

The objective was to understand how information can be concealed within an image, how concealed information can be extracted, and how cryptographic hashes can be used to verify the integrity of recovered evidence.

---

## 2. Objectives

The objectives of this laboratory were to:

1. Explain the difference between steganography and encryption.
2. Understand insertion and substitution-based hiding techniques.
3. Understand the Least Significant Bit (LSB) method of data hiding.
4. Prepare and preserve an authorised carrier image.
5. Create and hash a controlled evidence file.
6. Embed the evidence file into a new image using Steghide.
7. Extract the concealed evidence using the correct password.
8. Verify the integrity of the extracted evidence using SHA-256 hashes.
9. Compare the original and stego images using file size, hashes, hexadecimal output and readable strings.
10. Perform a controlled dictionary password-recovery test using StegSeek.
11. Complete an independent steganography exercise using a student-selected password.
12. Document the practical activities using screenshots and command evidence.

---

## 3. Authorised Laboratory Environment

All activities were performed using files created or supplied specifically for the authorised laboratory exercise.

No unauthorised systems, devices, images or third-party data were used.

The original carrier image was preserved and was not overwritten during the embedding process. A separate stego image was created for the experiment.

---

# 4. Steganography Concepts

## 4.1 What is Steganography?

Steganography is the practice of concealing information within another file or medium so that the existence of the hidden information is not immediately apparent.

For example, a text file can be concealed within an image. The image can continue to appear visually normal while containing additional hidden data.

In digital forensics, steganography is relevant because malicious actors may use it to conceal documents, commands, credentials, malware or other information within apparently harmless files.

---

## 4.2 Steganography vs Encryption

Steganography and encryption provide different forms of information protection.

**Encryption** transforms readable information (plaintext) into an unreadable form (ciphertext) using a cryptographic algorithm and usually a key.

**Steganography** focuses primarily on hiding the existence of the information by embedding it inside another file.

Therefore:

| Technique                  | Primary Purpose                                            |
| -------------------------- | ---------------------------------------------------------- |
| Encryption                 | Makes information unreadable without the appropriate key   |
| Steganography              | Conceals the existence of information                      |
| Steganography + Encryption | Can conceal information while also protecting its contents |

---

## 4.3 Substitution and LSB

One common steganographic approach is substitution.

In substitution-based steganography, selected bits or other elements of a carrier file are replaced with bits representing the hidden information.

The **Least Significant Bit (LSB)** is particularly useful because changing the least significant bit of a pixel value generally produces only a very small change in the numerical value.

For example:

```text
Original:  10110110
Modified:  10110111
                       ↑
                  LSB changed
```

Because the change is very small, the resulting image may appear visually identical or nearly identical to the original image.

However, at the digital level, the modified file can have different binary content and therefore a different cryptographic hash.

---

# 5. Part A — Evidence Preparation and Preservation

A dedicated working directory was created for the laboratory.

The authorised carrier image was downloaded and preserved as the original evidence item.

The following information was recorded:

| Evidence Item       | Description                            |
| ------------------- | -------------------------------------- |
| Carrier Image       | Authorised laboratory BMP image        |
| File Type           | BMP image                              |
| Original Filename   | `tower_original_image_for_lab.bmp`     |
| Integrity Method    | SHA-256                                |
| Preservation Status | Original retained without modification |

### Commands Used

```bash
mkdir -p steganography_lab
cd steganography_lab
```

File type verification:

```bash
file tower_original_image_for_lab.bmp
```

SHA-256 calculation:

```bash
sha256sum tower_original_image_for_lab.bmp
```

The resulting SHA-256 value was recorded in the evidence/hash documentation.

### Evidence Screenshot

![Original image verification](screenshots/02-folder-preparation.png)

![Original image SHA-256](screenshots/03-original-image-hash.png)

---

# 6. Part B — Creation of Controlled Evidence File

A controlled evidence file named `secret.txt` was created for the exercise.

The file contained:

* Student's full name
* A controlled evidence message
* Confirmation that the file was created for the ICDFA laboratory

Example:

```text
Name: Kafayat Omolara Animashawun

This is a controlled evidence message created for the ICDFA
Steganography and Hidden Data Analysis laboratory.

This file is intentionally created for the purpose of demonstrating
data hiding, extraction and forensic integrity verification.
```

The file was hashed before embedding.

```bash
sha256sum secret.txt
```

The resulting hash was preserved in the evidence records.

### Evidence

* `secret.txt`
* `secret_hash.txt`

### Screenshot

![Secret file creation and hashing](screenshots/04-secret-file-created.png)

![Secret file SHA-256](screenshots/05-secret-file-hash.png)

---

# 7. Part C — Steghide Embedding

The original carrier image was preserved.

The hidden evidence file was embedded into a **new stego image** using Steghide.

The guided laboratory password was:

```text
1234
```

The embedding command used was:

```bash
steghide embed \
-ef secret.txt \
-cf tower_original_image_for_lab.bmp \
-sf tower_stego_lab4.bmp \
-p 1234
```

The command embeds `secret.txt` into the carrier image and creates a separate output image named:

```text
tower_stego_lab4.bmp
```

The original carrier image remained unchanged.

The resulting stego image was then hashed:

```bash
sha256sum tower_stego_lab4.bmp
```

### Evidence Screenshot

![Steghide embedding](screenshots/06-steghide-embedding.png)

![Stego image hash](screenshots/07-stego-image-hash.png)

---

# 8. Part D — Extraction and Integrity Verification

The concealed evidence was extracted from the stego image using the correct password.

An extraction directory was created:

```bash
mkdir -p extracted
```

The extraction command was:

```bash
steghide extract \
-sf tower_stego_lab4.bmp \
-xf extracted/extracted_secret.txt \
-p 1234
```

The extracted file was then compared against the original `secret.txt`.

SHA-256 verification:

```bash
sha256sum secret.txt
sha256sum extracted/extracted_secret.txt
```

The files were also compared using:

```bash
diff secret.txt extracted/extracted_secret.txt
```

A successful `diff` with no output indicates that the contents are identical.

The matching SHA-256 values provide additional evidence that the recovered file has the same content as the original evidence file.

### Expected Verification

```text
secret.txt
SHA-256: <recorded hash>

extracted/extracted_secret.txt
SHA-256: <same recorded hash>
```

### Evidence Screenshot

![Successful extraction](screenshots/08-extraction.png)

![Hash and diff verification](screenshots/09-hash-verification.png)

---

# 9. Part E — Original and Stego Image Comparison

The original carrier image and the resulting stego image were compared using:

* File size
* SHA-256 hash
* Initial hexadecimal bytes
* Readable strings

## 9.1 File Size

File sizes were checked using:

```bash
ls -lh tower_original_image_for_lab.bmp
ls -lh tower_stego_lab4.bmp
```

The recorded values were documented in the forensic report.

---

## 9.2 SHA-256 Comparison

The hashes were calculated using:

```bash
sha256sum tower_original_image_for_lab.bmp
sha256sum tower_stego_lab4.bmp
```

The hashes were expected to differ because the stego image contains modified/embedded data.

Different SHA-256 hashes indicate that the two files are not digitally identical.

---

## 9.3 Initial Bytes Using xxd

The beginning of each image was examined using:

```bash
xxd -l 64 tower_original_image_for_lab.bmp
```

and:

```bash
xxd -l 64 tower_stego_lab4.bmp
```

The hexadecimal output was compared to determine whether the initial file structure differed.

---

## 9.4 Readable Strings

Readable strings were examined using:

```bash
strings tower_original_image_for_lab.bmp
```

and:

```bash
strings tower_stego_lab4.bmp
```

This provides a basic comparison of human-readable character sequences present within each file.

---

## 9.5 Interpretation

The original and stego images may appear visually similar because steganography can make relatively small changes to the underlying image data.

However, visual similarity does not mean that the files are digitally identical.

Even a small modification to a file can produce a completely different cryptographic hash. Therefore, SHA-256 comparison is useful in forensic analysis for demonstrating whether two files are byte-for-byte identical.

### Evidence Screenshots

![Image file size comparison](screenshots/10-image-comparison.png)

![XXD comparison](screenshots/11-xxd-comparison.png)

---

# 10. Part F — Controlled StegSeek Dictionary Test

A small controlled wordlist was created containing the laboratory password:

```text
1234
```

The wordlist was saved as:

```text
lab4_wordlist.txt
```

StegSeek was then used to test the password against the stego image.

Command:

```bash
stegseek tower_stego_lab4.bmp lab4_wordlist.txt
```

The purpose of this controlled test was to demonstrate how a dictionary-based password recovery workflow can be used against a steganographic file when the correct password is included in the supplied wordlist.

### Result

The result of the StegSeek test was recorded in the forensic report and supported with command-line evidence.

If the tool was unavailable or produced an error in the laboratory environment, the error message was preserved and documented rather than omitted.

### Evidence

* `lab4_wordlist.txt`
* StegSeek command output
* Screenshot of the recovery attempt

![StegSeek dictionary test](screenshots/12-stegseek-wordlist.png)

---

# 11. Part G — Independent Student Task

For the independent component of the practical, a personal hidden evidence file was created.

Filename:

```text
firstname_lastname_hidden_note.txt
```

The file contained a short explanation of steganography and its relevance to digital forensics.

Example content:

```text
Steganography is a technique used to conceal information within
another file or digital medium. It is important in digital forensics
because concealed information may contain evidence that is not
immediately visible during a conventional examination of a file.

This file was created as part of the ICDFA digital forensics
steganography laboratory.
```

A SHA-256 hash was calculated for the file.

```bash
sha256sum firstname_lastname_hidden_note.txt
```

The file was then embedded into a new carrier image using a student-selected password.

The resulting image was extracted again and the original and recovered files were compared using SHA-256.

A custom wordlist containing the selected password was also created and tested using StegSeek.

### Independent Task Evidence

The following evidence was collected:

* Student-created hidden note
* SHA-256 hash of the original note
* Custom stego image
* Extraction output
* SHA-256 hash of extracted note
* `diff` verification
* Custom password wordlist
* StegSeek recovery result
* Supporting screenshots

### Screenshots

![Independent steganography task](screenshots/13-independent-task.png)

![Independent extraction and verification](screenshots/14-independent-extraction.png)

---

# 12. Evidence Integrity

Cryptographic hashes were used throughout the laboratory to support evidence integrity.

SHA-256 was selected because it produces a fixed-length cryptographic digest that can be used to determine whether the contents of a file have changed.

The following items were hashed:

| Evidence                   | SHA-256 Recorded |
| -------------------------- | ---------------- |
| Original carrier image     | Yes              |
| `secret.txt`               | Yes              |
| Stego image                | Yes              |
| Extracted secret file      | Yes              |
| Independent hidden note    | Yes              |
| Extracted independent note | Yes              |

Where the original and extracted evidence files produced identical SHA-256 hashes, this supported the conclusion that the recovered file matched the original file.

---

# 13. Evidence Collection Summary

| Evidence Item                        | Purpose                             |
| ------------------------------------ | ----------------------------------- |
| `tower_original_image_for_lab.bmp`   | Preserved original carrier          |
| `secret.txt`                         | Controlled hidden evidence          |
| `secret_hash.txt`                    | SHA-256 record for secret file      |
| `tower_stego_lab4.bmp`               | Image containing concealed evidence |
| `extracted_secret.txt`               | Recovered hidden evidence           |
| `extracted_hash.txt`                 | Hash record for recovered evidence  |
| `lab4_wordlist.txt`                  | Controlled password dictionary      |
| `firstname_lastname_hidden_note.txt` | Independent student evidence        |
| Custom stego image                   | Independent steganography output    |
| Custom wordlist                      | Independent password-recovery test  |
| Screenshots                          | Activity and command evidence       |
| PDF report                           | Complete forensic documentation     |

---

# 14. Forensic Observations

The practical demonstrated that:

1. Information can be concealed within a digital image without necessarily producing an obvious visual difference.
2. The original carrier file should be preserved before performing any modification or embedding operation.
3. Hashing provides a reliable method for documenting file integrity.
4. A successfully extracted file can be verified against its source using SHA-256 and `diff`.
5. A stego image can have a different file size and cryptographic hash from its original carrier even when the images appear visually similar.
6. Hexadecimal and string analysis can provide additional information when comparing files.
7. Password-protected steganographic content can be subjected to controlled dictionary-based password testing when an appropriate wordlist is available.
8. Forensic conclusions should be based on recorded evidence and reproducible commands rather than assumptions.

---

# 15. Limitations

This exercise was conducted within a controlled training environment.

The password-recovery demonstration used a deliberately small laboratory wordlist and a known training password. It should therefore not be interpreted as representative of the difficulty of recovering a strong, randomly generated password.

The analysis was also limited to the authorised carrier image and files created for this practical.

---

# 16. Conclusion

This laboratory provided practical experience in identifying and working with concealed information using steganographic techniques.

The exercise demonstrated the complete workflow from preservation of the original carrier image through creation of controlled evidence, Steghide embedding, extraction, cryptographic hash verification, file comparison and controlled password dictionary testing.

The independent exercise further demonstrated the ability to apply the same forensic methodology to a student-created hidden evidence file.

The practical reinforces the importance of preserving original evidence, maintaining cryptographic integrity records, documenting commands and screenshots, and ensuring that forensic activities are repeatable and properly documented.

---

## 17. Supporting Evidence

The repository contains the following supporting materials:

```text
/evidence
/screenshots
/report
```

The complete forensic report is available in:

```text
report/Lab5_Steganography_Forensic_Report.pdf
```

All evidence files and screenshots included in this repository relate to the authorised ICDFA laboratory exercise.

---

## 18. Tools Used

* Steghide
* StegSeek
* SHA-256 / `sha256sum`
* `diff`
* `xxd`
* `strings`
* Linux command-line utilities
* GitHub for evidence organisation and submission

---

## 19. Academic Integrity Statement

All evidence presented in this repository was generated or handled as part of the authorised ICDFA laboratory exercise.

The practical was performed in a controlled training environment, and no unauthorised personal, organisational or third-party data was intentionally examined or used.
