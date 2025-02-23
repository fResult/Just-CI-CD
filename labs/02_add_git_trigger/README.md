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

4. Start `cd-pipeline` pipeline run

    We need to run `kubectl port-forward` command to forward the port for the event listener so that we can call it on `localhost`.

    4.1 Use the `kubectl port-forward command to forward port `8090` to `8080`.

        ```console
        ➜ kubectl port-forward service/el-cd-listener  8090:8080
        Forwarding from 127.0.0.1:8090 -> 8080
        Forwarding from [::1]:8090 -> 8080
        ```

    4.2 Open new Terminal, then run `curl` command to send a payload to the event listener service

        ```console
        ➜ curl -X POST http://localhost:8090 \
               -H 'Content-Type: application/json' \
               -d '{"ref":"main","repository":{"url":"https://github.com/fResult/Just-CI-CD"}}'
        {"eventListener":"cd-listener","namespace":"sn-labs-styxmaz","eventListenerUID":"9621fda9-bec7-4a4d-95e7-85cdb14c401d","eventID":"59b816b8-db82-49b5-84f7-fa81d337d418"}
        ```

        This should start a PipelineRun.

        ```console
        ➜ tkn pipelinerun ls
        NAME                   STARTED         DURATION  STATUS
        cd-pipeline-run-hhkpm  10 seconds ago  ---       Running
        ```

        We can see the logs using this command

        ```console
        ➜ tkn pr logs --last
        [clone : checkout] Cloning into 'Just-CI-CD'...

        [lint : echo-message] Calling Flake8 linter...

        [tests : echo-message] Running unit tests with PyUnit...

        [build : echo-message] Building image for https://github.com/fResult/Just-CI-CD...

        [deploy : echo-message] Deploying main branch of https://github.com/fResult/Just-CI-CD...
        ```
