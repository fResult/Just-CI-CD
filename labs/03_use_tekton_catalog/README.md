# Use Tekton CD Catalog

This folder holds the files for the lab: *Use Tekton CD Catalog* which is part of the **IBM-CD0215EN-Skills Network Introduction to CI/CD** course.

## Prerequisite

- [Tekton `git-clone` Task](https://hub.tekton.dev/tekton/task/git-clone)

## Available Scripts

> [!note]
> Run these commands below from the root directory.

1. Install `git-clone` task from Tekton catalogue

    ```console
    ➜ tkn hub install task git-clone
    Task git-clone(0.9) installed in default namespace
    ```

    Note: If the above command returns a error due to Tekton Version mismatch, please run the below command to fix this.

    ```bash
    kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-clone/0.9/git-clone.yaml
    ```

2. Create a `echo` and `checkout` tasks

    ```console
    ➜ kubectl apply -f labs/03_use_tekton_catalog/tasks.yaml
    task.tekton.dev/echo created
    task.tekton.dev/checkout created
    ```

3. Create a PersistenceVolumeClaim as Workspace

    ```console
    ➜ kubectl apply -f labs/03_use_tekton_catalog/pvc.yaml
    persistentvolumeclaim/pipelinerun-pvc created
    ```

    We can now reference this persistent volume by its name   `pipelinerun-pvc` when creating workspaces for our Tekton tasks.
