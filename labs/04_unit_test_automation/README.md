# Integrate Unit Test Automation

This folder holds the files for the lab: *Integrate Unit Test Automation* which is part of the **IBM-CD0215EN-Skills Network Introduction to CI/CD** course.

## Prerequisite

1. Establish the Tasks

    ```console
    ➜ kubectl apply -f labs/04_unit_test_automation/tasks.yaml
    task.tekton.dev/echo created
    task.tekton.dev/cleanup created
    ```

    > [!note]
    > If the above command for installing git-clone task returns a error due to Tekton Version mismatch, please run the below command to fix this.
    > ```console
    > ➜ kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-clone/0.9/git-clone.yaml
    > task.tekton.dev/git-clone created
    > ```

    Check that we have all these tasks.

    ```console
    ➜ tkn task ls
    NAME        DESCRIPTION              AGE
    cleanup     This task will clea...   2 minutes ago
    echo                                 2 minutes ago
    git-clone   These Tasks are Git...   2 minutes ago
    ```

2. Establish the Workspace

    **We also need a *PersistentVolumeClaim (PVC)* to use as a workspace.\
    Apply the following `pvc.yaml` file to establish the PVC:**

    ```console
    ➜ kubectl apply -f labs/04_unit_test_automation/pvc.yaml
    persistentvolumeclaim/pipelinerun-pvc created
    ```

    We can now reference this persistent volume claim by its name `pipelinerun-pvc` when creating workspaces for your Tekton tasks.

## Available Scripts

1. Add the flake8 Task

    We are going to use `flake8` to lint our code.\
    Luckily, there is it in the Tekton Catalogue.\
    So, we can install by this command:

    ```console
    ➜ tkn hub install task flake8
    Task flake8(0.1) installed in default namespace
    ```
