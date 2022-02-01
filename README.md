# Coroot-pg-agent

Coroot-pg-agent is an open-source prometheus exporter that gathers metrics from Postgres servers.

## Philosophy

Metrics should help you identify issues with your Postgres servers.  
This agent collects metrics that can help you answer questions such as:
  * What queries are consuming the most IOPS or CPU time?
  * Which transactions are hanging in the *Idle in transaction* state?
  * Which query is holding the lock?

This provides visibility into Postgres performance without the need to navigate through system views manually.
  
## Features

### Comprehensive query metrics

The agent aggregates data from *pg_stat_statements* and *pg_stat_activity* to provide accurate
metrics about queries, whether they are completed or still running.

### Locks monitoring

It is not enough to gather the number of active locks from *pg_locks*. 
What engineers really want to know is which query is blocking other queries.
The [pg_lock_awaiting_queries](https://coroot.com/docs/metrics/pg-agent#pg_lock_awaiting_queries) metric can provide the answer to that.

### Query normalization and obfuscation

In addition to query normalization, which Postgres does, the agent obfuscates all queries so that no sensitive data gets into the metrics labels.

## Quick start

### Create database role

    create role <USER> with login password '<PASSWORD>';
    grant pg_monitor to <USER>;

### Enable pg_stat_statements

    create extension pg_stat_statements;
    select * from pg_stat_statements; -- to check

### Run

    docker run --detach --name coroot-pg-agent \
    --env DSN="postgresql://<USER>:<PASSWORD>@<HOST>:5432/postgres?connect_timeout=1&statement_timeout=30000" \
    ghcr.io/coroot/coroot-pg-agent

## Metrics

The collected metrics are described [here](https://coroot.com/docs/metrics/pg-agent).

## License

Coroot-pg-agent is licensed under the [Apache License, Version 2.0](https://github.com/coroot/coroot-node-agent/blob/main/LICENSE).
