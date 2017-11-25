Overview
========

This set of configurations differs from that of the 'simple' directory in that it splits anchore-engine into two different kubernetes deployment definitions.

The 'core' deployment definition is a single pod comprising the main API services:
* external api
* catalog
* queues
* policy_engine
* kubernetes webhook handler

These services tend to be low resource consumption are not a primary block on analysis throughput. This deployment has a corresponding service
definition to enable the other deployment to access the various services via the cluster IP.

It is possible to further split this deployment in two again to deploy the policy-engine service on its own, 
but that is an exercise left for later but will follow the same pattern seen here (basically a service definition, deployment definition, and a config.yaml with only policy-engine enabled).

The second service, the 'analyzer' service can be scaled-out and includes a single pod definition for the analyzer processes that
actually do the image fetch, unpack, and analysis. These workers are the primary resource consumers in the process and allowing parallelism increases system throughput.


Persistence
===========

A single postgresql db is required and expected in these configs to be deployed as 'anchore-db-postgresql' service and thus available
in environment variables and secrets using that prefix (I installed using a helm chart, for example).

Anchore Engine Core
===================

Create initial service definition with port mappings for each service.
Create configmap of the core-config.yaml (use name config.yaml in the creation operation, or set the key in the deployment yaml). The config should have the 'analyzer' service enabled=False.
Create the deployment.


Anchore Engine Analyzers
========================

Create configmap of worker-config.yaml (renamed to config.yaml during create operation or mapped in key of analyzer deployment to show up as /config/config.yaml in volume).
Note that this config has all services except 'analyzer' set to enabled: False.
Create the deployment.

As needed you can scale the deployment (e.g. kubectl scale --replicas=3 deployment/anchore-engine-analyzer-deployment)


