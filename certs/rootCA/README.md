# Root CA

## Assumptions
For all certificates, stores, and keys, the password is 'password'

## How to generate them

### Prepare the directory
```
cd <path/to/rootCA>
mkdir certs crl newcerts private
chmod 700 private
touch index.txt
echo 1000 > serial
```

### Create the root key
```
openssl genrsa -aes256 -out private/ca.key.pem 4096
chmod 400 private/ca.key.pem
```
**Notes:**
* The output directory must exist before executing this command.
* -aes256 is the cryptographic algorithm, you can choose the want you need.
* 4096 is the amount of bytes for the key, i.e., this will generate a 4096 bytes key.
* This command will prompt for a pass phrase, put whatever you need, in this case is 'password'
* You can always use --help to get details on what commands and options you have.

### Creathe the root certificate
```
openssl req -config openssl.cnf -key private/ca.key.pem -new -x509 -days 7300 -sha256 -extensions v3_ca -out certs/ca.cert.pem
```
**Notes:**
* req for Certificate Requests
* -config openssl.cnf is the configuration to use, this file is located in the same directoy, take a look for details and changes
* -key, the already created private key
* -new -x509, a new request for a X509 certificate type
* -days 7300, expiration for this certificate, e.g., 20 years.
* -sha256, Hashing algorithm to use
* -extensions v3_ca, An specific extension to use for the CA
* certs/ca.cert.pem is the output file of the certificate.

### Verify the root certificate
```
openssl x509 -noout -text -in certs/ca.cert.pem
```


## References
[OpenSSL Certificate Authority - Create root pair](https://jamielinux.com/docs/openssl-certificate-authority/introduction.html)