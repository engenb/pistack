just a test grafana deploy w/ persistence to check stability w/o full prometheus-operator upgrade
helm upgrade --install grafana -f values.yaml stable/grafana