# Create a base pipeline

This folder holds the files for the lab _Create a Base Pipeline_ which is part of the **IBM-CD0215EN-Skills Network Introduction to CI/CD** course.


## Available Scripts:

> [!note]
> Run these commands below from the root directory

1. Apply `echo-message` task to the cluster.

    ```console
    ➜ kubectl apply -f labs/01_base_pipeline/tasks.yaml
    task.tekton.dev/echo created
    task.tekton.dev/checkout created
    ```

2. Apply `hello-pipeline` pipeline to the cluster.

    ```console
    ➜ kubectl apply -f labs/01_base_pipeline/pipeline.yaml
    pipeline.tekton.dev/hello-pipeline created
    pipeline.tekton.dev/cd-pipeline created
    ```

3. Then, list the task and pipeline we just created.

    ```console
    ➜ kubectl get pipeline,task
    NAME                                 AGE
    pipeline.tekton.dev/cd-pipeline      10s
    pipeline.tekton.dev/hello-pipeline   10s

    NAME                       AGE
    task.tekton.dev/checkout   15s
    task.tekton.dev/echo       15s
    ```

4. Run the pipeline using the Tekton CLI

    **Run the `hello-pipeline`:**

    ```console
    ➜ tkn pipeline start hello-pipeline --showlog -p message=Tekkon
    PipelineRun started: hello-pipeline-run-qnlwk
    Waiting for logs to be available...
    [hello : echo-message] Hello Tekkon
    ```

    **Run the `ci-pipeline`:**

    ```bash
    tkn pipeline start cd-pipeline \
          --showlog \
          -p repo-url=https://github.com/fResult/Just-CI-CD \
          -p branch=main
    ```

    Then, we can see the result like this.

    ```console
    PipelineRun started: cd-pipeline-run-7dxxg
    Waiting for logs to be available...
    [clone : checkout] Cloning into 'Just-CI-CD'...
    ```
