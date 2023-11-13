# JHUBaaS HELM CHART

This Helm chart is a customization of the one of ["Zero to Jupyter on k8s"](https://jupyterhub.github.io/helm-chart/)

## Quick start

### Requirements

- Kubernetes cluster
- Cert-Manager:
  ```
  kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.1.1/cert-manager.yaml
  kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.1.1/cert-manager.crds.yaml
  ```
- Ingress-Controller
  - everything pre-configured to run with NGINX ingress controller out-of-the-box
- Local-path:
  ```
  kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/v0.0.24/deploy/local-path-storage.yaml
  ```

### Customized values

Edit the `jhub_config.py` provided in [values.yaml](values.yaml) in order to customize your JupyterHUB configuration.

### Deploy the JupyterHUB

```bash
git clone git@github.com:ICSC-Spoke2-repo/HighRateAnalysis-WP5.git
cd HighRateAnalysis-WP5/stable/jhub-aas
helm repo add jupyterhub https://jupyterhub.github.io/helm-chart/
helm dependency build
kubectl create namespace jhub
helm upgrade --install --cleanup-on-fail --namespace jhub jhub ./ 
```
