Running Anchore Engine in Kubernetes
====================================

Anchore Engine in minimal configuration requires 2 containers to be executing: anchore-engine itself and the postgresql database.
Because there are many options for running and managing a postgresql database and because the lifecycle of the db should be
independent of the lifecycle of the service container, we recommend deploying them in distinct units (deployments or replication controllers)
rather than as a single pod configuration. This also allows for general use where the db may be hosted outside of the containter system (e.g. AWS RDS or GCP SQL).

The recommended high-level deployment architecture is:
* anchore-engine-db - a postgresql db running as its own service to decouple the lifecycle of the db and anchore-engine.
* anchore-engine - deployment, exposed as a service (but could be a single pod instead of a deployment).


For this example, I am running on GKE as it is a simple way to get a kubernetes cluster running, so the versions and capabilities available are slighty different if you're running an older version. I've tested against:
```
$# kubectl version
Client Version: version.Info{Major:"1", Minor:"8", GitVersion:"v1.8.2", GitCommit:"bdaeafa71f6c7c04636251031f93464384d54963", GitTreeState:"clean", BuildDate:"2017-10-24T19:48:57Z", GoVersion:"go1.8.3", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"7+", GitVersion:"v1.7.8-gke.0", GitCommit:"a7061d4b09b53ab4099e3b5ca3e80fb172e1b018", GitTreeState:"clean", BuildDate:"2017-10-10T18:48:45Z", GoVersion:"go1.8.3", Compiler:"gc", Platform:"linux/amd64"}
```

I'm also using the default config.yaml that exposes anchore-engine's external port as 8228, but that can easily be change to whatever makes sense for you.


The Database
------------

Anchore currently has a hard requirement on a postgresql database.

If using a kubernetes-deployed postgresql db, deploy as a separate service (helm chart works too):
* Deployment: anchore-postgresql, single pod with postgresql:9.7 image (or latest if you're brave)
* Service: anchore-engine-db
* Secrets: db user, db password

For this example, I just use a helm chart to deploy a database service:
```
helm init
helm install postgres
```

As per the chart documentation, to get the db password:
```
PGPASSWORD=$(kubectl get secret --namespace default anchore-postgres-db-postgresql -o jsonpath="{.data.postgres-password}" | base64 --decode; echo)`
```


Anchore Engine
--------------

* Configuration of Anchore Engine: use a ConfigMap for the config.yaml and expose it as a volume in the deployment.
```
kubectl create configmap anchore-engine-config --from-file=./config.yaml
```

* Secrets: db user, db password, db host, admin password.
Set your various passwords as b64 encoded strings and add them as secrets. These should be the admin password, and db user and password at minimum.
I've provided 2 files to split the db and anchore user secrets, but a single file would be fine, just be sure to update the anchore-engine-deployment.yaml to reflect the
proper name and keys if you merge them.

You'll need ot add your secrets to the yamls in base64 encoding, and be mindful of any included newlines or quotes as those will cause errors in the usage of the creds.

```
kubectl create -f anchore-engine-users-secrets.yaml
kubectl create -f anchore-engine-db-secrets.yaml
```

* Volumes: The anchore-engine container needs two volumes: /var/run/docker.sock and /config/config.yaml. Recommended to save config.yaml as a configmap.
* Deployment: Anchore-Engine, a single pod with a single image (docker.io/anchore/anchore-engine:latest).
```
kubectl create -f anchore-engine-deployment.yaml
```

* Service: Anchore-Engine
```
kubectl create -f anchore-engine-service.yaml
```

Or you can alternatively use the `kubectl expose` command depending on your specific configuration of k8s.




