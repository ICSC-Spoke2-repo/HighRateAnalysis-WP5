# JHUBaaS HELM CHART

This Helm chart is a customization of the one of ["Zero to Jupyter on k8s"](https://jupyterhub.github.io/helm-chart/)

## Quick start

### Requirements

- Kubernetes cluster
- Cert-Manager
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
git clone
cd charts/apps/jupyter-hub

kubectl create namespace jhub

helm upgrade --install --wait --cleanup-on-fail --namespace jhub \
                    jhub \
                    ./ 
```
