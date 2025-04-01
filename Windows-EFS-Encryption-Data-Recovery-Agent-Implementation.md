# Windows EFS Encryption & Data Recovery Agent Implementation

## Project Overview
This project demonstrates the configuration and use of **Encrypting File System (EFS)** on Windows Server 2019, including:
- Creating a Data Recovery Agent (DRA) certificate
- Encrypting/decrypting files with EFS
- Simulating and recovering from lost encryption keys
- Understanding enterprise data protection strategies

## Key Objectives
- Implement cryptographic file-level protection  
- Configure disaster recovery mechanisms for encrypted data  
- Demonstrate security governance through access control  
- Compare data protection strategies  

## Lab Environment
- **System**: PC10 (Windows Server 2019)
- **Users**:
  - `Admin` (Local admin, DRA)
  - `Pat` (Standard user)
- **Folder Structure**:
  - `C:\certificates` (DRA keys)
  - `C:\SecReports` (Encrypted files)

---

## Step-by-Step Implementation

### 1. System Preparation
```powershell
# Removed PC10 from domain to simulate key loss scenario
Remove-Computer -UnjoinDomaincredential Administrator -Restart -Force
```
### 2. DRA Certificate Creation
```cmd
mkdir C:\certificates
cd C:\certificates
cipher /r:EFSRA
```
#### **Generated**:

- **EFSRA.CER** (Public key)

- **EFSRA.PFX** (Password-protected private key)

### 3. Group Policy Configuration

- Opened **mmc.exe** → Added Group Policy Object Editor

- Configured path:
```
Computer Configuration → Windows Settings → Security Settings → Public Key Policies → Encrypting File System
```
- Added **EFSRA.CER** as Data Recovery Agent

### 4. User Account Setup
```cmd
net user Pat Password1 /add
```

### 5. File Encryption Process

| File | Method| Status |
| :---         |     :---:      |          ---: |
| Jan-Security.txt   | GUI Encryption    | Encrypted (E)    |
| Feb-Security.txt     | CLI: cipher /e *-Security.txt       | Encrypted (E)     |
| Mar-Security.txt     | CLI: cipher /d       | Decrypted (U)    |

- Enabled color-coding in File Explorer (Green = Encrypted)

### 6. Simulated Key Loss
```cmd
# Changed password to invalidate EFS keys
net user Pat Password123
```
### 7. Recovery Process

- Installed DRA private key (EFSRA.PFX)

- Decrypted files:
```cmd
cipher /d Jan-Security.txt
```
- GUI recovery of Feb-Security.txt via Advanced Attributes

## Key Findings & Security Insights
### Cryptographic Solutions
- EFS provides file-level encryption without full-disk encryption overhead
- Uses public-key cryptography (certificate-based)
- Native NTFS integration maintains usability

## Recovery Mechanisms
### DRA requirements:

- Must be configured before encryption

- Requires both public (.CER) and private (.PFX) keys

- Password changes invalidate user EFS keys

### Protection Strategies

| Method | Pros| Cons |
| :---         |     :---:      |          ---: |
| EFS   | Granular control	    | No key escrow by default    |
| BitLocker     | 	Whole-disk protection       | Less granular    |
| DRA     | 	Disaster recovery       | Pre-configuration required    |

### Security Governance
- Demonstrated separation of duties (Admin vs. Pat)
- Implemented access control through cryptography
- Established recovery procedures for business continuity

## Technical Commands Cheat Sheet
```cmd
# Encryption
cipher /e filename.txt

# Decryption 
cipher /d filename.txt

# DRA Creation
cipher /r:DRAname

# Status Check
cipher
```
## Visual Documentation

### Jan-Security.txt   GUI Encryption
![JanSecGUI](https://github.com/user-attachments/assets/942afb5c-fe6b-46a5-ac84-3ef2b108ac55)

### CLI Cipher
![CLIcipher](https://github.com/user-attachments/assets/ff64549b-80eb-4391-b768-8d63092a42fb)

### Color Coded Encryption 
![ColorCodeE](https://github.com/user-attachments/assets/33892550-835e-45dd-8ea4-34f4394c6d62)

### Certificate Import
![CertificateImport](https://github.com/user-attachments/assets/553a35fd-1011-4ebb-985a-fe0e3f592549)

### Certificate CLI
![Certificate](https://github.com/user-attachments/assets/a75b5840-fdf0-455b-80b7-a2adb6c5bdcd)


## Lessons Learned
1. **Pre-Configuration is Critical**: DRA must be established before encryption

2. **Password Changes Break EFS**: Unexpected side-effect for user accounts

3. **Recovery Complexity**: Requires careful certificate management

4. **Audit Trails**: EFS maintains clear ownership records




## Connect

**LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/pranab-karki/)

**Personal Website**: [Website](https://pranabka.github.io/)




