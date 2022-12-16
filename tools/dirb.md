# Dirb

For all the info: `dirb` or [https://www.kali.org/tools/dirb/](https://www.kali.org/tools/dirb/)

***

* Default usage: `dirb http://IP/`
* Use with wordlist: `dirb http://IP/ /usr/share/dirb/wordlists/small.txt`
* With a specific user agent: `-a "useragent"`
  * [http://useragentstring.com/pages/useragentstring.php](http://useragentstring.com/pages/useragentstring.php)
* With a specific cookie: `-c "COOKIE:XYZ"`
* With username and password: `-u "username:password"`
* Not recursive: `-r` If not set, once it found one directory, it continue to that directory only. This is usefull if we need all the directories before digging deeper.
* Silent mode: `-S` if used, only shows the found directories.
* Extensions `-X ".php,.bak"` or `-x filewithextensions`. It also search for a file with these extensions for every directory.
* Save results into a file: `o file`

### With Burp Proxy[​](broken-reference) <a href="#with-burp-proxy" id="with-burp-proxy"></a>

First, we open Burp Suite Intercept to off We specify the proxy: `dirb http://IP/ -p http://127.0.0.1:8080` Example:

```
dirb http://192.15.42.3 /usr/share/dirb/wordlists/small.txt -p http://127.0.0.1:8080
```

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

And to see the results faster, we can filter using the "Status" or "Length"

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>
