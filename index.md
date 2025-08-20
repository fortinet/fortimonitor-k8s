---
title: ""
---

## Index

- [Requirements](#requirements)
- [Quickstart](#quickstart)
- [Configuration](#configuration)
- [Upgrading FortiMonitor](#upgrading-fortimonitor)
- [Uninstalling FortiMonitor](#uninstalling-fortimonitor)
- [Provider-specific Instructions](#provider-instructions)

## Requirements

### Prerequisites

- Certified Kubernetes cluster running 1.32+
- Helm/kubectl access to target cluster
- Cluster is configured for [Dynamic Volume Provisioning](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/).
  - A default `StorageClass` is also required unless you wish to manually specify one (see below)

### Resource Presets

This chart comes with three presets for setting resource requests depending on cluster size: `small`, `medium`, and `large`.
The default is `medium`.

You can specify a preset at installation using for example `--set size=large`

You can also override any individual resource requests via values. See [Configuration Options](#configuration-options) below.

| Size               | Suggested # of Pods   | Requested OnSight CPUs | Requested OnSight Memory | Requested OnSight PV Size |
|--------------------|-----------------------|------------------------|--------------------------|---------------------------|
| small              | < 500                 | 1.0                    | 1Gi                      | 50Gi                      |
| medium             | 500-2000              | 2.0                    | 2Gi                      | 50Gi                      |
| large              | >2000                 | 3.0                    | 3Gi                      | 50Gi                      |

## Quickstart

Before proceeding, check the [Provider-specific Instructions](#provider-instructions) section for any specifics required
for certain cluster providers.

1. Add this Helm repo using `helm repo add fortimonitor https://fortimonitor.github.io/kubernetes/repo`
2. Switch to the desired Kubernetes namespace for the installation
3. Install FortiMonitor using `helm install -g --set customerKey=<your-customer-key> --set size=<size> fortimonitor/fortimonitor`
  a. You can specify a name for this cluster by also adding `--set clusterName=<name>` to the above command or your values.yaml

After a successful install, an OnSight pod should be created within the namespace.
It will automatically configure the integration and register it to your FortiMonitor account.
In a few minutes, your cluster should show up in the FortiMonitor control panel.

## Configuration

Configuration of the FortiMonitor integration can be done via the usual Helm values mechanisms.
The preferred method is by creating a `values.yaml` file which can be passed to `helm install`/`helm update` with
the `-f values.yaml` flag.

An example values.yaml file using the above Quickstart instructions would look like:

```yaml
size: medium
customerKey: your-customer-key-123
clusterName: "My Cluster"
```

### Configuration Options

The following options are available to be set in `values.yaml` or via `--set`:

| Key Name                  | Default            | Type            | Description                                                                                  |
|---------------------------|--------------------|-----------------|----------------------------------------------------------------------------------------------|
| customerKey               | None (Required)    | String          | Your FortiMonitor customer key                                                               |
| size                      | medium             | String          | Size of the cluster you are deploying to                                                     |
| clusterName               | Kubernetes Cluster | String          | The name of this cluster as it will show up in the controlpanel                              |
| topNNamespaces            | 0                  | Int             | Number of namespaces to pull in, ordered by number of pods. 0 to include all.                |
| filterNamespaces.include  | None               | List of Strings | If set, the integration will only collect namespaced objects from the specified namespaces.  |
| filterNamespaces.exclude  | None               | List of Strings | If set, the integration will ignore objects from the specified namespaces.                   |
| onsightRequests.cpu       | None               |                 | Requested CPU for the FortiMonitor OnSight                                                   |
| onsightRequests.memory    | None               |                 | Requested Memory for the FortiMonitor OnSight                                                |

## Upgrading FortiMonitor

1. Fetch new charts using `helm repo update`
2. Upgrade your deployment using `helm upgrade -f values.yaml <deployment_name> fortimonitor/fortimonitor`
  a. You can find the deployment name by using `helm ls` within the namespace you originally installed the integration.

## Uninstalling FortiMonitor

Run `helm uninstall <deployment_name>`

## Provider-specific Instructions {#provider-instructions}

### AWS Elastic Kubernetes Service (EKS)

- If your cluster is not an EKS Auto Mode cluster, you will need to install/configure an add-on
  such as the [EBS CSI driver](https://docs.aws.amazon.com/eks/latest/userguide/ebs-csi.html) or
  the [EFS CSI driver](https://docs.aws.amazon.com/eks/latest/userguide/efs-csi.html)
- For non-Auto Mode clusters, you may also need to create or specify a default `StorageClass` unless you wish
  to specify one (see [Configuration Options](#configuration-options)). For more information, see
  [Create a StorageClass](https://docs.aws.amazon.com/eks/latest/userguide/create-storage-class.html)

