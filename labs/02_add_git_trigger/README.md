# Adding GitHub Triggers

This folder holds the files for the lab: *Adding GitHub Triggers* which is part of the **IBM-CD0215EN-Skills Network Introduction to CI/CD** course.

## Available Scripts

> [!note]
> Run these commands below from the root directory

1. Apply `cd-listener` event listener to the cluster.

    ```console
    ➜ kubectl apply -f labs/02_add_git_trigger/eventlistener.yaml
    eventlistener.triggers.tekton.dev/cd-listener created
    ```

    Then, check it was created correctly.

    ```console
    ➜ tkn eventlistener ls
    NAME         AGE            URL                                                   AVAILABLE
    cd-listener  8 minutes ago  http://el-cd-listener.default.svc.cluster.local:8080  True
    ```
