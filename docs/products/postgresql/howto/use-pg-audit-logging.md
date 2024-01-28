---
title: Collect audit logs in Aiven for PostgreSQL®
sidebar_label: Collect audit logs
pro: true
---

Enable and configure the Aiven for PostgreSQL® audit logging feature on your Aiven for PostgreSQL service. Access and visualize your logs.

:::important
Aiven for PostgreSQL® audit logging is available on
`[Pro Platform](/docs/platform/concepts/pro-platform)`.
:::

## About audit logging

The audit logging feature allows you to monitor and track activities within relational
database systems, which helps to achieve the following:

- Data security and integrity
- Detection and prevention of an unauthorized access, data breaches, and unexpected changes
- Identification of a potential fraud or misuse
- Compliance with regulations and standards required either by an industry or a government
- Accountability (by user and change tracking)
- Improved incident management and root cause analysis
- And [more](/docs/products/postgresql/concepts/pg-audit-logging)

:::note[See also]
For more information on the Aiven PostgreSQL® audit logging feature, check out
[Aiven for PostgreSQL® audit logging](/docs/products/postgresql/concepts/pg-audit-logging).
:::

## Prerequisites

- `[Pro Platform](/docs/platform/concepts/pro-platform)` enabled for your Aiven
  organization
- `[Pro Features](/docs/platform/concepts/pro-platform)` enabled for your Aiven project
- PostgreSQL version 11 or higher
- ``avnadmin`` superuser role
- Depending on what interface you'd like to use to interact with the feature
  - Access to the [Aiven Console](https://console.aiven.io/)
  - [Aiven API](https://api.aiven.io/doc/)
    ([Aiven API Postman collection](https://www.postman.com/aiven-apis/workspace/aiven/collection/21112408-1f6306ef-982e-49f8-bdae-4d9fdadbd6cd))
  - [Aiven CLI client](/docs/tools/cli)
  - SQL

## Enable audit logging

You can enable audit logging by setting the ``pgaudit.featureEnabled`` parameter to
``true`` in your service's advanced configuration. You can do that using the Aiven
[console](https://console.aiven.io/), [API](https://api.aiven.io/doc/),
[CLI](/docs/tools/cli), or SQL.

### Enable in Aiven Console

:::important
In [Aiven Console](https://console.aiven.io/), you can enable audit logging at the service
level only. To enable it on a database or for a user, you need to use SQL.
:::

1. Log in to [Aiven Console](https://console.aiven.io/), and navigate to your organization
   \> project > Aiven for PostgreSQL service.
1. On the **Overview** page of your service, select **Service settings** from the sidebar.
1. On the **Service settings** page, navigate to the **Advanced configuration** section
   and select **Configure**.
1. In the **Advanced configuration** window, select **Add configuration options**, add
   the ``pgaudit.featureEnabled`` parameter, set it to ``true``, and select
   **Save configuration**.

### Enable with Aiven API

You can use the `curl` command line tool to interact with the
[Aiven API](/docs/tools/api). Call the
[ServiceUpdate](https://api.aiven.io/doc/#tag/Service/operation/ServiceUpdate) endpoint to
update your service's configuration by passing ``{"pgaudit.featureEnabled": "true"}`` in
the ``user_config`` object.

```bash
   curl --request PUT                                                                      \
      --url https://api.aiven.io/v1/project/YOUR_PROJECT_NAME/service/YOUR_SERVICE_NAME    \
      --header 'Authorization: Bearer YOUR_BEARER_TOKEN'                                   \
      --header 'content-type: application/json'                                            \
      --data
         '{
            "user_config": {
               "pgaudit.featureEnabled": "true"
            }
         }'
```

### Enable with Aiven CLI

You can use the [Aiven CLI client](/docs/tools/cli) to interact with the
[Aiven API](/docs/tools/api). Run the [avn service update](/docs/tools/cli/service-cli)
command to update your service by setting the ``pgaudit.featureEnabled`` parameter's value
to ``true``.

```bash
avn service update -c pgaudit.featureEnabled=true SERVICE_NAME
```

:::important
By default, audit logging does not emit any audit records. To trigger a logging operation
and start receiving audit records, configure audit logging parameters as detailed in
[Configure audit logging](#configure-audit-logging).
:::

### Enable with SQL

:::note
SQL allows for fine-grained enablement of audit logging: on a database, for a user (role),
or for a database-role combination.
:::

#### Enable on a database

1. [Connect to your Aiven for PostgreSQL service](/docs/products/postgresql/howto/list-code-samples).
1. Run the following query:

   ```sql
   ALTER DATABASE DATABASE_NAME set pgaudit.featureEnabled = 'on'
   ```

#### Enable for a user

1. [Connect to your Aiven for PostgreSQL service](/docs/products/postgresql/howto/list-code-samples).
1. Run the following query:

   ```sql
   ALTER ROLE ROLE_NAME SET pgaudit.featureEnabled = 'on'
   ```

#### Enable on a DB for a user

1. [Connect to your Aiven for PostgreSQL service](/docs/products/postgresql/howto/list-code-samples).
1. Run the following query:

   ```sql
   ALTER ROLE ROLE_NAME IN DATABASE DATABASE_NAME SET pgaudit.featureEnabled = 'on'
   ```

## Configure audit logging {#configure-audit-logging}

:::note
Configuration changes take effect only on new connections.
:::

You can configure audit logging by setting
[its parameters](https://github.com/pgaudit/pgaudit/tree/6afeae52d8e4569235bf6088e983d95ec26f13b7#readme)
using the Aiven [console](https://console.aiven.io/), [API](https://api.aiven.io/doc/),
[CLI](/docs/tools/cli), or SQL.

:::note[Audit logging parameters]
For information on all the parameters available for configuring audit logging, check
[Settings](https://github.com/pgaudit/pgaudit/tree/6afeae52d8e4569235bf6088e983d95ec26f13b7).
:::

### Configure in Aiven Console

:::important
In [Aiven Console](https://console.aiven.io/), you can enable audit logging at the service
level only. To enable it on a database or for a user, you need to use SQL.
:::

1. Log in to [Aiven Console](https://console.aiven.io/), and navigate to your organization
   \> project > Aiven for PostgreSQL service.
1. On the **Overview** page of your service, select **Service settings** from the sidebar.
1. On the **Service settings** page, navigate to the **Advanced configuration** section
   and select **Configure**.
1. In the **Advanced configuration** window, select **Add configuration options**, find a
   desired parameter (all prefixed with ``pgaudit.log``), set its value as needed, and
   select **Save configuration**.

### Configure with Aiven API

You can use the [Aiven API](https://api.aiven.io/doc/) to configure audit logging on your
service. Call the
[ServiceUpdate](https://api.aiven.io/doc/#tag/Service/operation/ServiceUpdate) endpoint
passing desired audit logging parameters in the ``user_config`` object.

```bash
   curl --request PUT                                                                      \
      --url https://api.aiven.io/v1/project/YOUR_PROJECT_NAME/service/YOUR_SERVICE_NAME    \
      --header 'Authorization: Bearer YOUR_BEARER_TOKEN'                                   \
      --header 'content-type: application/json'                                            \
      --data
         '{
            "user_config": {
              "pgaudit": {
                "PARAMETER_NAME": "PARAMETER_VALUE"
              }
            }
          }'
```

### Configure with Aiven CLI

You can use the [Aiven CLI client](/docs/tools/cli) to configure audit logging on
your service by running the following command:

```bash
avn service update -c pgaudit.PARAMETER_NAME=PARAMETER_VALUE SERVICE_NAME
```

### Configure with SQL

:::note
SQL allows for fine-grained configuration of audit logging: on a database, for a user
(role), or for a database-role combination.
:::

#### Configure on a database

1. [Connect to your Aiven for PostgreSQL service](/docs/products/postgresql/howto/list-code-samples).
1. Run the following query:

   ```sql
   ALTER DATABASE DATABASE_NAME SET pgaudit.log_PARAMETER_NAME = PARAMETER_VALUE
   ```

#### Configure for a user

1. [Connect to your Aiven for PostgreSQL service](/docs/products/postgresql/howto/list-code-samples).
1. Run the following query:

   ```sql
   ALTER ROLE ROLE_NAME SET pgaudit.log_PARAMETER_NAME = PARAMETER_VALUE
   ```

#### Configure on a DB for a user

1. [Connect to your Aiven for PostgreSQL service](/docs/products/postgresql/howto/list-code-samples).
1. Run the following query:

   ```sql
   ALTER ROLE ROLE_NAME IN DATABASE DATABASE_NAME SET pgaudit.log_PARAMETER_NAME = PARAMETER_VALUE
   ```

## Configure session audit logging

Session audit logging allows recording detailed logs of all SQL statements and commands
executed during a database session in the system's backend.

To enable the session audit logging, run the following query:

```sql
SELECT aiven_extras.set_pgaudit_parameter('log', 'DATABASE_NAME', 'CLASSES_OF_STATEMENTS_TO_BE_LOGGED');
```

:::note[Example]

   ```sql
   SELECT aiven_extras.set_pgaudit_parameter('log', 'defaultdb', 'read, ddl');
   ```

:::

:::note[See also]
For more details on how to set up, configure, and use session audit logging, check out
[Session audit logging](https://github.com/pgaudit/pgaudit/tree/6afeae52d8e4569235bf6088e983d95ec26f13b7).
:::

## Access your logs

To access audit logs from Aiven for PostgreSQL, you need to create an integration with a
service that allows monitoring and analyzing logs. For that purpose, you can seamlessly
integrate Aiven for PostgreSQL with an Aiven for OpenSearch® service.

### Use Aiven Console

For instructions on how to integrate your service with Aiven for OpenSearch, check
*Enable log integration* in
[Manage OpenSearch® log integration](/docs/products/opensearch/howto/opensearch-log-integration).

### Use Aiven API

Call the
[ServiceIntegrationCreate](https://api.aiven.io/doc/#tag/Service_Integrations/operation/ServiceIntegrationCreate)
endpoint passing the following parameters in the request body:

- ``integration_type``: ``logs``
- ``source_service``: the name of an Aiven for PostgreSQL
- ``destination_service``: the name of an Aiven for OpenSearch service

```bash
   curl --request POST \
     --url https://api.aiven.io/v1/project/{project_name}/integration \
     --header 'Authorization: Bearer REPLACE_WITH_YOUR_BEARER_TOKEN' \
     --header 'content-type: application/json' \
     --data
        '{
           "integration_type": "logs",
           "source_service": "REPLACE_WITH_POSTGRESQL_SERVICE_NAME",
           "destination_service": "REPLACE_WITH_OPENSEARCH_SERVICE_NAME",
        }'
```

### Use Aiven CLI

You can also use :doc:`Aiven CLI (/docs/tools/cli) to create the service integration.

```bash
   avn service integration-create --project $PG_PROJECT \
     -t logs                                            \
     -s $PG_SERVICE_NAME                                \
     -d $OS_SERVICE_NAME
```

:::note[Results]
After the service integration is set up and propagated to the service configuration, the
logs are available in Aiven for OpenSearch. Each log record emitted by audit logging is
stored in Aiven for OpenSearch as a single message, which cannot be guaranteed for
external integrations such as Remote Syslog.
:::

## Visualize your logs

Since your logs are already available in Aiven for OpenSearch, you can use
[OpenSearch Dashboards](/docs/products/opensearch/dashboards) to visualize them. Check out
how to access OpenSearch Dashboards in the
[Aiven for OpenSearch getting started guide](/docs/products/opensearch/get-started).
For instructions on how to start using OpenSearch Dashboards, see
[Getting started](/docs/products/opensearch/dashboards/get-started).

To preview your audit logs in OpenSearch Dashboards, use the filtering tool: select
``AIVEN_AUDIT_FROM``, set its value to `pg`, and apply the filter.

![Audit logging logs in OpenSearch Dashboards](/images/products/postgresql/pgaudit-logs-in-os-dashboards.png)

:::note
If the index pattern in OpenSearch Dashboards had been configured before you enabled the
service integration, the audit-specific AIVEN_AUDIT_FROM field is not available for
filtering. Refresh the fields list for the index in OpenSearch Dashboards under
**Stack Management** → **Index Patterns** → Your index pattern → **Refresh field list**.
:::

## Disable audit logging

You can disable audit logging by setting the ``pgaudit.featureEnabled`` parameter to
``false`` in your service's advanced configuration. You can do that at any time using the
Aiven [console](https://console.aiven.io/), [API](https://api.aiven.io/doc/),
[CLI](/docs/tools/cli), or SQL.

:::important
Audit logging gets disabled automatically if you unsubscribe from Pro Platform or Pro Features.
:::

### Disable in Aiven Console

:::important
In [Aiven Console](https://console.aiven.io/), you can disable audit logging at the
service level only. To disable it on a database or for a user, you need to use SQL.
:::

1. Log in to [Aiven Console](https://console.aiven.io/), and navigate to your organization
   \> project > Aiven for PostgreSQL service.
1. On the **Overview** page of your service, select **Service settings** from the sidebar.
1. On the **Service settings** page, navigate to the **Advanced configuration** section
   and select **Configure**.
1. In the **Advanced configuration** window, select **Add configuration options**, add the
   ``pgaudit.featureEnabled`` parameter, set it to ``false``, and select
   **Save configuration**.

### Disable with Aiven API

You can use the `curl` command line tool to interact with
[the Aiven API](/docs/tools/api). Call the
[ServiceUpdate](https://api.aiven.io/doc/#tag/Service/operation/ServiceUpdate) endpoint to
update your service's configuration by passing ``{"pgaudit.featureEnabled": "false"}`` in
the ``user_config`` object.

```bash
curl --request PUT                                                                      \
   --url https://api.aiven.io/v1/project/YOUR_PROJECT_NAME/service/YOUR_SERVICE_NAME    \
   --header 'Authorization: Bearer YOUR_BEARER_TOKEN'                                   \
   --header 'content-type: application/json'                                            \
   --data
      '{
         "user_config": {
            "pgaudit.featureEnabled": "false"
         }
      }'
```

### Disable with Aiven CLI

You can use the [Aiven CLI client](/docs/tools/cli) to interact with the
[Aiven API](/docs/tools/api). Run the [avn service update](/docs/tools/cli/service-cli)
command to update your service by setting the ``pgaudit.featureEnabled`` parameter's value
to ``false``.

```bash
avn service update -c pgaudit.featureEnabled=false SERVICE_NAME
```

### Disable with SQL

:::note
SQL allows you to disable audit logging on a few levels: database, user (role), or
database-role combination.
:::

#### Disable on a database

1. [Connect to your Aiven for PostgreSQL service](/docs/products/postgresql/howto/list-code-samples).
1. Run the following query:

   ```sql
   ALTER DATABASE DATABASE_NAME set pgaudit.featureEnabled = 'off'
   ```

#### Disable for a user

1. [Connect to your Aiven for PostgreSQL service](/docs/products/postgresql/howto/list-code-samples).
1. Run the following query:

   ```sql
   ALTER ROLE ROLE_NAME SET pgaudit.featureEnabled = 'off'
   ```

#### Disable on a DB for a user

1. [Connect to your Aiven for PostgreSQL service](/docs/products/postgresql/howto/list-code-samples).
1. Run the following query:

   ```sql
   ALTER ROLE ROLE_NAME IN DATABASE DATABASE_NAME SET pgaudit.featureEnabled = 'off'
   ```
