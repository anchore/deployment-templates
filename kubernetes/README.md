# Deploying Anchore Engine on Kubernetes

Anchore Engine can be deployed in several configurations on a Kubernetes cluster, so initially we will provided examples for a dev/test deployment and a production deployment that scales.

## Simple (dev/test)
The simple deployment runs all services in a single container in a single pod. This is basically the same as the docker-compose configuration
directly in the anchore-engine source-tree.

## Scalable (production)
A more complex configuration is recommended for production usage to allow the worker services to scale out as needed to achieve more analysis throughput by allowing more concurrent analysis. Each worker currently performs a single analysis at a time as fed from a queue.

Details for each are in the respective directories.


