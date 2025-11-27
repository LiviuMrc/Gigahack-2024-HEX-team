# GigaHack 2024 - Cybersecurity Challenge Winner

**Team HEX** - 1st Place Solution

## Team Members
- Mereacre Liviu
- Mocrenco Artiom
- Brinzila Calin
- Nicu Iurie
- Gidilica Nichita

## Challenge Overview

In this cybersecurity CTF challenge, we were presented with a futuristic scenario set in 2042. As Interpol agents, our mission was to track down stolen digital art NFTs from an underground crypto trading network. The only lead was an "ancient storage device" (USB flash drive) belonging to a gang leader known as "Byron."

**Objective:** Extract maximum useful information from the provided USB device to recover stolen NFTs.

## Solution Summary

Our winning solution involved multiple stages of cryptanalysis, reverse engineering, and forensic investigation:

### 1. Device Analysis
The USB drive contained two partitions:
- **Partition 1:** Source folder with photos + Validator binary
- **Partition 2:** Encrypted files, SSH keys, and duplicate images

### 2. Binary Reverse Engineering
We decompiled the Validator binary and discovered:
- Infinite loop trap (removed)
- ROT13 encoding scheme
- Environment variable check: `ADA=Lovelace` (reference to Ada Lovelace, the first programmer)
- SHA256 hash validation comparing filenames to file contents

### 3. Steganographic Analysis
By comparing image filenames with their actual SHA256 hashes, we extracted hidden data:
- **KeyPass folder differences:** Decoded to "Cardano" (blockchain platform)
- **Remote folder differences:** Revealed IP address `194.180.251.125` (Moldova-based server)

### 4. Network Penetration
- Port scan revealed SSH on port 1788
- Authenticated using discovered credentials:
  - Username: `byron`
  - Password: `Cardano`
  - SSH keys from partition 2

### 5. Cryptographic Attack
Found encrypted files on the server:
- Performed **known-plaintext attack** using `profile.aes` encryption example
- Recovered password: `Lovelace`
- Decrypted `blazarlabs.aes` containing wallet seed phrase

### 6. Asset Recovery
Successfully accessed the Cardano wallet containing:
- **Wisdom Proxies NFT** (Floor price: 44 ADA)
- **Cardano Proxies NFT** (Floor price: 32 ADA)
- ~198 ADA tokens (~$78 USD)

## Technical Skills Demonstrated

- **Reverse Engineering:** .NET binary decompilation and analysis
- **Cryptanalysis:** ROT13 decoding, known-plaintext attacks, AES decryption
- **Digital Forensics:** File hash analysis, steganography detection
- **Network Security:** Port scanning, SSH authentication
- **OSINT:** LinkedIn reconnaissance for context clues
- **Blockchain:** Cardano wallet recovery and NFT transfer

## Key Techniques

1. **Hash Comparison Forensics**
   ```bash
   sha256sum [filename] # Compared actual hash vs filename
   ```

2. **ROT13 Decoding**
   - `NQN` → `ADA`
   - `Ybirynpr` → `Lovelace`

3. **Hex to ASCII Conversion**
   - Extracted hidden strings from hash differences
   - Reconstructed IP address from reversed byte order

4. **Known-Plaintext Attack**
   - Used bash history showing `openssl aes-128-cbc -in .profile -out profile.aes`
   - Deduced encryption password from context clues

## Tools Used

- **ILSpy / dnSpy** - .NET decompilation
- **OpenSSL** - AES decryption
- **nmap** - Network reconnaissance
- **sha256sum** - Hash verification
- **Excel** - Data visualization and pattern recognition
- **SSH** - Remote access

## Lessons Learned

This challenge showcased the importance of:
- Methodical enumeration of all available data
- Cross-referencing multiple clues (Ada Lovelace theme)
- Understanding cryptographic implementations
- Persistence in pattern recognition

## Competition Details

- **Event:** GigaHack 2024
- **Category:** Cybersecurity Challenge
- **Result:** 1st Place
- **Location:** Moldova
- **Date:** September 2024

---

**Disclaimer:** This repository is for educational purposes, documenting our CTF competition solution. All activities were performed within the authorized scope of the competition.
