---
myst:
  substitutions:
    key1: |
      ```{figure} ../img/iam.png
      :width: 400px
      :name: IAM
      ```

    key2: |
      ```{figure} ../img/IAM_registration.png
      :name: registration
      `````  

    key3: |
      ```{figure} ../img/request_plus.png
      :name: group
      ``` 

    key4: |
      ```{figure} ../img/server_options.png
      :name: server_options
      ``` 

    key5: |
      ```{figure} ../img/jupyter_demo.png
      :name: jlab
      ```
---

# Access instructions
This section is dedicated to the instructions in order to access the facility.

The entrypoint is https://hub.131.154.98.51.myip.cloud.infn.it/ and it will redirect the user to the destination JupyterHub, deployed with [WP5 JHub Helm Chart](https://github.com/ttedeschi/HighRateAnalysis-WP5/tree/main/stable/jhub-aas).

Before accessing the hub itself, an **authentication step** is needed, relying on the IAM-ICSC service. 


## Requesting an account on IAM-ICSC
If the user doesn't already have an account, go to this page: https://iam-icsc.cloud.infn.it

|            |            |
| ---------- | ---------- |
| {{ key1 }} | {{ key2 }} |

1. Apply for a new account by clicking on the green "Apply for an account" button (see [Figure top-left](IAM));
1. Login with your INFN-AAI credentials.
1. The page will then redirect you to a form to be filled in (see [Figure top-right](registration)). You can use "Spoke 2 - Quasi interactive analysis of big data with high-throughput"  as motivation;
1. Click on "Register" and wait for an admin approval;
1. After this approval, you'll receive an email to confirm the username and to set the password;
1. Your IAM account is now active. 

You can now login to the IAM page and request access to the `spoke2analysisfacility` group, which will give you access to the High Rate platform.
To do so (see [Figure below](group)), click on `Join a group` under `Group request` and select: `icsc/users/spoke2analysisfacility`. 
You can use the same motivation as before: "Spoke 2 - Quasi interactive analysis of big data with high-throughput".

|            | 
| ---------- | 
| {{ key3 }} | 

After we approve your request, you will be able to access the platform.

## Accessing the platform

1. Go to the entrypoint URL: https://hub.192.135.24.49.myip.cloud.infn.it;

:::warning
The page uses https, but for the moment no CA certificate is used. On your browser, you could receive the message "Connection not secure". Click on "Accept the risk and continue" (or similar) to enter the homepage.
:::

1. Click on `Sign in with OAuth2.0`;
1. Log in using the IAM-ICSC credentials created in the section above;
1. You will now see the Jupyter server possible configurations (see [Figure below](server_options)).

Specifically, you can choose from the dropdown menu:
- The JupyterLab image to use (see "Configuration" section below);
- The required number of cores for the lab session:
   - 1, 2, 4 o 8
- The required memory capacity for the lab session:
   - 2, 4, 8, 16

|            | 
| ---------- | 
| {{ key4 }} | 


Wait for the server startup, which will automatically start the JupyterLab session (see [Figure below](jlab)).

|            | 
| ---------- | 
| {{ key5 }} | 


**Careful**: Currently, each user is allocated 10 GB of persistent storage upon account creation. All files must be saved within the "persistent-storage" directory, otherwise they will be lost each time you exit and enter the platform.

### Logout procedure

To release the resources and correctly logout, simply click on "File -> Hub Control Panel" and then click on "Stop My Server".

## Configuration 
Once logged-in, the user has to choose an image to be loaded on the cluster. Choosing a custom image is an option (see [Create a custom image](./custom-image.md)), however some ready-to-use images are available:
```{list-table}
:header-rows: 1
* - *Image*
  - *System*
  - *Included Package(s)*
  - *Default*
  - *Offloading*
* - [ghcr.io/icsc-spoke2-repo/jlab:wp5-alma9-highrate-v0.1.4](https://github.com/ICSC-Spoke2-repo/wp5-custom-images/blob/highrate/jupyterlab/Dockerfile.wp5-alma9)
  - AlmaLinux9
  - Python 3.11 + Dask + ROOT 6.32.02
  - Yes
  - No (all inside the k8s cluster)
* - [ghcr.io/icsc-spoke2-repo/jlab:wp5-alma9-highrate-offload-v0.0.2-cvmfs-infn](https://github.com/ICSC-Spoke2-repo/wp5-custom-images/blob/highrate-offloading/jupyterlab/Dockerfile.wp5-alma9)
  - AlmaLinux9
  - Python 3.11 + Dask + ROOT 6.32.02
  - Yes
  - Yes (offloading to HTCondor nodes)
```