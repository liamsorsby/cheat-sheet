# Generating a JKS (Java Key Store) file

### Prerequisites
- Private Key named private.key
- CA Chain named ca-chain0.crt and ca-chain1.crt (if you have a secondary part)
- Certificate named certificate.crt
- Docker

### Steps

When you've got your ca chain (either a single part or in multiple parts), private key and certificate run the following commands

Set your desired keystore name and password
```bash
KEYSTORE_FILE="keystore.jks"
KEYSTORE_PASSWORD="changeme"
```

Note: If you only have a single chain then you'll need to modify the command below
```bash
openssl pkcs12 -export -in certificate.crt -inkey private.key -name "client" -out bundle.p12 -password "pass:$KEYSTORE_PASSWORD"
docker run -v $(pwd):/root -w /root -it openjdk:11 keytool -importkeystore -deststorepass "$KEYSTORE_PASSWORD" -destkeystore "$KEYSTORE_FILE" -srckeystore bundle.p12 -srcstoretype PKCS12 -srcstorepass "$KEYSTORE_PASSWORD" &&
docker run -v $(pwd):/root -w /root -it openjdk:11 keytool -import -alias kafka_ca_1 -file ca-chain1.crt -keystore "$KEYSTORE_FILE" -storepass "$KEYSTORE_PASSWORD" -noprompt &&
docker run -v $(pwd):/root -w /root -it openjdk:11 keytool -import -alias kafka_ca_2 -file ca-chain0.crt -keystore "$KEYSTORE_FILE" -storepass "$KEYSTORE_PASSWORD" -noprompt
rm bundle.p12 ca-chain0.crt ca-chain1.crt certificate.crt private.key
```

This will leave you with a single jks file and remove the ca chains, private key & cert

### Navigation
[Back Home](../README.md)
