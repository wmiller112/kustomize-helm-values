# About
This repo contains a demonstration of Kustomizing values of a Helm chart that is being deployed via the [Kustomize Helm Chart inflator](https://github.com/kubernetes-sigs/kustomize/blob/master/examples/chart.md) by generating the `HelmChartInflationGenerator` via a separate kustomization. This repo is structured as follows

- `envs/`
  - `base/` - Create a namespace
  - `patches/`
    - `demo1/` - Point to `helm/base` as a `generator`
    - `demo2/` - Create a serviceAccount named `kustomize-created`, and points to `helm/patches/demo2` as a `generator`


- `helm/` - base and patches for `HelmChartInflationGenerator`
  - `base/` - Points to hello-world chart, sets `valuesInline`: image.tag to `1.15`
  - `patches/`
    - `demo2` - sets `valuesInline`: serviceAccount.create `false`, serviceAccount.name: `kustomize-created`

## Try it out
### Demo1  
`kustomize build envs/patches/demo1 --enable-helm > results/demo1.yaml`

- Namespace
- Hello-world helm chart resources
  - `1.15` image version on the deployment
  - hello-world serviceAccount created by the helm chart

### Demo2  
`kustomize build envs/patches/demo2 --enable-helm > results/demo2.yaml`

- Namespace
- Hello-world helm chart resources
  - `1.15` image version on the deployment
  - No hello-world serviceAccount
  - `kustomize-created` serviceAccount with label `app.kubernetes.io/managed-by: Kustomize`
  - deployment points to this replacement serviceAccount
