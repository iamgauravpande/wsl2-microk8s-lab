# wsl2-microk8s-lab
This repo contains Kubernetes/Cloud native tools that will be deployed on microk8s cluster running on WSL2

### Infra needs:

- WSL2: wsl2 lets a windows user use a virtual linux environment on Windows Machine itself. This is pretty handy feature for quick development needs on your local machine. See [Install WSL2](https://learn.microsoft.com/en-us/windows/wsl/install) to get started.

- Microk8s: Microk8s is a low-ops, lightweight upstream kubernetes solution. See [Install Microk8s](https://microk8s.io/docs/getting-started) to get started.

- Github Self Hosted Repo Runner: Follow below steps to quikly Install and Configure the Runner on your wsl2 environment.

*NOTE: GITHUB_PERSONAL_ACCESS_TOKEN below should have repo scope*

```
## Create a folder:
mkdir actions-runner && cd actions-runner || exit

## Download the latest runner package:
curl -o actions-runner-linux-x64-2.320.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.320.0/actions-runner-linux-x64-2.320.0.tar.gz

## Extract the installer:
tar xzf ./actions-runner-linux-x64-2.320.0.tar.gz

## get the registration token:
TOKEN=$(curl -s  -L -X POST -H "Accept: application/vnd.github+json" -H "Authorization: Bearer <GITHUB_PERSONAL_ACCESS_TOKEN>" -H "X-GitHub-Api-Version: 2022-11-28" https://api.github.com/repos/{OWNER}/{REPO_NAME}/actions/runners/registration-token | jq '.token')

## configure the runner: 
./config.sh --url https://github.com/{OWNER}/{REPO_NAME} --token $TOKEN --name "gp_runner_01" --no-default-labels --labels "gp_runner_01" --unattended

## Start Self Hosted Runner as a Service:
sudo ./svc.sh install <username>
sudo ./svc.sh start <username>
```

### LoadBalancer needs:
- MetalLB: A network LB implementation that tries to “just work” on bare metal clusters. See [Enable MetalLB Addon](https://microk8s.io/docs/addon-metallb)

*NOTE: While enabling the addon make sure to provide the subnet/range of IP that falls within your local network, See below example*

```
microk8s enable metallb:172.30.208.2-172.30.208.5
```

### Declartive Delivery tool:
- ArgoCD: We will use ArgoCD to deploy other tools/applications on our Microk8s Cluster see [Delivery Tool](https://github.com/iamgauravpande/wsl2-microk8s-lab/tree/main/delivery-tool#readme) for details.


