---
title: Kubernetes
description: 
published: true
date: 2024-08-23T20:04:47.929Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:34:40.922Z
---

# Kubernetes

You can also deploy Foundry VTT on your K8S/K3S cluster using different approachs.

## Kustomize 
by [mbround18](https://github.com/mbround18/)

Requirements:

* Kubernetes Cluster (EKS/GKE/AKS/K3S/K8S)

Follow the detailed step on the his [repo](https://github.com/mbround18/foundryvtt-docker/blob/main/docs/kube.md).


## Helm Charts
by [Hugo Prudente](https://github.com/hugoprudente/)

Requirements:

* Helm 
* Kubernetes Cluster (EKS/GKE/AKS/K3S/K8S)
* Container built with [Felddy's Easy Container](https://github.com/felddy/foundryvtt-docker.git)

You can install dirctly from the Helm repo
```shell
helm repo add foundry-vtt https://hugoprudente.github.io/helm-charts/incubator/
helm install my-foundry-vtt foundry-vtt/foundry-vtt --version 0.2.14
```

Or clone the repository and do some tweeks.

```shell
git clone https://github.com/hugoprudente/charts/
```

Edit the file `values.yaml` with your desired configuration.

```shell
helm install --name foundry ./incubator/foundry-vtt
```
or
```shell
helm3 install foundry ./incubator/foundry-vtt
```

For support ping @hugoprudente on twitter.

