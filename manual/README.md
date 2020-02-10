# Panopta Kubernetes Integration

## Manual Installation
1. Verify whether `kube-state-metrics` and `metrics-server` are already configured in your cluster by running `kubectl get svc -A` and looking for each.
2. If `metrics-server` is NOT installed, install it by running `kubectl apply -f metrics-server/` from this directory
3. If `kube-state-metrics` is NOT installed, install it by running `kubectl apply -f kube-state-metrics/` from this directory
4. Edit `panopta/configmap.yaml` and insert your customer key where it says `YOUR-CUSTOMER-KEY`
5. Deploy the Panopta integration with `kubectl apply -f panopta/` from this directory
