# 🏭 UDS Software Factory

This is the integration / wayfinding repository for the Unicorn Delivery Service (UDS) Software Factory created and offered by Defense Unicorns.  In its current state it is made up of the following UDS packages that are bundled together:

- [GitLab](https://github.com/defenseunicorns/uds-package-gitlab) - a DevOps software package that can develop, secure, and operate software
- [GitLab Runner](https://github.com/defenseunicorns/uds-package-gitlab-runner) - a Continuous Integration runner that integrates with GitLab
- [Mattermost](https://github.com/defenseunicorns/uds-package-mattermost) - an open-source, self-hostable online chat service
- [SonarQube](https://github.com/defenseunicorns/uds-package-sonarqube) - an open-source platform developed by SonarSource for continuous inspection of code quality

This repo serves as an integration repository for testing, creating common [Architectural Decision Records](./adr), and tracking issues that have effects across the individual packages that make up Software Factory.

Also note that the Software Factory team helps to manage the following shared UDS packages:

- [Postgres Operator](https://github.com/defenseunicorns/uds-package-postgres-operator) - a Kubernetes operator to deploy PostgreSQL databases in a cluster
