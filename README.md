# Shabad OS Manifests

This repo contains all the manifest files used by [Flux](https://fluxcd.io/) to manage the state of the Shabad OS Kubernetes cluster.

Once Flux is correctly initialised, the cluster will synchronise itself into the state defined by the YAML manifest files.

Flux will also update the images and Helm charts to the latest versions and push the changes back to the repository.

## Cluster Setup

Install Helm, create a flux namespace, install Flux with Helm, job done 😊.

### Install Helm

[Install the Helm CLI](https://docs.fluxcd.io/en/stable/tutorials/get-started-helm.html#prerequisites).

### Create Flux namespace

Create a namespace dedicated to running flux:

```
kubectl create namespace flux
```

### Add the Flux Custom Resource Definition

Add the custom resource definition for Flux:
```
kubectl apply -f https://raw.githubusercontent.com/fluxcd/flux/helm-0.10.1/deploy-helm/flux-helm-release-crd.yaml
```

### Install Flux

Ensure Helm is [correctly installed](https://docs.fluxcd.io/en/stable/tutorials/get-started-helm.html#prerequisites).

To initialise or upgrade Flux in the cluster:

```
helm repo add fluxcd https://charts.fluxcd.io

helm upgrade -i flux \
--set helmOperator.create=true \
--set helmOperator.createCRD=true \
--set git.url=git@github.com:ShabadOS/manifests \
--set git.user="Shabad OS Bot" \
--set git.email="team@shabados.com" \
--namespace flux \
fluxcd/flux
```

## Storing secrets

Install the [kubeseal CLI](https://github.com/bitnami-labs/sealed-secrets/releases).

In the `secrets` directory run:

```
kubectl -n shabad-os create secret generic \
  secret-name \
  --dry-run \
  --from-file=input.json \
  -o json > secret.tmp.json

kubeseal --format=yaml -n shabad-os -- < secret.tmp.json > sealed-secret.yaml
```

## Helm Releases

- Traefik
- Bitnami Sealed Secrets

## Workloads

- Shabad OS Website
- Shabad OS Database Viewer
- GurbaniNow Dev API
- GurbaniNow Dev Website
- Shabad OS Database MariaDB
