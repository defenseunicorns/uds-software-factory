# 🏭 UDS Software Factory

This is the integration / wayfinding repository for the Unicorn Delivery Service (UDS) Software Factory created and offered by Defense Unicorns.  In its current state it is made up of the following UDS packages that are bundled together:

- [GitLab](https://github.com/defenseunicorns/uds-package-gitlab) - a DevOps software package that can develop, secure, and operate software
- [GitLab Runner](https://github.com/defenseunicorns/uds-package-gitlab-runner) - a Continuous Integration runner that integrates with GitLab
- [Mattermost](https://github.com/defenseunicorns/uds-package-mattermost) - an open-source, self-hostable online chat service
- [SonarQube](https://github.com/defenseunicorns/uds-package-sonarqube) - an open-source platform developed by SonarSource for continuous inspection of code quality
- [Coder](https://github.com/defenseunicorns/uds-package-coder) (:warning: alpha) - a cloud IDE solution for developing software without local tooling

This repo serves as an integration repository for testing, creating common [Architectural Decision Records](./adr), and tracking issues that have effects across the individual packages that make up Software Factory.

Also note that the Software Factory team helps to manage the following shared UDS packages and repositories:

- [Postgres Operator](https://github.com/defenseunicorns/uds-package-postgres-operator) - a Kubernetes operator to deploy PostgreSQL databases in a cluster
- [Valkey](https://github.com/defenseunicorns/uds-package-valkey) (:warning: alpha) - a Redis-alternative that can be deployed in a cluster (intended for use with GitLab)
- [UDS Common](https://github.com/defenseunicorns/uds-common) - a common repo to share actions, UDS tasks and more between package repositories

### tl;dr - [try it now](#quickstart-demo-bundle)

## Bundles

> [!NOTE]
> These UDS Bundles are intended for dev and test environments and should not be used for production. They can however serve as examples to create custom bundles.

This repository publishes multiple bundles for dev, test and demo purposes. They are located in sub-directories under `bundles`.

### swf-dev

This is a bundle primarily for development that is located at `bundles/dev`. It requires an existing k3d cluster to deploy.

This bundle requires ~ `9 CPUs and 28GB of memory` available to run.

### k3d-swf-demo

This bundle is a demo bundle of Software Factory deployed on top of full [UDS Core](https://github.com/defenseunicorns/uds-core). It includes the deployment of an underlying k3d cluster. The bundle definition is located at `bundles/k3d-demo`

This is a fairly large bundle and requires `16 CPUs and 64GB of memory` available to run. It is best deployed on an adequately sized linux machine with Docker or equivalent installed. This is not currently tested on Mac due to resource limitations.

---

### Quickstart (Demo Bundle)

If you have the resources for it locally (see above), you can deploy the full Software Factory with full `uds-core` and `k3d` using the [uds-k3d-swf-demo bundle](./bundles/k3d-demo/README.md).

#### Prerequisites

- [Docker Compatible Runtime](https://docs.docker.com/engine/) necessary for running `k3d`.
- [UDS CLI](https://github.com/defenseunicorns/uds-cli?tab=readme-ov-file#install) v0.10.4 or later

> [!NOTE]: Apple users follow these [instructions](./docs/development.md) to properly set up your environment to deploy this bundle.

#### Deployment

To deploy this bundle run the following command:

<!-- x-release-please-start-version -->

```bash
uds deploy k3d-swf-demo:0.2.2
```

<!-- x-release-please-end -->

### Quickstart (Dev Bundle)

Alternatively, you can deploy the [uds-k3d-swf-dev bundle](./bundles/dev/README.md), which is meant to be deployed on top of [k3d-core-slim-dev](https://github.com/defenseunicorns/uds-core/blob/main/bundles/k3d-slim-dev/README.md). This bundle includes all of Software Factory, but only utilizes part of the underlying `uds-core` baseline. This allows it to be run on a wider variety of hardware, particularly with local development in mind.

#### Prerequisites

- [K3D](https://k3d.io/) for dev & test environments or any [CNCF Certified Kubernetes Cluster](https://www.cncf.io/training/certification/software-conformance/#logos) for production environments.
- [UDS CLI](https://github.com/defenseunicorns/uds-cli?tab=readme-ov-file#install) v0.10.4 or later

> [!NOTE]: Apple users follow these [instructions](./docs/development.md) to properly set up your environment to deploy this bundle.

#### Deployment

For `swf-dev` you have two options, build and deploy from source or deploy the artifacts from where they are hosted in the ghcr OCI registry.

To build and deploy from source you can utilize the UDS tasks in this repo by running:

```bash
    uds run
```

Alternatively, you can deploy from OCI by running the following two commands:

1. Run the below command to deploy the `k3d-core-slim-dev` bundle:

> [!NOTE]: You can append `--set INSECURE_ADMIN_PASSWORD_GENERATION=true` to the below command to enable a default keycloak admin. This is useful for development and testing of the SWF stack and enables the ability to run `uds run setup:create-doug-user` to create a user to test with using the username `doug` and the password `unicorn123!@#`.

    ```bash
    uds deploy k3d-core-slim-dev:0.22.0
    ```

1. Run the below command to deploy the `swf-dev` bundle on top of the dev cluster:

    <!-- x-release-please-start-version -->
    ```bash
    uds deploy swf-dev:0.2.2
    ```
    <!-- x-release-please-end -->
