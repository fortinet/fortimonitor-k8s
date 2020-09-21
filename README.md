# Panopta Kubernetes Integration

## Prerequisites
* A Kubernetes cluster configured with kubectl

## Deploying Panopta
**Note:** If your cluster provider already installed either `kube-state-metrics` or `metrics-server`, disable them in your values.yaml file first (described below)
1. Add this Helm repo using `helm repo add panopta https://panopta.github.io/kubernetes/repo`
2. Install Panopta using `helm install --set customer_key=YOUR-CUSTOMER-KEY panopta/panopta`

In a few minutes, your cluster should show up in the Panopta control panel.

### Advanced configuration
If you wish to further customize your Panopta deployment, you can edit a values.yaml file.
An example values.yaml file with all available options can be found [here](https://github.com/Panopta/kubernetes/blob/master/panopta/values.yaml)
Then deploy Panopta using:
`helm install -f values.yaml panopta/panopta`

### Configuration Parameters
| Key Name                  | Default                                    | Description                                                                                                                |
|---------------------------|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| customer_key              | <Required>                                 | Your Panopta customer key                                                                                                  |
| agent_config              | None                                       | Any additional blocks of configuration to deploy onto the nodes' agents                                                    |
| kubeStateMetrics.install  | true                                       | Whether to install kube-state-metrics as part of the deployment. Set to `false` if it's already installed.                 |
| kubeStateMetrics.endpoint | http://kube-state-metrics.default.svc:8080 | The endpoint where kube-state-metrics can be accessed. If it was installed outside of Panopta, you'll need to update this. |
| metricsServer.install     | true                                       | Whether to install metrics-server as part of the deployment. Set to `false` if it's already installed.                     |

## Upgrading Panopta
1. Fetch new charts using `helm repo update`
2. Upgrade your deployment using `helm upgrade <deployment name> panopta/panopta`

## Uninstalling Panopta
Run `helm uninstall <release_name>`

You can find the name of the release with `helm ls`
