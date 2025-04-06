# Group Policy Objects (GPOs) – Windows Server 2019 Security Hardening

## Project Summary

This lab demonstrates the implementation and enforcement of Group Policy Objects (GPOs) within an Active Directory environment using Windows Server 2019. Acting as a security analyst for Structureality Inc., I applied security configurations to a newly created Organizational Unit (OU) in alignment with enterprise security policies.

The focus areas include account policy hardening, folder provisioning, and automation of logon/logoff scripts using GPOs. This hands-on implementation maps directly to CompTIA Security+ objectives related to secure configuration and operational automation.

---

## Objectives

- Establish a new Organizational Unit (OU)
- Apply and configure linked Group Policy Objects (GPOs) with security settings
- Define account lockout policies and file system preferences
- Automate system behavior with logon and logoff scripts
- Validate policy propagation using administrative tools
- Demonstrate understanding of Active Directory and Group Policy structures


---


## Environment

- **Domain Controller:** DC10 (Windows Server 2019)
- **Client Machine:** PC10 (Windows Server 2019)
- **Domain:** `ad.structureality.com`


---


## Tools & Technologies

- **Windows Server 2019**
- **Active Directory Users and Computers (ADUC)**
- **Group Policy Management Console (GPMC)**
- **Command Line Tools:** `gpresult`, `rsop`
- **Scripting:** `.cmd` batch files, `.txt` scripts
- **Network shares for script deployment**


---

## What I Did

### 1. Organizational Unit Setup
- Created `SalesClients` OU in `ad.structureality.com` domain
- Migrated the following assets into the OU:
  - User: `Dani`
  - Admin: `Cam`
  - Computer: `PC10`


### 2. Group Policy Object (GPO) Creation and Linking
- Created a GPO named `SalesPolicy` and linked it to the `SalesClients` OU.
- Configured the following settings:
  - **Computer Configuration:** Account Lockout Policy (`3` invalid attempts)
  - **User Configuration:** Folder Provisioning - Created `C:\SalesDocs` on all OU computers via user configuration.
> - ![GPO-settings-in-GPMC](https://github.com/user-attachments/assets/0e7a720c-efd0-4d46-86db-92392fb444fd)
> - ![GPO-settings](https://github.com/user-attachments/assets/bf357418-ad0c-4419-a1f6-556e9ccb49f8)
> - ![SalesDocsFolder](https://github.com/user-attachments/assets/40a97dfd-cedb-4422-86ba-b2d5e9c66ae1)


### 3. Script Automation
- Created and shared `\\DC10\scripts\` directory with full control permissions for all users.
- Created:
  - `logon.txt` – Displays a logon message
  - `logoff.txt` – Displays a logoff message
  - `scriptlogon.cmd` – Echoes contents of `logon.txt`
  - `scriptlogoff.cmd` – Echoes contents of `logoff.txt`
- Scripts deployed via GPO using UNC path references to support distributed application.
> - ![Script-creation](https://github.com/user-attachments/assets/1af50729-34fc-45e9-826f-a52a78152a12)
> - ![Scriptcmd](https://github.com/user-attachments/assets/2d6b958a-887d-4025-8a14-db56f90b9743)
> - ![LogOnGP](https://github.com/user-attachments/assets/7b9f87db-db60-4494-8c27-c5faa47ba806)


### 4. Policy Validation
- Verified user policy application using `gpresult /r` for both standard and admin users.
- Used `rsop.msc` to inspect the effective policy and confirm:
  - `SalesPolicy` as the source GPO for account lockout policies
  - Folder provisioning was successful
- Observed expected behavior upon logon/logoff:
  - Notepad opens with appropriate messages based on user action
> - ![gpresult-r-output](https://github.com/user-attachments/assets/697b52cf-4fc8-4f10-9c07-9125f1d221fa)
> - ![gpresult-r-output2](https://github.com/user-attachments/assets/dc65cc2a-25d6-4fa4-ac42-4a92e5383d83)
> - ![rsop-policy-report](https://github.com/user-attachments/assets/f88cc824-1f99-40fd-9f2c-da37feb87bc5)
> - ![Script-output-in-Notepad](https://github.com/user-attachments/assets/62b36936-eadf-456e-b6b3-d41671479555)
---

## How to Reproduce

### Pre-requisites:
- Two Windows Server 2019 VMs: one domain controller (DC10), one client (PC10)
- Domain setup: `ad.structureality.com`

### Steps:
1. **Configure Active Directory:**
   - Create OU
   - Move users and computers
2. **Apply Group Policy:**
   - Create GPO (`SalesPolicy`)
   - Define policies (e.g., lockout thresholds, folders)
3. **Deploy Scripts:**
   - Share `C:\scripts` folder on DC10
   - Create `.cmd` scripts pointing to shared `.txt` files
   - Configure scripts in GPO
4. **Test & Validate:**
   - Log in as standard and admin users
   - Use `gpresult` and `rsop`
   - Observe logon/logoff behavior

---

---

## Key Takeaways

- Reinforced how **GPOs enforce security compliance** without manual user intervention
- Practiced **centralized administration** of scripts and system configurations
- Understood the **importance of OU and GPO hierarchy** for scalable IT operations
- Gained hands-on experience using `gpresult` and `rsop` to debug and verify policies


## Future Improvements

- Implement **PowerShell-based logon/logoff scripts** for more dynamic automation
- Extend the GPO to include additional security settings (e.g., Windows Firewall, audit policy).
- Use **WMI filters** to target specific system types within the OU
- Integrate **security baselines** from Microsoft or CIS for production-like hardening
- Configure **software deployment policies** to push vetted apps to Sales division machines

---

## Key Security+ Objectives Demonstrated

| Objective | Description |
|----------|-------------|
| 4.5 | Modify enterprise capabilities to enhance security (e.g., GPOs, account lockout) |
| 4.7 | Implement automation/orchestration for secure operations (e.g., logon/logoff scripts) |

---

## Quick Knowledge Check (with Answers)

> Use these to test your knowledge or interview prep:

**Q:** What can be set as a member of an OU?  
**A:** Computers, Users

**Q:** Can a GPO be linked to an OU?  
**A:** Yes, a GPO can be linked to an OU

**Q:** What can GPOs affect?  
**A:** Either users, computers, or both

**Q:** What does `gpresult` do?  
**A:** Displays current GPOs affecting the system

**Q:** What tool shows effective GPO configuration?  
**A:** Resultant Set of Policy (RSOP)

---


## Connect with Me

**LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/pranab-karki/)

**Personal Website**: [Website](https://pranabka.github.io/)
