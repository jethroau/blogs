

```
openssl genrsa -out "root-ca.key" 4096
  
openssl req \
-new -key "root-ca.key" \
-out "root-ca.csr" -sha256 \
-subj '/C=CN/ST=HK/L=HK/O=HKBN/CN=HKBN Docker Registry CA'




openssl x509 -req -days 3650 -in "root-ca.csr" \
-signkey "root-ca.key" -sha256 -out "root-ca.crt" \
-extfile "root-ca.cnf" -extensions root_ca


openssl genrsa -out "registry.hkbn.net.key" 4096


openssl req -new -key "registry.hkbn.net.key" -out "registry.csr" -sha256 \
-subj '/C=CN/ST=HK/L=HK/O=HKBN/CN=registry.hkbn.net'


openssl x509 -req -days 75000 -in "registry.csr" -sha256 \
-CA "root-ca.crt" -CAkey "root-ca.key" -CAcreateserial \
-out "registry.hkbn.net.crt" -extfile "registry.cnf" -extensions server






docker run --rm \
--entrypoint htpasswd \
registry \
-Bbn jethro jethro > auth/nginx.htpasswd
````
