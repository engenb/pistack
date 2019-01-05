helm secrets upgrade --install prometheus-operator --namespace monitoring -f values.yaml -f secrets.yaml stable/prometheus-operator
^ didn't work with helm/tiller 2.12.0.  had to upgrade to 2.12.1

# etcd notes
after fetching etcd ca.crt, ca.key:

create openssl.cnf
```
[ req ]
req_extensions = v3_req
distinguished_name = req_distinguished_name
[ req_distinguished_name ]
[ v3_req ]
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, keyEncipherment, digitalSignature
extendedKeyUsage=serverAuth, clientAuth
```

generate etcd-client.key
```
openssl genrsa -out etcd-client.key 2048
```

generate etcd-client.csr
```
openssl req -new -key etcd-client.key -out etcd-client.csr -subj "/CN=etcd" -config openssl.cnf
```

generate etcd-client.crt
```
openssl x509 -req -in etcd-client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out etcd-client.crt -days 365 -extensions v3_req -extfile openssl.cnf
```

```
kubectl -n monitoring create secret generic etcd-client-certs --from-file=./etcd-client.crt --from-file=./ca.crt --from-file=./etcd-client.key
```