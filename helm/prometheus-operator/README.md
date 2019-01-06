```
ssh pi@[master node]

mkdir certs
sudo cp /etc/kubernetes/pki/etcd/ca.crt ~/certs
sudo cp /etc/kubernetes/pki/etcd/healthcheck-client.crt ~/certs
sudo cp /etc/kubernetes/pki/etcd/healthcheck-client.key ~/certs
cd certs
sudo chmod 644 healthcheck-client.key
kubectl -n monitoring create secret generic etcd-healthcheck-client-certs --from-file=./ca.crt --from-file=./healthcheck-client.crt --from-file=./healthcheck-client.key
cd ~/
rm -rf certs
```

```
helm secrets upgrade --install prometheus-operator --namespace monitoring -f values.yaml -f secrets.yaml stable/prometheus-operator
```
^ didn't work with helm/tiller 2.12.0.  had to upgrade to 2.12.1
