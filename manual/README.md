# FortiMonitor Kubernetes Integration

## Manual Installation
1. Create a `fortimonitor` namespace and switch into it
2. Verify whether `metrics-server` is already configured in your cluster by running `kubectl get svc -A | grep metrics-server`
3. If `metrics-server` is NOT installed, install it by running `kubectl apply -f metrics-server/` from this directory
4. Install `fortimonitor-kube-state-metrics` by running `kubectl apply -f kube-state-metrics/` in this directory
    - **Note**: The FortiMonitor integration requires its own lightweight copy of `kube-state-metrics`.
      If your cluster already has `kube-state-metrics` installed, you can try updating `KUBE_STATE_ENDPOINT` in `fortimonitor/configmap.yaml`
      to match its correct Service endpoint and recreate the fortimonitor-onsight pod. However this method is not supported.
5. Edit `fortimonitor/configmap.yaml` and insert your customer key where it says `YOUR-CUSTOMER-KEY`.
   You can also set a cluster name here if you wish.
6. Deploy the FortiMonitor integration with `kubectl apply -f fortimonitor/` from this directory

After a few minutes, you should see your cluster + OnSight appear under the `Incoming OnSights` group in the ControlPanel!
