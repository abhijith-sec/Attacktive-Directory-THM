# Attacktive-Directory-THM
TryHackMe Attacktive Directory room write-up and notes

## Overview

Attacktive Directory is an Active Directory focused TryHackMe room that introduces enumeration, Kerberos attacks, and domain resource access.

## Attack Path

1. Service Enumeration
2. SMB Enumeration
3. Username Discovery
4. Kerberos Enumeration
5. AS-REP Roasting
6. Password Cracking
7. Domain Resource Access

---
## Step 1 - Service Enumeration

### Objective

Identify exposed services and determine whether the target is an Active Directory Domain Controller.

### Command

```bash
nmap -A <target-ip>
```

### Findings

Observed services:

- Kerberos (88)
- LDAP (389)
- SMB (445)
- SMB (139)
- DNS (53)

These services strongly indicated an Active Directory environment.

### Screenshot

![Nmap](https://github.com/abhijith-sec/Attacktive-Directory-THM/blob/main/screenshots/01_nmap.png) 
![Nmap](https://github.com/abhijith-sec/Attacktive-Directory-THM/blob/main/screenshots/02_nmap.png))

## Step 2 - Host Resolution Configuration

### Objective

Configure local hostname resolution to ensure Active Directory and Kerberos tools can correctly communicate with the target domain.

### Enumeration Findings

During service enumeration, Nmap revealed:

```text
DNS_Domain_Name: spookysec.local
DNS_Computer_Name: AttacktiveDirectory.spookysec.local
```

### Command

```bash
sudo nano /etc/hosts
```

### Entry Added

```text
10.48.182.217 spookysec.local AttacktiveDirectory.spookysec.local
```

### Verification

```bash
ping spookysec.local
```

### Why This Was Necessary

Many Active Directory tools, including Kerberos and Impacket utilities, rely on proper DNS resolution. Adding the domain information to `/etc/hosts` ensured that the attack machine could correctly resolve the domain controller hostname and communicate with domain services.

### Result

The domain `spookysec.local` successfully resolved to the target IP address, enabling further Active Directory enumeration.

