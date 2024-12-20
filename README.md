# stack-deploy-github-action
Github action used to deploy a WordPress site to Stack.

# Usage

## Example workflow

```yaml
name: deploy

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: Publish to Registry
      uses: elgohr/Publish-Docker-Github-Action@master
      id: build
      with:
        name: ghcr.io/blueoceanideas/stack-deployer
        username: ${{ secrets.GITHUB_USERNAME }}
        password: ${{ secrets.GITHUB_TOKEN }}
        registry: ghcr.io
    - name: Deploy on Stack
      uses: presslabs/stack-deploy-github-action
      env:
        GOOGLE_CREDENTIALS: ${{ secrets.GOOGLE_CREDENTIALS }}
      with:
        namespace: proj-rl3e02
        wordpress: myawesomesite-2e304
        image: ghcr.io/blueoceanideas/stack-deployer:${{ steps.build.outputs.tag }}
        google_project: vlads-ureactor
        google_zone: europe-west2
        google_cluster: stack
```

## Arguments

`image` - newly built docker image to deploy

`namespace` - the namespace in which you want to deploy

`wordpress` - wordpress resource name that you want to update

`google_project` - google cloud project where the k8s cluster is

`google_zone` - google cloud zone where the k8s cluster is

`google_cluster` - kubernetes cluster name
