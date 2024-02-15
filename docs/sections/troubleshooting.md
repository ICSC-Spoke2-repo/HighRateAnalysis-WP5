# Troubleshooting and Notes
Some important points to be kept in mind when using the High Rate Analysis platform:
- Each user is assigned 10 Gb of persistent storage
:::{danger}
Anything stored outside the `persistent-storage` area will be lost when the JupyterLab session ends. Moreover, no redundancy nor backup is in place for the `persistent-storage` area. **ALWAYS BACKUP EVERYTHING**!
:::

- It is possible to create a custom Docker image, starting from a common [Dockerfile](https://github.com/ttedeschi/custom-images/blob/main/jupyterlab/Dockerfile.wp5-alma8) of the working images.
:::{attention}
Dask scheduler and workers have to be spawned both with the custom image. Please, modify [this line](https://github.com/ttedeschi/custom-images/blob/main/jupyterlab/labextension.yaml#L14) accordingly!
:::

- A monitoring dashboard setup is being prepared: although not ready yet, please report any kind of issues or bugs!

- Is `cvmfs` filesystem needed? Please, provide feedback
