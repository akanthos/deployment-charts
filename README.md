# Helm chart packaging and publish

## Packaging 

#### Package helm charts

(The version in Chart.yaml needs to have been adapted according to the latest changes)
```bash 
helm package <chart-directory>
```

## Exposing the helm charts

### Exposing via Github Pages

1. Enable Github Pages 
Check the documentatation [here](https://docs.github.com/en/pages/quickstart).

2. Generate index.yaml from packaged charts:
    ```bash 
    helm repo index . --url https://akanthos.github.io/deployment-charts/
    ```

3. Push changes to the repository (via feature branch or in main):
    ```bash 
    git checkout -b chart_update
    git commit -am "A description of the chart changes"
    git push
    ```

4. Merge into main
Should be done via Pull Request or with some similar workflow:

### Exposing via Azure Container Registry

Documentation can be found [here](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-helm-repos).

Prerequisites:
* An Azure subscription
* Azure Container Registry instance
* an access-token (we'll name it ***helm-token*** in this example)


1. Package your application as shown in the first section.
2. Login to your ACR using azure-cli
3. Enable helm experimental features:
    ```bash
    export HELM_EXPERIMENTAL_OCI=1
    ```
4. 
    ```bash
    helm registry login $ACR_NAME.azurecr.io \
      --username $USER_NAME \
      --password $PASSWORD
    helm push charts/quarkus-chart-0.1.0.tgz oci://$ACR_NAME.azurecr.io/deployment-charts
    ```

## Installing a specific app version

#### Using the local definitions

Installing an app on a k8s cluster using helm 3 can be done with the example command: 
```bash 
helm upgrade --install quarkus-app1 -f values-test.yaml deployment-charts/quarkus-chart --version 0.2.0
```

#### Using the charts exposed via Github Pages

```bash 
helm repo add https://akanthos.github.io/deployment-charts/
helm repo update
helm upgrade --install quarkus-app1 -f values-test.yaml deployment-charts/quarkus-chart --version 0.2.0
```

#### Using the charts exposed via Azure Container Registry as OCI artifacts

```bash 
az acr helm repo add -n $ACR_NAME
helm repo update
helm upgrade --install quarkus-app1 oci://$ACR_NAME.azurecr.io/deployment-charts/quarkus-chart --version 0.2.0
```

**NOTE:** `az acr helm repo` is deprecated and helm 3 should be used.
