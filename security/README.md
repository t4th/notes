# security
## mbedtls

### md5
### crc32
### sha256

## openssl
### create private key
```
openssl genpkey -out privkey.pem -algorithm rsa 2048
```
### create public key from private key
```
openssl rsa -in privkey.pem -outform PEM -pubout -out pubkey.pem
```
### create signature
create digital digest signature for file.c file:
```
    openssl dgst -sha256 -sign privkey.pem -out sign.sha256 file.c
```

create base64 version of signature:
```
    openssl enc -base64 -in sign.sha256 -out sign.sha256.base64
```