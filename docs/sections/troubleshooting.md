# Troubleshooting and Notes
Some important points to be kept in mind when using the High Rate Analysis platform:
- Each user is assigned 10 Gb of persistent storage
:::{danger}
Anything stored outside the `persistent-storage` area will be lost when the JupyterLab session ends. Moreover, no redundancy nor backup is in place for the `persistent-storage` area. **ALWAYS BACKUP EVERYTHING**!
:::

- `cvmfs` is now correctly configured in the platform (both `infn.it` and `cern.ch` repositories).