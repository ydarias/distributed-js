## Self-signed certificate generation

```shell
mkdir -p ./{recipe-api,shared}/tls

openssl req -nodes -new -x509 -keyout recipe-api/tls/basic-private-key.key -out shared/tls/basic-certificate.cert
```

## Creating a general CA

```shell
# only to generate the CA
openssl genrsa -des3 -out ca-private-key.key 2048
openssl req -x509 -new -nodes -key ca-private-key.key -sha256 -days 365 -out shared/tls/ca-certificate.cert

# required for each new service
openssl genrsa -out recipe-api/tls/producer-private-key.key 2048
openssl req -new -key recipe-api/tls/producer-private-key.key -out recipe-api/tls/producer.csr
openssl x509 -req -in recipe-api/tls/producer.csr -CA shared/tls/ca-certificate.cert -CAkey ca-private-key.key -CAcreateserial -out shared/tls/producer-certificate.cert -days 365 -sha256
```