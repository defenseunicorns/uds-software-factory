# 🏭 UDS Software Factory

This is the integration / wayfinding repository for the Unicorn Delivery Service (UDS) Software Factory created and offered by Defense Unicorns.  In its current state it is made up of the following UDS packages that are bundled together:

- [GitLab](https://github.com/defenseunicorns/uds-package-gitlab) - a DevOps software package that can develop, secure, and operate software
- [GitLab Runner](https://github.com/defenseunicorns/uds-package-gitlab-runner) - a Continuous Integration runner that integrates with GitLab
- [Mattermost](https://github.com/defenseunicorns/uds-package-mattermost) - an open-source, self-hostable online chat service
- [SonarQube](https://github.com/defenseunicorns/uds-package-sonarqube) - an open-source platform developed by SonarSource for continuous inspection of code quality

This repo serves as an integration repository for testing, creating common [Architectural Decision Records](./adr), and tracking issues that have effects across the individual packages that make up Software Factory.

Also note that the Software Factory team helps to manage the following shared UDS packages and repositories:

- [Postgres Operator](https://github.com/defenseunicorns/uds-package-postgres-operator) - a Kubernetes operator to deploy PostgreSQL databases in a cluster
- [UDS Common](https://github.com/defenseunicorns/uds-common) - a common repo to share actions, UDS tasks and more between package repositories

### tl;dr - [try it now](#quickstart)

## Bundles

> [!NOTE]
> These UDS Bundles are intended for dev and test environments and should not be used for production. They also serve as examples to create custom bundles.

This repository publishes multiple bundles for dev, test and demo purposes. They are located in sub-directories under `bundles`.

### swf-dev

This is a bundle primarily for development that is located at `bundles/dev`. It requires an existing k3d cluster to deploy.

This bundle requires ~ 8 CPUs and 28GB of memory available to run.

### k3d-swf-demo

This bundle is a demo bundle of Software Factory deployed on top of full [UDS Core](https://github.com/defenseunicorns/uds-core). It includes the deployment of an underlying k3d cluster. The bundle definition is located at `bundles/k3d-demo`

This is a fairly large bundle and requires `16 CPUs and 64GB of memory` available to run. It is best deployed on an adequately sized linux machine with Docker or equivalent installed. This is not currently tested on Mac due to resource limitations.

---

### Quickstart, Dev & Test Environments

#### Prerequisites

- [K3D](https://k3d.io/) for dev & test environments or any [CNCF Certified Kubernetes Cluster](https://www.cncf.io/training/certification/software-conformance/#logos) for production environments.
<!-- renovate: datasource=github-tags depName=defenseunicorns/uds-cli versioning=semver -->
- [UDS CLI](https://github.com/defenseunicorns/uds-cli?tab=readme-ov-file#install) v0.9.4 or later

#### Quickstart

If you want to try out UDS Software Factory, you can use the [uds-k3d-swf-demo bundle](./bundles/k3d-demo/README.md) to create a local k3d cluster with full UDS Core and Software Factory installed. Note the [requirements](#k3d-swf-demo) mentioned above.

To deploy this bundle run the following command:

<!-- x-release-please-start-version -->

```bash
uds deploy uds-k3d-swf-demo:0.1.1
```

<!-- x-release-please-end -->

Alternatively, you can deploy the [uds-k3d-swf-dev bundle](./bundles/dev/README.md), which is meant to be deployed on top of [k3d-core-slim-dev](https://github.com/defenseunicorns/uds-core/blob/main/bundles/k3d-slim-dev/README.md). This bundle includes all of Software Factory, but only utilizes part of the underlying uds-core baseline. This allows it to be run on a wider variety of hardware, particularly with local development in mind.

> [!NOTE]: Apple users follow these [instructions](./docs/development.md) to properly set up your environment to deploy this bundle.

When `swf-dev` you can have two options, build and deploy from source or deploy the artifacts from where they are hosted in the ghcr OCI registry.

To build and deploy from source you can utilize the UDS tasks in this repo by running:

```bash
    uds run
```

Alternatively, you can deploy from OCI by running the following two commands:

1. Run the below command to deploy the `k3d-core-slim-dev` bundle:

    ```bash
    uds deploy k3d-core-slim-dev:0.16.1
    ```

1. Run the below command to deploy the `swf-dev` bundle on top of the dev cluster:

    <!-- x-release-please-start-version -->
    ```bash
    uds deploy swf-dev:0.1.1
    ```
    <!-- x-release-please-end -->