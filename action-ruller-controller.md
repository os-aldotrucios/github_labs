# Github Action Runner Controller

## Pre requisitos
Para utilizar ARC, asegúrese de que dispone de lo siguiente.

- Un clúster de Kubernetes

- Para un entorno de nube administrado, puede utilizar AKS. Para obtener más información, consulte Azure Kubernetes Service en la documentación de Azure.
Para una configuración local, puede utilizar minikube o kind. Para obtener más información, consulte minikube start en la documentación de minikube y kind en la documentación de kind.
- Helm 3


## Instalación


1. Instalar el chart de ACR:

    ```sh
    NAMESPACE="arc-systems"
    helm install arc \
        --namespace "${NAMESPACE}" \
        --create-namespace \
        oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set-controller
    ```

2. Instalar ARC Runner:

    ```sh
    INSTALLATION_NAME="arc-runner-set"
    NAMESPACE="arc-runners"
    GITHUB_CONFIG_URL="https://github.com/<your_enterprise/org/repo>"
    GITHUB_PAT="<PAT>"
    helm install "${INSTALLATION_NAME}" \
        --namespace "${NAMESPACE}" \
        --create-namespace \
        --set githubConfigUrl="${GITHUB_CONFIG_URL}" \
        --set githubConfigSecret.github_token="${GITHUB_PAT}" \
        oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set
    ```

3. Verificar la instalación:


    ```sh
    helm list -A
    ```

4. Verificar pods corriendo:

    ```sh
    kubectl get pods -n arc-systems
    ```

5. Probar un runner:

    ```sh
    name: Actions Runner Controller Demo
    on:
    workflow_dispatch:

    jobs:
    Explore-GitHub-Actions:
        # You need to use the INSTALLATION_NAME from the previous step
        runs-on: arc-runner-set
        steps:
        - run: echo "🎉 This job uses runner scale set runners!"
    ```

6. Validar pods creandose:

    ```sh
        kubectl get pods -n arc-systems
    ```