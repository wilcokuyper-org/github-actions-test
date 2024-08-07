# Github Runners

We use the Actions Runner Controller kubernetes operator that self-hosting Github action runners.

See: https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners-with-actions-runner-controller

## Prerequisites

In order to be able to install you need to create a Github App on the organization and a secret containing the required variables.

Follow the steps in https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners-with-actions-runner-controller/authenticating-to-the-github-api#authenticating-arc-with-a-github-app to set this up.

## Installation
The runners require 2 separate helm releases:

- Controller : Operator for orchestration and runner scaling
- Runner Set : The actual runners that run the actions. These will be scaled up and down by the controller when needed. You need to use the runner set name (i.e. github-runner-set, as specified in the runner.yaml file) in the runs-on key a project's github action.

Controller:
```shell
helm install arc --namespace github --create-namespace oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set-controller -f controller.yaml --version 0.9.3
```

Runners set:

```shell
helm install github-runner-set --namespace github --create-namespace oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set -f runner-set.yaml --version 0.9.3
```

If you want to upgrade values, use the following

Controller:
```shell
helm upgrade arc --namespace github --reuse-values oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set-controller -f controller.yaml --version 0.9.3
```

Runners set:

```shell
helm upgrade github-runner-set --namespace github --reuse-values oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set -f runner-set.yaml --version 0.9.3
```
