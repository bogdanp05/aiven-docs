---
title: Optimize PostgreSQL® slow queries
---

PostgreSQL® gives you the ability to
[indentify slow queries](/docs/products/postgresql/howto/identify-pg-slow-queries) using the `pg_stat_statements` view. The following article
contains several common optimization techniques to improve slow running
queries.

## Common slow queries optimizations

It is worth knowing that many database indexes on a table can also cause
problems for write performance due to the overhead of maintaining them.

### Handle an increase in database connections

When your application code scales horizontally to accommodate high
loads, you might find that you inadvertently reach the
[connection limits](/docs/products/postgresql/reference/pg-connection-limits) for your plan. Each connection in PostgreSQL runs in a
separate process, and this makes them more expensive (compared to
threads, for example) in terms of inter-process communication and memory
usage, since each connection consumes a certain amount of RAM.

In such cases, you can use the
[connection pooling](/docs/products/postgresql/concepts/pg-connection-pooling), based on [PgBouncer](https://www.pgbouncer.org), to handle
an increase in database connections. You can add and configure the
connection pooling for your service in the **Pools** view in [Aiven
Console](https://console.aiven.io/).

### Move read-only queries to standby nodes

If your Aiven for PostgreSQL® service is running a *Business* or
*Premium* plan, you have one or more standby nodes available in a high
availability setup in the same cloud region.

To reduce the effect of slow queries on the primary node, you can
redirect read-only queries to the additional
[read-only](create-read-replica) nodes by
directly connecting via the **read-only replica URL** .

### Move read-only queries to a remote read-only replica

You can also create a
[remote read-only replica](create-read-replica) services in the same or different cloud/region providing a
dedicated read-only service that you can use to reduce the query load on
the primary service for read-only queries.
