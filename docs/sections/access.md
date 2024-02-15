---
myst:
  substitutions:
    key1: "Accounts should be requested with the motivation: **spoke2 tests**.
    <br>
    Once approved, another request is needed, in order to be added to the `highrate` group. Please, use the '*Join a group*' button in the '*Group request tab*', adding the same motivation as before, when requested."
    key2: |
      ```{important}
      {{ key1 }}
      ```
    key3: |
      ```{figure} ../img/iam.png
      :width: 400px
      ```
    key4: |
      ```{figure} ../img/request.png
      ```
---

# Resource Access
This section will be dedicated to the instructions in order to access the facility.

The entrypoint is https://hub.192.135.24.49.myip.cloud.infn.it and it will redirect the user to the destination JupyterHub, deployed with [WP5 JHub Helm Chart](https://github.com/ttedeschi/HighRateAnalysis-WP5/tree/main/stable/jhub-aas).

Before accessing the hub itself, an authentication step is needed, relying on an IAM-DEMO account. 

## Account Requests
If the user doesn't already have an account, please use the same authentication page available at the following link: https://iam-demo.cloud.cnaf.infn.it/

|            |                       |
| ---------- | --------------------- |
| {{ key3 }} | {{ key2 }} {{ key4 }} |

A short video of the entire procedure is available [here](https://drive.google.com/file/d/1xe7JnEDpsPZBlCtI_krstdwg1aNZ4Xkf/view?usp=sharing)

## Configuration
Once logged-in, the user has to choose an image to be loaded on the cluster. Choosing a custom image is an option, however some ready-to-use images are available:
```{list-table}
:header-rows: 1
* - *Image*
  - *System*
  - *Included Package(s)*
  - *Default*
* - [ghcr.io/ttedeschi/jlab:wp5-alma8-0.0.34](https://github.com/ttedeschi/custom-images/blob/0.0.34/jupyterlab/Dockerfile.wp5-alma8)
  - AlmaLinux8
  - Python 3.11 + Dask
  - Yes
* - [ghcr.io/ttedeschi/jlab:wp5-alma8-0.0.40](https://github.com/ttedeschi/custom-images/blob/0.0.40/jupyterlab/Dockerfile.wp5-alma8)
  - AlmaLinux8
  - Python 3.11 + Dask + ROOT 6.30
  - No
```

:::{note}
In both cases, a Dask cluster can be spawned on Kubernetes nodes thanks to the *Dask LabExtension*. When using [ghcr.io/ttedeschi/jlab:wp5-alma8-0.0.40](https://github.com/ttedeschi/custom-images/blob/0.0.40/jupyterlab/Dockerfile.wp5-alma8) image, Dask workers are all deployed with the same ROOT version.
:::
