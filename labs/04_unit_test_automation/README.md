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

2. Modify and use`lint` task with `flake8` in the Pipeline

    ```console
    ➜ kubectl apply -f labs/04_unit_test_automation/pipeline.yaml
    pipeline.tekton.dev/cd-pipeline configured
    ```

3. Run the Pipeline

    ```bash
    ➜ tkn pipeline start cd-pipeline \
          -p repo-url="https://github.com/fResult/Just-CI-CD.git" \
          -p branch="main" \
          -w name=pipeline-workspace,claimName=pipelinerun-pvc \
          --showlog
    ```

    We can see the run pipeline result like this:

    ```console
    ➜ tkn pipeline start cd-pipeline \
    >     -p repo-url=https://github.com/fResult/Just-CI-CD.git \
    >     -p branch=main \
    >     -w name=pipeline-workspace,claimName=pipelinerun-pvc \
    >     --showlog
    PipelineRun started: cd-pipeline-run-cqccb
    Waiting for logs to be available...
    [init : remove] Removing all files from /workspace/source ...

    [clone : clone] <- There will be many lines from git-clone
    [clone : clone] ...
    [clone : clone] ...
    [clone : clone] + printf '%s' https://github.com/fResult/Just-CI-CD.git

    [lint : flake8] ...
    [lint : flake8] ...
    [lint : flake8] Successfully installed [... list of package ...]
    [lint : flake8] ...
    [lint : flake8] [notice] A new release of pip is available: 23.0.1 -> 25.0.1
    [lint : flake8] [notice] To update, run: pip install --upgrade pip
    [lint : flake8] 0

    [tests : echo-message] Running unit tests with PyUnit...

    [build : echo-message] Building image for https://github.com/fResult/Just-CI-CD.git ...

    [deploy : echo-message] Deploying main branch of https://github.com/fResult/Just-CI-CD.git ...
    ```

4. Create a `nose` Test Task

    ```console
    ➜ kubectl apply -f labs/04_unit_test_automation/tasks.yaml
    task.tekton.dev/echo configured
    task.tekton.dev/cleanup configured
    task.tekton.dev/nose created
    ```
