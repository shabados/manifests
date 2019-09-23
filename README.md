# Shabad OS Manifests

This repo contains all the manifest files used by [Flux](https://fluxcd.io/) to manage the state of the Shabad OS Kubernetes cluster.

Once Flux is correctly initialised, the cluster will synchronise itself into the state defined by the YAML manifest files.

Flux will also update the images and Helm charts to the latest versions and push the changes back to the repository.

## Cluster Setup

Install Helm, install Flux with Helm, job done 😊.

### Install Helm

[Install and configure Helm](https://docs.fluxcd.io/en/stable/tutorials/get-started-helm.html#prerequisites) by creating a tiller service account, and deploying Helm using the [Helm CLI](https://helm.sh/docs/using_helm/#installing-helm).


Set up the tiller service account:
```
# Create service account for tiller
kubectl -n kube-system create sa tiller

# Create admin rolebinding for tiller service account
kubectl create clusterrolebinding tiller-cluster-rule \
    --clusterrole=cluster-admin \
    --serviceaccount=kube-system:tiller
```

Deploy Tiller:
```
helm init --skip-refresh --upgrade --service-account tiller
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

Then run:

```
# Create the secret as normal, but dry-run
kubectl create secret generic mysecret --dry-run --from-file=foo=input-file -o json | kubeseal > secret.json
```

## Helm Releases

- Traefik
- Bitnami Sealed Secrets

## Workloads

- Shabad OS Website
- Shabad OS Database Viewer
- GurbaniNow Dev API
- Shabad OS Database MariaDB