---
title: About logging, metrics and alerting
---

Administrators can configure log and metrics integrations to Aiven
services so that you can monitor the health of your service.

## Logs

On the **Overview** page of your service, select **Integrations** to add
an integration that will send service logs to an Aiven for OpenSearch®
service. This can be an existing service, or you can choose to create a
new one.

## Metrics

On the **Overview** page of your service, select **Integrations** to set
up an integration to push service metrics to an M3, InfluxDB® or
PostgreSQL® service on Aiven. This can be an existing service or you can
create a new one to receive the metrics.

## Dashboards

You can create custom OpenSearch® or Grafana® dashboards to monitor the
service.

## Alerts

The platform also has alert policies to notify you via email when a key
metric rises above or below a set threshold, such as low memory or high
CPU consumption. For information on setting the addresses for these
emails, see
[this article](/docs/platform/howto/technical-emails).
