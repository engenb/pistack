helm secrets upgrade --install prometheus-operator --namespace monitoring -f values.yaml -f secrets.yaml stable/prometheus-operator
^ didn't work with helm/tiller 2.12.0.  had to upgrade to 2.12.1