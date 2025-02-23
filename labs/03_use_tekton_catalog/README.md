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

4. Create Pipeline with the `output` Workspace for `git-clone` task

    ```console
    ➜ kubectl apply -f labs/03_use_tekton_catalog/pipeline.yaml
    pipeline.tekton.dev/cd-pipeline created
    ```

    We are now ready to run our pipeline.

5. Run the Pipeline

    ```bash
    tkn pipeline start cd-pipeline \
        -p repo-url=https://github.com/fResult/Just-CI-CD \
        -p branch=main \
        -w name=pipeline-workspace,claimName=pipelinerun-pvc \
        --showlog
    ```

    **We should see output similar to this:**

    ```console
    ➜ tkn pipeline start cd-pipeline \
    >     -p repo-url=https://github.com/fResult/Just-CI-CD \
    >     -p branch=main \
    >     -w name=pipeline-workspace,claimName=pipelinerun-pvc \
    >     --showlog
    PipelineRun started: cd-pipeline-run-94wmr
    Waiting for logs to be available...
    [clone : clone] <- There will be many lines from git-clone
    [clone : clone] ...
    [clone : clone] ...
    [clone : clone] + printf '%s' https://github.com/fResult/Just-CI-CD.git

    [lint : echo-message] Calling Flake8 linter...

    [tests : echo-message] Running unit tests with PyUnit...

    [build : echo-message] Building image for https://github.com/fResult/Just-CI-CD.git ...

    [deploy : echo-message] Deploying main branch of https://github.com/fResult/Just-CI-CD.git ...
    ```

    We can see PipelineRun status following this command.

    ```console
    ➜ tkn pipelinerun ls
    NAME                    STARTED         DURATION   STATUS
    cd-pipeline-run-94wmr   9 minutes ago   32s        Succeeded
    ```

    And also check the logs of the last run with:

    ```console
    ➜ tkn pipelinerun logs --last
    [clone : clone] <- There will be many lines from git-clone
    [clone : clone] ...
    [clone : clone] ...
    [clone : clone] + printf '%s' https://github.com/fResult/Just-CI-CD.git

    [lint : echo-message] Calling Flake8 linter...

    [tests : echo-message] Running unit tests with PyUnit...

    [build : echo-message] Building image for https://github.com/fResult/Just-CI-CD.git ...

    [deploy : echo-message] Deploying main branch of https://github.com/fResult/Just-CI-CD.git ...
    ```
