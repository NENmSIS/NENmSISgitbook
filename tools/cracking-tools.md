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

### Hashcat

<pre class="language-bash"><code class="lang-bash">hashcat -m 0 -a 0 target_hashes.txt /usr/share/wordlists/rockyou.txt
<strong>#-m 0 designates the type of hash we are cracking (MD5)
</strong>#-a 0 designates a dictionary attack
#target_hashes.txt is our input file of hashes
#/usr/share/wordlists/rockyou.txt is the absolute path to the wordlist file for this dictionary attack

</code></pre>
