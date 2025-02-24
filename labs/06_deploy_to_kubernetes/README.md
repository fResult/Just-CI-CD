# Deploy to Kubernetes / OpenShift

This folder holds the files for the lab: _Deploy to Kubernetes_ which is part of the **IBM-CD0215EN-Skills Network Introduction to CI/CD** course.

## Prerequiste

Install `git-clone`, `flake8`, `buildah`, and  `openshift-client` tasks from Tekton catalogue

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

## Available Scripts

1. Apply Tekton manifest files

    ```console
    ➜ kubectl apply -f labs/06_deploy_to_kubernetes
    pipeline.tekton.dev/cd-pipeline created
    persistentvolumeclaim/pipelinerun-pvc created
    task.tekton.dev/echo created
    task.tekton.dev/cleanup created
    task.tekton.dev/nose created
    ```

2. Start the pipeline

    When we start the pipeline, we now need to pass in the`app-name` parameter, which is the name of the application to deploy.

    Our application is called `hitcounter` so this is the name that we will pass in, along with all the other parameters from the previous steps.

    Now, start the pipeline to see your new deploy task run.\
    Use the Tekton CLI `pipeline start` command to run the pipeline, passing in the parameters `repo-url`, `branch`, `app-name`, and `build-image` using the `-p` option.\
    Specify the workspace `pipeline-workspace` and persistent volume claim `pipelinerun-pvc` using the `-w` option:

    ```bash
    tkn pipeline start cd-pipeline \
        -p repo-url=https://github.com/fResult/Just-CI-CD.git \
        -p branch=main \
        -p app-name=hitcounter \
        -p build-image=image-registry.openshift-image-registry.svc:5000/$SN_ICR_NAMESPACE/tekton-lab:latest \
        -w name=pipeline-workspace,claimName=pipelinerun-pvc \
        --showlog
    ```

    We should see `Waiting for logs to be available...` while the pipeline runs.\
    The logs will be shown on the screen.\
    Wait until the pipeline run completes successfully.

    **Then, check the run status**
