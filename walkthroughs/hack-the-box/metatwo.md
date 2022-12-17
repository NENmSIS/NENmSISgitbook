# MetaTwo

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Following the [Wordpress section of hacktrics](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/wordpress) i found this page:

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

And obtain this response, showing that is vulnerable to some attacks when we have the credentials

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

There is also another direction: `/wp-admin/admin-ajax.php` that seems vulnerable due to the usage of the plugin `bookingpress`.

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Lets test this exploit:&#x20;

{% embed url="https://github.com/destr4ct/CVE-2022-0739" %}

and obtain these credentials in Wordpress MD5 hash format

```
|admin|admin@metapress.htb|$P$BGrGrgf2wToBS79i07Rk9sN4Fzk.TV.|
|manager|manager@metapress.htb|$P$B4aNM28N0E.tMy/JIcnVMZbGcU16Q70|
```

Crack them with john and obtain the manager's credentials.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
