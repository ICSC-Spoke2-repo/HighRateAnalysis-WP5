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

    key5: |
      ```{figure} ../img/IAM_registration.png
      ```
    
    key6: |
      ```{figure} ../img/jupyter_demo.png
      ```  

    key7: |
      ```{figure} ../img/request_plus.png
      ``` 

    key8: |
      ```{figure} ../img/server_options.png
      ``` 

    key9: |
      ```{figure} ../img/signIN.png
      ``` 

    key10: |
      ```{figure} ../img/stop_server.png
      ``` 



---

# Resource Access
This section will be dedicated to the instructions in order to access the facility.

The entrypoint is https://hub.192.135.24.49.myip.cloud.infn.it and it will redirect the user to the destination JupyterHub, deployed with [WP5 JHub Helm Chart](https://github.com/ttedeschi/HighRateAnalysis-WP5/tree/main/stable/jhub-aas).

Before accessing the hub itself, an authentication step is needed, relying on an IAM-DEMO account. 

## Account Requests
If the user doesn't already have an account, please use the same authentication page available at the following link: https://iam-demo.cloud.cnaf.infn.it/

|            |              |
| ---------- | ------------ |
| {{ key3 }} | {{ key9}}    |


By clicking on "Sign in with OAuth 2.0", you will be redirected to the login [page](https://iam-demo.cloud.cnaf.infn.it/)

For first time access:
1. Apply for a new account by clicking on the green "Apply for an account" button (see fig.1);
1. The page will redirect you to a form to be filled in (fig.2). You can use "spoke2 tests" as motivation;
1. Click on "Register" and wait for the confirmation email to the address indicated in the form and click on the URL in the email to confirm;
1. After the first confirmation, the account activation process will start. At the end of this process, you'll receive a second email to confirm the username and to set the password;
1. When you access the new page, you will have to join the "highrate" group.


Your account is now ready to use.

|            | 
| ---------- | 
| {{ key5 }} | 


Once the account creation process is complete, you will receive an email containing a confirmation URL within a few moments.
Upon clicking the URL, you will need to wait for the account activation by the admins. You will receive further communication via email (this may take longer than the initial email), confirming your username and providing a URL for setting the password.
Upon accessing the new page, you will need to join the "highrate" group.


|            | 
| ---------- | 
| {{ key7 }} | 




Once joined to the group (which should be visible in the "Groups" section), through the URL provided, https://hub.192.135.24.49.myip.cloud.infn.it
you will gain access to the resources of the INFN cloud. Specifically, you can choose from the dropdown menu:
- The Operating System image to be used
   - Alma8-0.0.34 + python3.11 + Dask
   - Alma8-0.0.40 + python3.11 + Dask + ROOT 6.30
- The required number of cores
   - 1, 2, 4 o 8
- The required memory capacity
   - 2, 4, 8, 16



|            | 
| ---------- | 
| {{ key8 }} | 


Wait for the server startup and access JupyterHub.


|            | 
| ---------- | 
| {{ key6 }} | 


Careful: Currently, each user is allocated 10 GB of persistent storage upon account creation. All files must be saved within the "persistent-storage" directory, otherwise they will be lost upon resource release

To release the resources, simply click on "File - Hub Control Panel" and then click on "Stop My Server".



* A short video of the entire procedure is available [here](https://drive.google.com/file/d/1xe7JnEDpsPZBlCtI_krstdwg1aNZ4Xkf/view?usp=sharing)*

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
