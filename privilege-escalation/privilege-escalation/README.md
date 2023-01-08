---
description: General Privilege escalation page
---

# Privilege escalation

Linux: [GTFOBins](https://gtfobins.github.io/)

Windows: [LOLBAS](https://lolbas-project.github.io)

| Windows                          | Linux                                  |
| -------------------------------- | -------------------------------------- |
| Missing security patches         | Missing security patches               |
| Kernel exploits                  | Kernel exploits                        |
| Vulnerable programs and services | Vulnerable services                    |
| Common misconfigurations         | Common misconfiguraions                |
| Insecure credentials             | Insecure credentials                   |
|                                  | Exploiting Super User Do (SUDO)        |
|                                  | Exploiting Set User ID (SUID) binaries |



## Types of privilege escalation attack

### Kernel exploits

Are designed to exploit vulnerabilities in the underlying kernel, to execute arbitrary code with elevated or "root" permissions.

### SUID binaries

SUID is an inbuilt Linux feature that allows users to execute binaries and files with the permissions of other users.

### Exploiting vulnerable services and permissions

Attackers will typically aim to identify misconfigured or vulnerable services and programs that could facilitate the escalation of privileges. For example, on Linux systems, attackers will try to identify and exploit misconfigurations with **cron jobs** and leverage the functionality to execute arbitrary code or malicious binaries.&#x20;

Exploiting vulnerable or insecure services on Windows typically involves embedding a payload in a service with administrative privileges. When the service is executed, it executes a payload with the administrative privileges, therefore allowing the binary to execute commands with "root" privileges.

### Insecure credentials

This technique involves searching for insecure credentials that have been stored on a system by users or by carrying out a process of cracking weak user credentials.

### Exploiting SUDO

Attackers will usually target users who have SUDO privileges. SUDO allows users to run commands as another user, typically the root user. SUDO privileges are usually configured manually by administrators, which leaves the door open to potential misconfigurations. For example, an administrator can assign SUDO permissions to a non-root user for certain command-line utilities (such as find or vim) that can run shell commands or arbitrary code.



