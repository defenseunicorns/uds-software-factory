# 🏭 UDS Software Factory

[<img alt="Made for UDS" src="https://raw.githubusercontent.com/defenseunicorns/uds-common/refs/heads/main/docs/assets/made-for-uds.svg" height="20px"/>](https://github.com/defenseunicorns/uds-core)
[![Latest Release](https://img.shields.io/github/v/release/defenseunicorns/uds-software-factory)](https://github.com/defenseunicorns/uds-software-factory/releases)
[![Build Status](https://img.shields.io/github/actions/workflow/status/defenseunicorns/uds-software-factory/release.yaml)](https://github.com/defenseunicorns/uds-software-factory/release.yaml)
[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/defenseunicorns/uds-software-factory/badge)](https://api.securityscorecards.dev/projects/github.com/defenseunicorns/uds-software-factory)

This is the integration / wayfinding repository for the UDS Software Factory created and offered by Defense Unicorns.  The Software Factory is made up of bundled applications that assist with development of new software in airgap environments.  These applications are split into `primary` and `lab` applications to denote applications that are ready for wider use and those that we are still learning from and experimenting with.

The `primary` UDS Software Factory packages are:

- [GitLab](https://github.com/defenseunicorns/uds-package-gitlab) - a DevOps software package that can develop, secure, and operate software
- [GitLab Runner](https://github.com/defenseunicorns/uds-package-gitlab-runner) - a Continuous Integration runner that integrates with GitLab
- [Renovate](https://github.com/defenseunicorns/uds-package-renovate) - a dependency checking bot that integrates with GitLab
- [Mattermost](https://github.com/defenseunicorns/uds-package-mattermost) - an open-source, self-hostable online chat service
- [SonarQube](https://github.com/defenseunicorns/uds-package-sonarqube) - an open-source platform developed by SonarSource for continuous inspection of code quality
- [Postgres Operator](https://github.com/defenseunicorns/uds-package-postgres-operator) - a Kubernetes operator to deploy PostgreSQL databases in a cluster
- [Valkey](https://github.com/defenseunicorns/uds-package-valkey) - a Redis-alternative that can be deployed in a cluster (intended for use with GitLab)

The `lab` UDS Software Factory packages are:

- [Sigstore](https://github.com/defenseunicorns/uds-package-sigstore) - a keyless signing infrastructure for software artifact signing and attestations
- [Archivista](https://github.com/defenseunicorns/uds-package-archivista) - a GraphQL datastore for in-toto attestations

This repo serves as an integration repository for testing, creating common [Architectural Decision Records](./adr), and tracking issues that have effects across the individual packages that make up Software Factory.

Also note that the Software Factory team helps to manage the following UDS packages and repositories:

- [UDS Common](https://github.com/defenseunicorns/uds-common) - a common repo to share workflows, UDS tasks and more between UDS Package repositories
- ⚠️ (alpha) [Minio Operator](https://github.com/defenseunicorns/uds-package-minio-operator) - an S3-compatible object storage provider

### 📜 tl;dr - [try it now](#quickstart-demo-bundle)

## Bundles

> [!CAUTION]
> These UDS Bundles are intended for dev, test and demo environments and should _not_ be used for production. They can however serve as examples to create custom bundles.

This repository publishes multiple bundles for dev, test and demo purposes. They are located in sub-directories under `bundles`.

### swf-dev

This bundle is for development of the `primary` Software Factory packages and is located at `bundles/dev`. It requires an existing Kubernetes cluster with at least [UDS Core Base](https://github.com/defenseunicorns/uds-core/tree/main/packages/base) and [UDS Core Identity and Authorization](https://github.com/defenseunicorns/uds-core/tree/main/packages/identity-authorization) on it to deploy.

This bundle requires ~ `9 CPUs and 28GB of memory` available to run.

### k3d-swf-demo

This bundle is a demo bundle of the `primary` Software Factory packages deployed on top of full [UDS Core](https://github.com/defenseunicorns/uds-core). It includes the deployment of an underlying K3d cluster and is located at `bundles/k3d-demo`

This is a fairly large bundle and requires `16 CPUs and 64GB of memory` available to run. It is best deployed on an adequately sized Linux machine with Docker or equivalent installed. This is not currently tested on macOS due to resource limitations.

---

### Quickstart (Demo Bundle)

If you have the resources for it locally (see above), you can deploy the `primary` Software Factory packages with full `uds-core` and `k3d` using the [uds-k3d-swf-demo bundle](./bundles/k3d-demo/README.md).

#### Prerequisites

- [Docker Compatible Runtime](https://docs.docker.com/engine/) necessary for running `k3d`.
- [UDS CLI](https://github.com/defenseunicorns/uds-cli?tab=readme-ov-file#install) v0.10.4 or later

> [!NOTE]
> Apple users follow these [instructions](./docs/development.md) to properly set up your environment to deploy this bundle.

#### Deployment

To deploy this bundle run the following command:

<!-- x-release-please-start-version -->

```bash
uds deploy k3d-swf-demo:0.3.0
```

<!-- x-release-please-end -->

### Quickstart (Dev Bundle)

Alternatively, you can deploy the [uds-swf-dev bundle](./bundles/dev/README.md), which is meant to be deployed on top of [k3d-core-slim-dev](https://github.com/defenseunicorns/uds-core/blob/main/bundles/k3d-slim-dev/README.md) or another Kubernetes cluster with at least [UDS Core Base](https://github.com/defenseunicorns/uds-core/tree/main/packages/base) and [UDS Core Identity and Authorization](https://github.com/defenseunicorns/uds-core/tree/main/packages/identity-authorization). This bundle includes the `primary` Software Factory packages, but only requires part of the underlying `uds-core` baseline allowing it to be run on a wider variety of hardware, particularly with local development in mind.

#### Prerequisites

- [K3D](https://k3d.io/) for dev & test environments or any [CNCF Certified Kubernetes Cluster](https://www.cncf.io/training/certification/software-conformance/#logos) for production-esque environments.
- [UDS CLI](https://github.com/defenseunicorns/uds-cli?tab=readme-ov-file#install) v0.10.4 or later

> [!NOTE]
> Apple users follow these [instructions](./docs/development.md) to properly set up your environment to deploy this bundle.

#### Deployment

For `swf-dev` you have two options, build and deploy from source or deploy the artifacts from where they are hosted in the `ghcr.io` OCI registry.

##### 1. Deploy from Source

To build and deploy from source you can utilize the UDS tasks in this repo by running:

```bash
uds run
```

##### 2. Deploy from Artifacts

_Alternatively_, you can deploy from OCI by running the following two commands:

To easily create a K3d cluster with [UDS Core Base](https://github.com/defenseunicorns/uds-core/tree/main/packages/base) and [UDS Core Identity and Authorization](https://github.com/defenseunicorns/uds-core/tree/main/packages/identity-authorization) run the below command to deploy the `k3d-core-slim-dev` bundle:

> [!TIP]
> You can append `--set INSECURE_ADMIN_PASSWORD_GENERATION=true` to the below command to enable a default keycloak admin. This is useful for development and testing of the SWF stack and enables the ability to run `uds run setup:create-doug-user` to create a user to test with using the username `doug` and the password `unicorn123!@#UN`.

> [!TIP]
> You can install this bundle on nearly any Kubernetes cluster as long as you install the Base and Identity and Authorization layers from UDS Core.  You may need to make some changes to your node configuration which you can see in the [development documentation](./docs/development.md#linux-users).

```bash
uds deploy k3d-core-slim-dev:0.29.1
```

Run the below command to deploy the `swf-dev` bundle on top of the dev cluster:

<!-- x-release-please-start-version -->
```bash
uds deploy swf-dev:0.3.0
```
<!-- x-release-please-end -->

## Development

When developing these bundles it is ideal to utilize the json schemas for UDS Bundles, Zarf Packages and Maru Tasks. This involves configuring your IDE to provide schema validation for the respective files used by each application. For guidance on how to set up this schema validation, please refer to the [guide](https://github.com/defenseunicorns/uds-common/blob/main/docs/uds-packages/development/development-ide-configuration.md) in uds-common.
