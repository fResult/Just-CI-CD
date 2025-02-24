# Deploy to Kubernetes / OpenShift

This folder holds the files for the lab: _Deploy to Kubernetes_ which is part of the **IBM-CD0215EN-Skills Network Introduction to CI/CD** course.

## Prerequiste

1. Install `git-clone`, `flake8`, `buildah`, and  `openshift-client` tasks from Tekton catalogue

    ```console
    ➜ tkn hub install task git-clone
    Task git-clone(0.9) installed in default namespace

    ➜ tkn hub install task flake8
    Task flake8(0.1) installed in default namespace

    ➜ tkn hub install task buildah
    Task buildah(0.9) installed in default namespace

    ➜ tkn hub install task openshift-client
    Task openshift-client(0.2) installed in default namespace
    ```
