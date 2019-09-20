# Shabad OS Manifests

This repo contains all the manifest files used by [Flux](https://fluxcd.io/) to manage the state of the Shabad OS Kubernetes cluster.

Once Flux is correctly initialised, the cluster will synchronise itself into the state defined by the YAML manifest files.

Flux will also update the images and Helm charts to the latest versions and push the changes back to the repository.

## Initialise Flux

Ensure Helm is [correctly installed](https://docs.fluxcd.io/en/stable/tutorials/get-started-helm.html#prerequisites).

To initialise or upgrade Flux in the cluster:

```
helm repo add fluxcd https://charts.fluxcd.io

helm upgrade -i flux \
--set helmOperator.create=true \
--set helmOperator.createCRD=false \
--set git.url=git@github.com:ShabadOS/manifests \
--set git.user="Shabad OS Bot" \
--set git.email="team@shabados.com" \
--namespace flux \
fluxcd/flux
```

## Helm Releases

- Traefik

## Workloads

- Shabad OS Website
- Shabad OS Database Viewer
- GurbaniNow Dev API
- Shabad OS Database MariaDB