# Group Policy – Windows Server 2019 Security Hardening

## Overview

This project demonstrates the application of **Group Policy Objects (GPOs)** within a simulated enterprise environment to enhance security, automate configuration, and enforce compliance on Windows Server systems. 

As part of Structureality Inc.'s internal security team, I implemented and validated organizational-level policies targeting a new Sales division. This lab aligns with **CompTIA Security+ objectives 4.5 and 4.7**, emphasizing enterprise security enhancement and automation.

---

## Tools & Technologies

- **Windows Server 2019**
- **Group Policy Management Console (GPMC)**
- **Active Directory Users and Computers (ADUC)**
- **Command Line Tools:** `gpresult`, `rsop`
- **Scripting:** `.cmd` batch files, `.txt` scripts
- **Virtual Environment:** DC10 (Domain Controller), PC10 (Client Machine)

---

## Objectives

- Establish a new Organizational Unit (OU)
- Apply and test Group Policy Objects (GPOs)
- Define account lockout policies and file system preferences
- Automate system behavior via logon/logoff scripts
- Validate policy propagation using administrative tools

---

## What I Did

### Organizational Unit Setup
- Created `SalesClients` OU in `ad.structureality.com`
- Migrated users (`Dani`, `Cam`) and a client machine (`PC10`) into the new OU
> - ![SalesClientsOU-in-ADUC](https://github.com/user-attachments/assets/bb533744-87d9-478d-9c5b-fe0bf3b15e09)
### GPO Creation and Linking
- Created `SalesPolicy` GPO linked to `SalesClients`
- Defined key settings under:
  - **Computer Configuration:** Account Lockout Policy (`3` invalid attempts)
  - **User Configuration:** Folder creation (`C:\SalesDocs`)
> - ![GPO-settings-in-GPMC](https://github.com/user-attachments/assets/0e7a720c-efd0-4d46-86db-92392fb444fd)
> - ![GPO-settings](https://github.com/user-attachments/assets/bf357418-ad0c-4419-a1f6-556e9ccb49f8)
> - ![SalesDocsFolder](https://github.com/user-attachments/assets/40a97dfd-cedb-4422-86ba-b2d5e9c66ae1)
### Script Automation
- Wrote basic `logon.txt` and `logoff.txt` messages
- Converted and deployed scripts as `.cmd` files via a network share
- Configured GPO to execute scripts at user logon/logoff
> - ![Script-creation](https://github.com/user-attachments/assets/1af50729-34fc-45e9-826f-a52a78152a12)
> - ![Scriptcmd](https://github.com/user-attachments/assets/2d6b958a-887d-4025-8a14-db56f90b9743)
> - ![LogOnGP](https://github.com/user-attachments/assets/7b9f87db-db60-4494-8c27-c5faa47ba806)


### Policy Validation
- Used `gpresult /r` to verify policy enforcement
- Ran `rsop.msc` to confirm policy source and status
- Verified `SalesDocs` folder creation and script execution
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

## Key Learnings

- Reinforced how **GPOs enforce security compliance** without manual user intervention
- Practiced **centralized administration** of scripts and system configurations
- Understood the **importance of OU and GPO hierarchy** for scalable IT operations
- Gained hands-on experience using `gpresult` and `rsop` to debug and verify policies


## Future Improvements

- Implement **PowerShell-based logon/logoff scripts** for more dynamic automation
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
