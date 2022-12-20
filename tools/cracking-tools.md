# Cracking Tools

## John

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
```

#### Crack PGP private key

Isolate a file with the full private key `-----BEGIN PGP PRIVATE KEY BLOCK-----... clave...-----END PGP PRIVATE KEY BLOCK-----` in the `.keys`  file and do:

```bash
gpg2john .keys > gpg.john
john -w=/usr/share/wordlists/rockyou.txt gpg.john

```
