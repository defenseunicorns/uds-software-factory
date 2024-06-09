# Database

Software Factory applications that require a database are configured to use Postgresql as a backend.  This service can be provided to a given application in a few supported ways:

## UDS Postgres Operator Package

If your environment requires an in-cluster solution, you can make use of the [UDS Postgres Operator Package](https://github.com/defenseunicorns/uds-package-postgres-operator).  This can be included in your `uds-bundle.yaml` and is configured through the `postgresql` Helm value to create the required CRs for the operator.  Below is an example of what this configuration would look like for an application (GitLab in this case):

```yaml
postgresql:
    enabled: true
    teamId: "uds"
    volume:
        size: "10Gi"
    numberOfInstances: 2
    users:
        gitlab.gitlab: []
    databases:
        gitlabdb: gitlab.gitlab
    version: "13"
    ingress:
        remoteGenerated: Anywhere
```

Within your bundle you are free to make this a `value` or a `variable` override of the `uds-postgres-config` chart within the `postgres-operator` component as desired.

> [!IMPORTANT]
> The above configuration sets the `gitlabdb` to be owned by the `gitlab.gitlab` user which will translate to `{namespace}.{username}` to create a secret that shares the name of the username within the specified namespace.  You can learn more about configuring the databases and operator within the [Postgres Operator docs](https://github.com/zalando/postgres-operator/tree/master/docs).

To configure this database within an application you usually will need the secret reference, the username, and the database endpoint.  For the above those would be:

- Username: `gitlab.gitlab`
- Secret Reference: `gitlab.gitlab.pg-cluster.credentials.postgresql.acid.zalan.do`
- Database Endpoint: `pg-cluster.postgres.svc.cluster.local`

> [!TIP]
> You can find a practical example of how this is configured within a bundle inside of the `bundles/k3d-demo` bundle where the [Postgres Operator is included](https://github.com/defenseunicorns/uds-software-factory/blob/5c7f9fea76ec074a17555109fc55f733e3d27747/bundles/k3d-demo/uds-bundle.yaml#L39) and where it is [configured to work with applications](https://github.com/defenseunicorns/uds-software-factory/blob/5c7f9fea76ec074a17555109fc55f733e3d27747/bundles/k3d-demo/uds-bundle.yaml#L180).

## AWS Relational Database Service (RDS)

If you are in AWS you can configure your applications to connect to RDS to provide a database.  This connection can be provided in one of two ways: 

### Raw Username / Password / Endpoint

<!-- TODO: (@WSTARR) Should we allow for configuration similar to https://github.com/defenseunicorns/uds-package-mattermost/blob/main/chart/values.yaml#L11 so that we don't need to pre-create secrets for GL / SQ? -->

### IAM Roles for Service Accounts (IRSA)

<!-- TODO: (@WSTARR) Fill out this section -->
