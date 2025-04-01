# AIG Shields Up: Cybersecurity Virtual Experience  

## Project Overview  
This project was completed as part of the **AIG Shields Up: Cybersecurity Virtual Experience Program on Forage (February 2025)**. It involved analyzing critical cybersecurity threats, drafting security advisories, and developing an ethical hacking script to brute-force decrypt ransomware-encrypted files.  

## Key Objectives  
- **Threat Analysis:** Assessed the **Log4Shell (CVE-2021-44228)** and other **Log4j vulnerabilities (CVE-2021-45046, CVE-2021-45105)**, understanding their risks and exploitation methods.  
- **Security Advisory:** Drafted a professional security email to guide the Product Development team on remediation strategies.  
- **Ethical Hacking & Cryptography:** Used Python to develop a brute-force decryption script, leveraging the **RockYou password list** to recover encrypted files.  

## Technologies & Tools  
- **Python 3+** (for brute-force decryption script)  
- **Cybersecurity Frameworks & Best Practices**  
- **CISA Threat Intelligence Reports**  
- **RockYou Password List** (common passwords used for dictionary attacks)  
- **ZipFile Module** (for handling encrypted ZIP files)  

## Project Breakdown  

### 1. Cyber Threat Analysis  
- Investigated **Log4j vulnerabilities**, widely exploited by threat actors for remote code execution (RCE).  
- Researched **CISA advisories** and mitigation strategies for organizations.  
- Evaluated the impact of Log4Shell on enterprise security.  

### 2. Security Advisory Communication  
- Drafted a clear and concise email to the **Product Development team**, detailing the vulnerabilities, risks, and remediation steps.  
- Provided actionable guidance on **patching Log4j, threat hunting, and incident response**.  

### 3. Brute-Force Decryption Script  
- Developed a Python script to **brute-force the password of an encrypted ZIP file**, simulating ransomware decryption without paying ransom.  
- Utilized **RockYou.txt**, a well-known password dictionary, for testing common passwords.  
- Automated password attempts using Python's **ZipFile module**.  

##  Brute-Force Script Code Example  
```python
'''
Bruteforce script to crack encrypted ZIP file
'''

from zipfile import ZipFile
import sys

# Function to attempt to extract the ZIP file with a given password
def attempt_extract(zf_handle, password):
    try:
        zf_handle.extractall(pwd=password.strip())
        print(f"[+] Password found: {password.decode().strip()}")
        return True
    except:
        return False

def main():
    print("[+] Beginning bruteforce attack...")
    
    # Open the encrypted ZIP file
    with ZipFile('enc.zip') as zf:
        # Open the wordlist file
        with open('rockyou.txt', 'rb') as f:
            for password in f:
                if attempt_extract(zf, password):
                    print("[+] Successfully extracted files!")
                    return  # Exit after finding the correct password
    
    print("[!] Password not found in the provided list.")

if __name__ == "__main__":
    main()
```
### Key Takeaways
- Strengthened cyber threat analysis and incident response skills.
- Gained hands-on experience with password cracking techniques used by ethical hackers.
- Improved technical communication by drafting real-world security advisories.
- Developed a deeper understanding of ransomware mitigation strategies.

### Next Steps
- Expand the brute-force script to include multi-threading for faster decryption.
- Implement log analysis for detecting unauthorized access attempts.
- Research alternative encryption-breaking techniques used in forensic investigations.

## Connect with Me

**LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/pranab-karki/)

**Personal Website**: [Website](https://pranabka.github.io/)
 

