---
description: Linux, SQLi (postgreSQL),FTP, john,
---

# Vaccine

### Process

* Obtain using "Anonymous" user in FTP the backup.zip with a password of the web application. We use zip2john and crack the password. In that file, there are the login credentials for the web application.
* Connect to the Web app and discover a search bar. There is a SQLi vulnerability using `'` so after applying the burp academy technics we discover that is Postgresql and the version. Obtain a reverse shell crating a table with cmd code.
* In the reverse shell, locate the user flag. After use `sudo -l`to discover what can we execute as sudo. Use GTFOBins to elevate privileges. Discover the root flag.
