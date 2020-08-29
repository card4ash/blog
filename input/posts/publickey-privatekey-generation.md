Title: Crete public key private key OpenSSL
Published: 12/08/2019
Tags:
    - OpenSSL
    - publickey
    - privatekey
    - pkcs1
    - pkcs8
    - der
    - pem
    - intermediate
---

Create public key private key OpenSSL
===========================================
Generate Private key(pkcs1) and certificate
```shell
    openssl genrsa -out file.key 2048
    openssl req -new -x509 -key file.key -out publickey.cer -days 1825
```

```shell

    openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 -nodes  -keyout example.key -out example.crt -subj /CN=example.com  -addext subjectAltName=DNS:example.com,DNS:example.net,IP:10.0.0.1

    openssl pkcs8 -topk8 -inform pem -in file.key -outform pem -nocrypt -out file.pem

    openssl genrsa -out file.key 1024

```

Alternating Way (Create Private key and Certificate)

```shell
    openssl genrsa -out privkey.pem 4096
    openssl req -new -x509 -key privkey.pem -out cacert.pem -days 1095 -subj /CN=example.com  -addext subjectAltName=DNS:example.com,DNS:example.net,IP:10.0.0.1
```


convertion between different formats
======================================

PKCS #1
*********
Defines the traditional format for RSA keys. Two structures:

```shell
    -----BEGIN RSA PRIVATE KEY-----
    RSAPrivateKey
    -----END RSA PRIVATE KEY-----
```

```shell
    -----BEGIN RSA PUBLIC KEY-----
    RSAPublicKey
    -----END RSA PUBLIC KEY-----
```

Commands
***********

Generate RSA private key
**************************

```shell
    openssl genrsa -out private.pem 2048
```

Extract public key from RSA private key
******************************************

```shell
    openssl rsa -in private.pem -out public.pem -RSAPublicKey_out
```

ASN.1 and DER encoding
*************************
Within the RSA, PKCS#1 and SSL/TLS communities the Distinguished Encoding Rules (DER) encoding of ASN.1 is used to represent keys, certificates and such in a portable format. Although ASN.1 is not the easiest to understand representation formats and brings a lot of complexity, it does have its merits. The certificate or key information is stored in the binary DER for ASN.1 and applications providing RSA, SSL and TLS should handle DER encoding to read in the information.

PEM files
************
Because DER encoding results in a truly binary representation of the encoded data, a format has been devised for being able to send these in an encoding of printable characters so you can actually mail these things. The format I focus on now is the PEM format.

In essence PEM files are just base64 encoded versions of the DER encoded data. In order to distinguish from the outside what kind of data is inside the DER encoded string, a header and footer are present around the data. An example of a PEM encoded file is:

```shell
    -----BEGIN PUBLIC KEY-----
    MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDMYfnvWtC8Id5bPKae5yXSxQTt
    +Zpul6AnnZWfI2TtIarvjHBFUtXRo96y7hoL4VWOPKGCsRqMFDkrbeUjRrx8iL91
    4/srnyf6sh9c8Zk04xEOpK1ypvBz+Ks4uZObtjnnitf0NBGdjMKxveTq+VE7BWUI
    yQjtQ8mbDOsiLLvh7wIDAQAB
    -----END PUBLIC KEY-----
```

The first and last line indicate the DER format that should be expected inside. The data inside is a base64 encoded version of the DER encoded information.


Good explanations of the difference between the two formats pkcs1 and pkcs8 [https://tls.mbed.org/kb/cryptography/asn1-key-structures-in-der-and-pem](https://tls.mbed.org/kb/cryptography/asn1-key-structures-in-der-and-pem)

Also Take A look [https://stackoverflow.com/questions/18039401/how-can-i-transform-between-the-two-styles-of-public-key-format-one-begin-rsa/29707204](https://stackoverflow.com/questions/18039401/how-can-i-transform-between-the-two-styles-of-public-key-format-one-begin-rsa/29707204)


[Abstract Syntax Notation One](https://en.wikipedia.org/wiki/Abstract_Syntax_Notation_One)

[https://stackoverflow.com/questions/18039401/how-can-i-transform-between-the-two-styles-of-public-key-format-one-begin-rsa/29707204](https://stackoverflow.com/questions/18039401/how-can-i-transform-between-the-two-styles-of-public-key-format-one-begin-rsa/29707204)