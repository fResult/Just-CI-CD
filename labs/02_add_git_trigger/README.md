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

2. Apply `cd-binding` trigger binding to the cluster

    ```console
    ➜ kubectl apply -f labs/02_add_git_trigger/triggerbinding.yaml
    triggerbinding.triggers.tekton.dev/cd-binding created
    ```

    Then, check it was created correctly

    ```console
    ➜ tkn triggerbinding ls
    NAME        AGE
    cd-binding  25 minutes ago
    ```

3. Apply `cd-template` trigger template to the cluster

    ```console
    ➜ kubectl apply -f labs/02_add_git_trigger/triggertemplate.yaml
    triggertemplate.triggers.tekton.dev/cd-template created
    ```

    Then, check it was created correctly

    ```console
    ➜ tkn triggertemplate ls
    NAME          AGE
    cd-template   5 minutes ago
    ```
