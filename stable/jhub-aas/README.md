# JHUBaaS HELM CHART

This Helm chart is a customization of the one of ["Zero to Jupyter on k8s"](https://jupyterhub.github.io/helm-chart/)

## Quick start

### Requirements

- Kubernetes cluster
- Cert-Manager
- Ingress-Controller
  - everything pre-configured to run with NGINX ingress controller out-of-the-box
- Longhorn


### Minimal set of values

Edit the `jub_config.py` provided by this repo in order to customize your JupyterHUB configuration. Then you will need the following minimal set of values to be specified either on the default `values.yaml` file or putting it in an external file e.g. `/tmp/external_values.yaml`

```yaml
jupyterhub:
  ingress:
    enabled: true
    annotations:
      kubernetes.io/ingress.class: "nginx"
      # comment this to use your own certs
      cert-manager.io/issuer: "selfsigned-issuer"
    hosts:
      - jhub.<PUT PUBLIC HOST>
    tls:
      - hosts:
        - jhub.<PUT PUBLIC HOST>
        secretName: jhub-tls

  proxy:
    secretToken: <PUT HERE A RANDOM TOKEN openssl rand -hex 32>
  hub:
    cookieSecret: <PUT HERE A RANDOM TOKEN openssl rand -hex 32>
```

### Deploy DODAS JupyterHUB

```bash
git clone
cd charts/apps/jupyter-hub

kubectl create namespace jhub

helm upgrade --install --wait --cleanup-on-fail --namespace jhub \
                    jhub \
                    ./ \
                    --values /tmp/external_values.yaml
```

## Values

Below you will find the most common default settings. Please find the complete list [here]()

```yaml
jupyterhub:
  ingress:
    enabled: true
    annotations:
      kubernetes.io/ingress.class: "nginx"
      # comment this to use your own certs
      cert-manager.io/issuer: "selfsigned-issuer"
    hosts:
      - jhub.<PUT PUBLIC HOST>
    tls:
      - hosts:
        - jhub.<PUT PUBLIC HOST>
        secretName: jhub-tls
        # use this if you put your certs on ./certs dir
        #secretName: tls-secrets

  scheduling:
    userScheduler:
      enabled: false
    podPriority:
      enabled: false
    userPlaceholder:
      enabled: false
  cull:
    enabled: true
    timeout: 3600
    every: 600

  proxy:
    chp:
      networkPolicy:
        enabled: false
    secretToken: <PUT HERE A RANDOM TOKEN openssl rand -hex 32>

  singleuser:
    networkPolicy:
          enabled: false
    storage:
      dynamic:
        storageClass: longhorn

  hub:
    image:
      name: dodasts/jupyterhub
      tag: '1.3.0-infncloud'
    cookieSecret: <PUT HERE A RANDOM TOKEN openssl rand -hex 32>
    networkPolicy:
      enabled: false
    args:
    - jupyterhub
    - --config
    - /etc/jupyterhub/jupyterhub_config.py
    - --upgrade-db
    db:
      type: sqlite-pvc
      upgrade:
      pvc:
        accessModes:
          - ReadWriteOnce
        storage: 1Gi
        storageClassName: longhorn
    extraEnv:
      OAUTH_CALLBACK_URL: https://jhub.<PUBLIC IP>.myip.cloud.infn.it/hub/oauth_callback
      OAUTH_ENDPOINT: https://iam.cloud.infn.it/
      OAUTH_GROUPS: users
      ADMIN_OAUTH_GROUPS: admins
      WITH_GPU: "false"
    extraConfig:
      myConfig.py: |
        {{ .Files.Get "jub_config.py" | indent 6 }}
  
spark:
  enabled: false
```
