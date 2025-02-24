# Building an Image

This folder holds the files for the lab: *Building an Image* which is part of the **IBM-CD0215EN-Skills Network Introduction to CI/CD** course.

## Prerequisite

1. Install `git-clone`, `flake8`, and `buildah` tasks from Tekton catalogue

    ```console
    ➜ tkn hub install task git-clone
    Task git-clone(0.9) installed in default namespace

    ➜ tkn hub install task flake8
    Task flake8(0.1) installed in default namespace

    ➜ tkn hub install task buildah
    Task buildah(0.9) installed in default namespace
    ```

2. Apply `tasks.yaml` manifest

    ```console
    ➜ kubectl apply -f labs/05_build_an_image/tasks.yaml
    task.tekton.dev/echo configured
    task.tekton.dev/cleanup configured
    task.tekton.dev/nose configured
    ```


3. check that we have all these tasks.

    ```console
    ➜ tkn task ls
    NAME        DESCRIPTION              AGE
    buildah     Buildah task builds...   2 hours ago
    checkout                             13 hours ago
    cleanup     This task will clea...   12 hours ago
    echo                                 13 hours ago
    flake8      This task will run ...   11 hours ago
    git-clone   These Tasks are Git...   12 hours ago
    nose                                 10 hours ago
    ```

## Available Scripts

1. Apply Tekton Pipeline manifest file

    ```console
    ➜ kubectl apply -f labs/05_build_an_image/pipeline.yaml
    pipeline.tekton.dev/cd-pipeline created
    ```

2. Apply Kubernetes PVC

    ```console
    ➜ kubectl apply -f labs/05_build_an_image/pvc.yaml
    persistentvolumeclaim/pipelinerun-pvc created
    ```

3. Start the Pipeline

    When we start the pipeline, we need to pass in the build-image parameter, which is the name of the image to build.

    This will be different for every learner that uses this lab.\
    Here is the format:

    ```text
    image-registry.openshift-image-registry.svc:5000/$SN_ICR_NAMESPACE/tekton-lab:latest
    ```

    Notice the variable `$SN_ICR_NAMESPACE` in the image name.\
    This is automatically set to point to our container namespace.

    Now, start the pipeline to see your new build task run.\
    Use the Tekton CLI `pipeline start` command to run the pipeline, passing in the parameters `repo-url`, `branch`, and `build-image` using the `-p` option.\
    Specify the workspace `pipeline-workspace` and volume claim `pipelinerun-pvc` using the `-w` option:

    ```bash
    tkn pipeline start cd-pipeline \
        -p repo-url=https://github.com/fResult/Just-CI-CD.git \
        -p branch=main \
        -p build-image=image-registry.openshift-image-registry.svc:5000/$SN_ICR_NAMESPACE/tekton-lab:latest \
        -w name=pipeline-workspace,claimName=pipelinerun-pvc \
        --showlog
    ```

    We should see `Waiting for logs to be available...` while the pipeline runs.\
    The logs will be shown on the screen.\
    Wait until the pipeline run completes successfully

    **Then, check the run status**

    ```console
    ➜ tkn pipelinerun ls
    NAME                    STARTED         DURATION   STATUS
    cd-pipeline-run-6g86v   3 minutes ago   3m10s      Succeeded
    ```

    **We can also check the logs of the last run with:**

    ```console
    ➜ tkn pipelinerun logs --last

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

    [tests : nosetests]
    [tests : nosetests]  REST API Server Tests
    [tests : nosetests] - It should Create a counter
    [tests : nosetests] - It should not Create a duplicate counter
    [tests : nosetests] - It should Delete a counter
    [tests : nosetests] - It should be healthy
    [tests : nosetests] - It should call the index call
    [tests : nosetests] - It should List counters
    [tests : nosetests] - It should Read a counter
    [tests : nosetests] - It should Update a counter
    [tests : nosetests] - It should not Update a missing counter
    [tests : nosetests]
    [tests : nosetests] Name                             Stmts   Miss  Cover   Missing
    [tests : nosetests] --------------------------------------------------------------
    [tests : nosetests] service/__init__.py                  8      0   100%
    [tests : nosetests] service/common/log_handlers.py      11      1    91%   37
    [tests : nosetests] service/common/status.py            45      0   100%
    [tests : nosetests] service/routes.py                   48      0   100%
    [tests : nosetests] --------------------------------------------------------------
    [tests : nosetests] TOTAL                              112      1    99%
    [tests : nosetests] ----------------------------------------------------------------------
    [tests : nosetests] Ran 9 tests in 0.468s
    [tests : nosetests]
    [tests : nosetests] OK
    [tests : nosetests]

    build : build-and-push] STEP 1/10: FROM python:3.9-slim
    [build : build-and-push] Resolved "python" as an alias (/etc/containers/registries.conf.d/000-shortnames.conf)
    [build : build-and-push] Trying to pull docker.io/library/python:3.9-slim...
    [build : build-and-push] Getting image source signatures
    [build : build-and-push] ...
    [build : build-and-push] ...
    [build : build-and-push] COMMIT image-registry.openshift-image-registry.svc:5000/sn-labs-styxmaz/tekton-lab:latest
    [build : build-and-push] --> 276d3418d40
    [build : build-and-push] Successfully tagged image-registry.openshift-image-registry.svc:5000/sn-labs-styxmaz/tekton-lab:latest
    [build : build-and-push] 276d3418d40317aeb9c11187c83b6c2f991323ac6d8ce72554ebba7754d3dbfb
    [build : build-and-push] Getting image source signatures
    [build : build-and-push] Copying blob sha256:e189053df2a794c68187538562fe624a17eb4416683eff679f4b21ee63edb0ad
    [build : build-and-push] ...
    [build : build-and-push] ...
    [build : build-and-push] Writing manifest to image destination
    [build : build-and-push] Storing signatures
    [build : build-and-push] sha256:02896bbf48a64468184529d9a3b21888f72f9d1a1389c8e60d607c9d955c6b1fimage-registry.openshift-image-registry.svc:5000/sn-labs-styxmaz/tekton-lab:latest

    [deploy : echo-message] Deploying main branch of https://github.com/fResult/Just-CI-CD.git ...
    ```
