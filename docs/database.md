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
        - remoteGenerated: Anywhere
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

## External Database Provider (Username/Password)

In order to connect to a database with a username and password you first need to wire up the `postgres.password` key in the UDS config chart to create the required secret reference.  Then, depending on your application, will need to wire in the `username`, `endpoint` and any other connection information as the application requires.  This is a similar process to the configuration for the Postgres Operator above just providing the values needed for your database service.

## AWS IAM Roles for Service Accounts (IRSA)

Software Factory Packages also support IAM Roles for Service Accounts to connect to the Relational Database Service (RDS).  This is done by enabling IRSA on your cluster and creating IAM Roles for Kubernetes Service Accounts to assume.  You can see a guide for how to [setup IRSA on EKS within the AWS documentation](https://docs.aws.amazon.com/emr/latest/EMR-on-EKS-DevelopmentGuide/setting-up-enable-IAM.html).

Wiring this into an application is app specific (and will be documented in that app's `configuration.md` file) but generally involves instructing the app to use IAM roles and then annotating the Service Accounts with the correct Amazon Resource Name (ARN) corresponding to the role you want it to assume.
