# Infrastructure Details
The infrastructure consists of a k8s cluster made of:
- A Master node with 16 CPUs, 32 GB RAM, 80 GB disk;
- 7 worker nodes with 16 CPUs, 32 GB RAM, 80 GB disk.

On such cluster, a JupyterHub is deployed where the users can deploy a JupyterLab session to get access to a full IDE (persistent storage, terminal, notebooks, editors, ...).

Users can scale up their computation, using Dask, by offloading to external resources:
- 70 worker nodes, each with 502 GB RAM and 96 core in total, shared among projects;
- Access "grid-like" with `cvmfs` available (both `cern.ch` and `infn.it` repositories).

These nodes are accessible via HTCondor-CE and the connection is totally transparent for the user (using the [InterLink](https://www.intertwin.eu/article/infrastructure-component-interlink) service).

Below a simplified diagram of the High Rate platform:

![image](../img/infrastructure.png)
