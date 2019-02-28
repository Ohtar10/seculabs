# Root Intermeduate

## Assumptions
* For all certificates, stores, and keys, the password is 'password'
* This tutorial will use the rootCA in this repository

## How to generate them

### Prepare the directory
```
cd <path/to/rootIntermediate>
mkdir certs crl csr newcerts private
chmod 700 private
touch index.txt
echo 1000 > serial
```

### Create the intermediate key
```
openssl genrsa -aes256 -out private/intermediate.key.pem 4096
chmod 400 private/intermediate.key.pem 
```

**Notes:**
Notice so it's the same thing as the rootCA, look for that readme
for more details on the command arguments.

### Create the intermediate certificate
```
openssl req -config openssl.cnf -new -sha256 -key private/intermediate.key.pem -out csr/intermediate.csr.pem
```
**Notes:**
* Notice this is the same command as the rootCA except here we are requesting a sign, not the actual certificate.
* At this point you most provide a 'Common Name' for this certificate as it will be needed in next stages

### Sign the the csr with the rootCA using openssl.cnf from the rootCA
```
openssl ca -config ../rootCA/openssl.cnf -extensions v3_intermediate_ca -days 3650 -notext -md sha256 -in csr/intermediate.csr.pem -out certs/intermediate.cert.pem
chmod 444 certs/intermediate.cert.pem
```
**Notes:**
* Since we are using the openssl.cnf from rootCA, the 'dir' variable should be an absolute path to rootCA to avoid failure signing the intermediate certificate.
* Also, notice the duration of this certificate is less than the root one.
* At this point, the preparation steps are mandatory so when signing this certificate the command is able to find the resource files.
* Pay attention to any error messages as they will tell you what is missing/mismatching

### Verify this certificate
```
openssl x509 -noout -text -in certs/intermediate.cert.pem
```

### Verify this certificate against the root ca to ensure chain of trust
```
openssl verify -CAfile ../rootCA/certs/ca.cert.pem certs/intermediate.cert.pem
```
This should output ```certs/intermediate.cert.pem: OK```

### Create the certificate chain
This is just a concatenation of the intermediate and root certificates
```
cat certs/intermediate.cert.pem ../rootCA/certs/ca.certs.pem > certs/ca-chain.cert.pem
chmod 444
```

## References
[OpenSSL Certificate Authority - Create intermediate pair](https://jamielinux.com/docs/openssl-certificate-authority/create-the-intermediate-pair.html)
