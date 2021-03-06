---
Title: Using the OSS Cluster API
description: 
weight: $weight
alwaysopen: false
categories: ["RS"]
---
{{%excerpt-include filename="rs/concepts/data-access/oss-cluster-api.md" %}} 
For more information, see [Redis OSS Cluster API]({{< relref "/rs/concepts/data-access/oss-cluster-api.md" >}}).

Note: The OSS Cluster API setting is not cluster-wide. 
The setting only applies to the specified database.

## Configuring an RS Database to Use the OSS Cluster API

To configure an RS database to use the OSS Cluster API:

1. Make sure that the database meets these requirements:
    * The database must use the standard hashing policy.
    * The database proxy policy is `all-master-shards`.
    * The database proxy policy must not use node `include` or `exclude`.
1. Find the database ID to make sure that we convert the correct database.

    ```sh
    $ sudo rladmin info db | grep test
    db:4 [test]:
    ```

    In this example, the db ID is 4.

1. Using the rladmin command line utility, enable the OSS Cluster API 
for the specified database.

    ```sh
    $ sudo rladmin
    rladmin> tune db <database name or ID> oss_cluster enable
    ```

    Note: To disable OSS Cluster API with rladmin, run: `tune db <database name or ID> oss_cluster disable`

1. Reconfigure the database to load the new settings and restart the endpoint proxy.

    ```sh
    $ sudo rlutil dmc_reconf bdb=<db-id>
    ```

To get the benefits of using the OSS Cluster API, make sure that you are using 
Redis clients that are cluster-aware, such as redis-py-cluster or jedis.
