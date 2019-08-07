# Panopta Kubernetes Integration

## Prerequisites
* A Kubernetes cluster configured with kubectl
* Helm's Tiller is deployed on the cluster and has correct RBAC permissions. If you're unsure what this entails, see *Setting up Tiller* below.
* `kube-state-metrics` is deployed on the cluster in the `kube-system` namespace with its service named `kube-state-metrics` (the default Helm chart for kube-state-metrics deploys the service with a generated name)
* metrics-server is deployed on the cluster

If your provider does not automatically provision kube-state-metrics and metrics-server, you can deploy them using the included charts in this repo. Instructions to do so are below. 

## Deploying Panopta
**Note:** Make sure you have satisfied the above requirements first.
1. Add this Helm repo using `helm repo add panopta https://panopta.github.io/kubernetes/repo`
2. Install Panopta using `helm install --set customer_key=YOUR-CUSTOMER-KEY panopta/panopta`

In a few minutes, your cluster should show up in the Panopta control panel.

### Advanced configuration
If you wish to further customize your Panopta deployment, you can edit a values.yaml file.
An example values.yaml file with all available options can be found [here](https://github.com/Panopta/kubernetes/blob/master/panopta/values.yaml)
Then deploy Panopta using:
`helm install -f values.yaml panopta/panopta`

## Upgrading Panopta
1. Fetch new charts using `helm repo update`
2. Upgrade your deployment using `helm upgrade <deployment name> panopta/panopta`

## Setting up Tiller
1. Fetch `tiller-rbac.yaml` from [here](https://github.com/Panopta/kubernetes/blob/master/tiller-rbac.yaml)
2. Deploy the correct RBAC credentials for Tiller using `kubectl create -f tiller-rbac.yaml`
2. Setup Tiller using `helm init --history-max=200`

## Setting up kube-state-metrics
1. If you have not already added the Panopta helm repo, see step `1.` under *Deploying Panopta*
2. `helm install --namespace "kube-system" panopta/kube-state-metrics`

## Setting up metrics-server
1. If you have not already added the Panopta helm repo, see step `1.` under *Deploying Panopta*
2. `helm install --namespace "kube-system" panopta/metrics-server`
