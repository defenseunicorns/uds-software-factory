# Networking

Software Factory applications are configured by default to assume they will connect to the internal dependencies that are used for testing (see examples such as `minio`, `redis` and `postgres` in a given `bundle/uds-bundle.yaml`). This is intentional, so as to not create overly permissive network policies by default. For example, there is not a default setup of egress anywhere for pods that may need to connect to external storage.

> [!IMPORTANT]
> If you are using different internal services, cloud services or a mix you **MUST** configure values in the given config chart accordingly via bundle overrides. A couple of example are provided below.

Configure an application with external services:

```yaml
# charts/config/values.yaml
storage:
  internal: false
redis:
  internal: false
postgres:
  internal: false
```

Configure an application with external postgres and s3, but in-cluster redis:

```yaml
# charts/config/values.yaml
storage:
  internal: false
postgres:
  internal: false
redis:
  internal: true
  selector:
    app.kubernetes.io/name: redis
  namespace: redis
  port: 6379
```

> [!TIP]
> There may be a need to integrate with other in-cluster services that are not a part of the standard connectivity needed by the application (as an example a Jira integration with GitLab). As such, Software Factory applications provide the ability to add custom rules to allow additional internal network connectivity.

Add custom rule:

```yaml
# charts/config/values.yaml
custom:
   # Notice no `remoteGenerated` field here on custom internal rule
  - direction: Ingress
    selector:
      app: webservice
    remoteNamespace: jira
    remoteSelector:
      app: jira
    port: 8180
    description: "Ingress from Jira"
  # No `remoteNamespace`, `remoteSelector`, or `port` fields on rule to `remoteGenerated`
  - direction: Egress
    selector:
      app: webservice
    remoteGenerated: Anywhere
    description: "Egress from Webservice"
```

> [!NOTE]
> The above is just an example of what can be done with the custom key and not representative what any specific integration would need to look like.
