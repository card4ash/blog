Title: Mqtt Publish Subscribe Example
Published: 09/09/2020
Tags:
    - mqtt
    - mosquitto_pub 
    - mosquitto_sub
    - introduction
---

Publishing Using mosquitto_pub
==================================

```
    mosquitto_pub -h 127.0.0.1 -m "test message" -t wasa/data
```

Useful Flag Options and Examples
=====================================

-r  Sets retain flag
-n  Sends Null message useful for clearing retain message.
-p – Set Port number Default is 1883
-u – Provide a username
-P – Provide a password
-i – Provide client name
-I – Provide a client id prefix- Used when testing client restrictions using prefix security.

```
    mosquitto_pub -h 127.0.0.1 -m "test message" -t wasa/data -u ash -P secred -d
```

with client id

```
    mosquitto_pub -h 127.0.0.1 -m "test message" -t wasa/data -u ash -P secred -d -i testclientid 
```

Publishing JSON Data
JSON Data has a special format described here. When is comes to publishing with the mosquitto_pub client you need to escape the quotes so that they are included.

So don’t use {“status”:”off”} but instead use: {\”status\”:\”off\”}.

```
    mosquitto_pub -h 127.0.0.1 -m {\"status\":\"off\"} -t wasa/data -u ash -P secred -d -i testclientid 
```

```
    mosquitto_pub -h localhost -t test -m "{\"value1\":20,\"value2\":40}"
```

The Mosquitto_sub Client
============================

```
    mosquitto_sub -h localhost -t wasa/# -u ash -P secred
```

Links
==========
1. [http://www.steves-internet-guide.com/mosquitto_pub-sub-clients/](http://www.steves-internet-guide.com/mosquitto_pub-sub-clients/)