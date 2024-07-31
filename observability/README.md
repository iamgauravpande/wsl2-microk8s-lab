### Observability

This will contain all the observability tools helm charts that will be deployed on microk8s cluster using Github Actions and ArgoCD

### Pre-requisites:

- Requirement of Github Self hosted Runner: Login to your github accound and follow [Adding self hosted runner](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/adding-self-hosted-runners?ref=goatreview.com)

### Observability Tenants:

#### Grafana: 

- Under `observability/tenants` directory run below command to Various Observability Tools helm repo and pull it. 

```
/snap/bin/microk8s helm repo add grafana https://grafana.github.io/helm-charts
/snap/bin/microk8s helm pull grafana/grafana
tar -zxvf grafana-8.3.6.tgz
rm -rf grafana-8.3.6.tgz
```
- To get initial password retrieve it using below command: 

*NOTE: jq package should be installed on your machine*

`kubectl get secrets/<secret_resource_name> -n <namespace> -o json | jq '.data | map_values(@base64d)'`