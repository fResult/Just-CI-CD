# Create a base pipeline

This folder holds the files for the lab _Create a Base Pipeline_ which is part of the **IBM-CD0215EN-Skills Network Introduction to CI/CD** course.


## Available Scripts:

> [!note]
> Run these commands below from the root directory

1. Apply `echo-message` task to the cluster.

    ```console
    ➜ kubectl apply -f labs/01_base_pipeline/tasks.yaml
    task.tekton.dev/echo-message created
    ```

2. Apply `hello-pipeline` pipeline to the cluster.

    ```console
    ➜ kubectl apply -f labs/01_base_pipeline/pipeline.yaml
    pipeline.tekton.dev/hello-pipeline created
    ```

3. Then, list the task and pipeline we just created.

    ```console
    ➜ kubectl get pipeline,task
    NAME                                 AGE
    pipeline.tekton.dev/hello-pipeline   20m

    NAME                           AGE
    task.tekton.dev/echo-message   79s
    ```

4. Run the pipeline using the Tekton CLI

    ```console
    ➜ tkn pipeline start --showlog hello-pipeline
    PipelineRun started: hello-pipeline-run-tq7p5
    Waiting for logs to be available...
    [hello : echo] Hello World!
    ```
