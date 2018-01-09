Anchore Engine Helm Chart
=========================

This chart deploys the Anchore Engine docker container image analysis system. Anchore Engine
requires a PostgresSQL database (>=9.6) which may be handled by the chart of supplied externally,
and executes in a 2-tier architecture with an api/control layer and a batch execution worker pool layer.


Chart Details
-------------

The chart is split into three primary sections: GlobalConfig, CoreConfig, WorkerConfig. As the name implies,
the GlobalConfig is for configuration values that all components require, while the Core and Worker sections are
tier-specific and allow customization for each role.




Installing the Chart
--------------------


Deploying PostgreSQL as a dependency:


Using and existing/external PostgreSQL service:



Configuration
-------------

While the configuration options of Anchore Engine are extensive, the options provided by the chart are:

Global Configuration Options:
* globalConfig.dbConfig.ssl: True/False to enable/disable ssl on db connection (default: 'True')





Adding Workers
--------------

To set a specific number of workers once the service is running:

`helm upgrade --set workerConfig.replicaCount=2`

To launch with more than one worker you can either modify values.yaml or run with:

`helm install --set workerConfig.replicaCount=2 <chart location>`

