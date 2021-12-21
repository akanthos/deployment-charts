# Helm chart packaging and publish

#### Package helm charts

(The version in Chart.yaml needs to have been adapted according to the latest changes)
```bash 
helm package <chart-directory>
```

#### Generate index.yaml from packaged charts:
```bash 
helm repo index . --url https://akanthos.github.io/deployment-charts/
```

#### Push changes to the repository (via feature branch or in main):
```bash 
git checkout -b chart_update
git commit -am "A description of the chart changes"
git push
```

#### Merge into main
Should be done via Pull Request or with some similar workflow:

#### To install an app with a specific app version use:

```bash 
helm upgrade --install quarkus-app1 -f values-test.yaml deployment-charts/quarkus-chart --version 0.2.0
```

