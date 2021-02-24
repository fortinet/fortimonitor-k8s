# Panopta Kubernetes Integration

## Prerequisites
* A Kubernetes cluster configured with kubectl
* Helm installed locally

## Resource Requirements
This chart comes with three default deployment sizes: `small`, `medium`, and `large`  
You can specify any upon installation using for example `--set size=large`  
You can also override any individual request using `--set onsightRequests.<resource>=<value>` where `<resource>` is `cpu`, `memory`, or `ephemeral`

| Size               | Suggested # of Pods   | Requested OnSight CPUs | Requested OnSight Memory | Requested OnSight Ephemeral Storage |
|--------------------|-----------------------|------------------------|--------------------------|-------------------------------------|
| small              | < 500                 | 1.0                    | 1Gi                      | 10Gi                                |
| medium             | 500-2000              | 2.0                    | 2Gi                      | 20Gi                                |
| large              | >2000                 | 3.0                    | 3Gi                      | 50Gi                                |

## Deploying Panopta
**Note:** See above for determining `size`. The default is `medium`.
**Note:** If your cluster already has `metrics-server` installed, you can disable its installation (described below).
1. Add this Helm repo using `helm repo add panopta https://panopta.github.io/kubernetes/repo`
2. Install Panopta using `helm install --set customer_key=<your-customer-key> --set size=<size> <name-of-release> panopta/panopta`

In a few minutes, your cluster should show up in the Panopta control panel.

### Advanced configuration
If you wish to further customize your Panopta deployment, you can pass additional options to the install command by adding one to many  
`--set <key>=<value>`  
to the install command.  
Below is a table of available configuration options.  
You can also specify such options in a YAML-formatted `values.yaml` file which you can then pass along to the install command with `-f values.yaml`

### Configuration Options

| Key Name                  | Default                                    | Description                                                                                                              |
|---------------------------|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| customer_key              | None (Required)                            | Your Panopta customer key                                                                                                |
| size                      | medium                                     | Size of the cluster you are deploying to                                                                                 |
| clusterName               | Kubernetes Cluster                         | The name of this cluster as it will show up in the controlpanel                                                          |
| metricsServer.install     | true                                       | Whether to install metrics-server as part of the deployment. Set to `false` if it's already installed.                   |
| topNNamespaces            | 0                                          | Number of namespaces to pull in, ordered by number of pods. 0 to include all.                                            |
| onsightRequests.cpu       | None                                       | Requested CPU for the Panopta OnSight                                                                                    |
| onsightRequests.memory    | None                                       | Requested Memory for the Panopta OnSight                                                                                 |
| onsightRequests.ephemeral | None                                       | Requested Ephemeral Storage for the Panopta OnSight                                                                      |
| agent_config              | None                                       | Any additional blocks of configuration to deploy onto the nodes' agents                                                  |

## Upgrading Panopta
1. Fetch new charts using `helm repo update`
2. Upgrade your deployment using `helm upgrade <deployment name> panopta/panopta`

## Uninstalling Panopta
Run `helm uninstall <release_name>`

You can find the name of the release with `helm ls`
