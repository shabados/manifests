# Shabad OS Manifests

This repo contains all the manifest files used by [Flux](https://fluxcd.io/) to manage the state of the Shabad OS Kubernetes cluster.

## Initialise Flux

Ensure Helm is [correctly installed](https://docs.fluxcd.io/en/stable/tutorials/get-started-helm.html#prerequisites).

To initialise or upgrade Flux in the cluster:

```
helm repo add fluxcd https://charts.fluxcd.io

helm upgrade -i flux \
--set helmOperator.create=true \
--set helmOperator.createCRD=false \
--set git.url=git@github.com:ShabadOS/manifests \
--namespace flux \
fluxcd/flux
```

## Workloads

- Shabad OS Website
- Shabad OS Database Viewer
- GurbaniNow Dev API
- Shabad OS Database MariaDB