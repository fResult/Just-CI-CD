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
    ➜ tkn tsk ls
    NAME        DESCRIPTION              AGE
    buildah     Buildah task builds...   2 hours ago
    checkout                             13 hours ago
    cleanup     This task will clea...   12 hours ago
    echo                                 13 hours ago
    flake8      This task will run ...   11 hours ago
    git-clone   These Tasks are Git...   12 hours ago
    nose                                 10 hours ago
    ```
