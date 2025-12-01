# HighRateAnalysis-WP5
This repo contains a collection of Helm Charts developed and used in the context of WP5 of ICSC Spoke 2 project. 
General documentation is hosted at https://icsc-spoke2-repo.github.io/HighRateAnalysis-WP5

## Deployment of High Rate Analysis Platform

### Quick start
To deploy a Kubernetes cluster using INFN Cloud resources, please refer to the official guide (you need admin permissions): https://guides.cloud.infn.it/docs/users-guides/en/latest/users_guides/sysadmin/compute/k8s.html

First
```bash
git clone git@github.com:ICSC-Spoke2-repo/HighRateAnalysis-WP5.git
cd HighRateAnalysis-WP5
```
then:
- Cert-Manager:
  ```bash
  kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.2/cert-manager.yaml
  kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.2/cert-manager.crds.yaml
  ```
- Ingress-Controller
  ```bash
  helm install ingress-nginx oci://ghcr.io/nginxinc/charts/nginx-ingress --create-namespace --version 1.0.2 -n ingress-nginx
  ```
- Local-path:
  ```bash
  kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/v0.0.24/deploy/local-path-storage.yaml
  ```
- Dask operator:
  ```bash
  helm install --repo https://helm.dask.org --create-namespace -n dask-operator --generate-name dask-kubernetes-operator
  ```
- cvmfs:
  ```bash
  kubectl apply -f stable/cvmfs/default-local.yaml
  kubectl apply -f stable/cvmfs/daemonset-cvmfs.yaml
  ```
- JHub: Edit the `jhub_config.py` provided in [values.yaml](stable/jhub-aas/values.yaml) in order to customize your JupyterHUB configuration. Then:
  ```bash
  git clone git@github.com:ICSC-Spoke2-repo/HighRateAnalysis-WP5.git
  cd stable/jhub-aas
  helm repo add jupyterhub https://jupyterhub.github.io/helm-chart/
  helm dependency build
  kubectl create namespace jhub
  helm upgrade --install --cleanup-on-fail --namespace jhub jhub ./
  cd ../../ 
  ```

### Enable offloading

Patch dask cluster custom resource:
```bash
kubectl patch crd daskclusters.kubernetes.dask.org \
  --type=json \
  -p='[
    {
      "op": "replace",
      "path": "/spec/versions/0/schema/openAPIV3Schema/properties/spec/properties/scheduler/properties/service/properties/ports/items/properties/targetPort/type",
      "value": "integer"
    }
  ]'
```

Modify [ssh-fwd.yaml](stable/ssh-fwd/ssh-fwd.yaml) inserting correct IP address and install ssh-forwarder machinery:
```bash
kubectl apply -f stable/ssh-fwd/ssh-fwd.yaml -n jhub
kubectl apply -f stable/ssh-fwd/ssh-fwd-svc.yaml -n jhub
kubectl apply -f stable/ssh-fwd/listener-svc.yaml -n jhub
```

Insert kubeconfig in `stable/vk/kustomization/kubeconfig`
```bash
kubectl apply -k stable/jhub-aas/vk/kustomization
```

Insert the newly-created InterLink configmap name in [vk-na.yaml](stable/vk/vk-na.yaml) and deploy the Virtual Kubelet pod (VK container + InterLink container + InterLink HTCondor Plugin container):
```bash
kubectl apply -f stable/vk/vk-na.yaml -n vk
kubectl apply -f stable/vk/sa-interlink.yaml -n vk
```
