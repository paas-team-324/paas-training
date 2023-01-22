# Lets Learn Basic Concepts

## Image & Container

A `container` is created by a static build called `Image` that holds the binaries:

<img width="344" alt="image" src="https://user-images.githubusercontent.com/100561043/167797354-5c04ecd1-f915-4c09-a3c0-0388aacd8bdb.png">

By using the [Podman](https://podman.io/) utility tool, we can build, run and manage images and containers.

With podman you can:

1. Pull Remote Images & Store localy & View 
2. Login to Remote Registries (Images' "store")
3. Run Containers from images & View all containers
4. Open a shell into the container
* And much more

-- insert the podman small guide here --


On Openshift platform images are saved on a `Registry`:
A `registry` is an `hub` of images, and it can be localy in the cluster or outside the cluster, like `Quey` or `DockerHub`

https://hub.docker.com/
https://quay.io/

At the `registry` the images are saved with `tags` that represent the `version` of the binary.

<img width="600" alt="image" src="https://user-images.githubusercontent.com/100561043/167798887-27571b2b-ebe3-4a30-b74e-94018e615f29.png">

## POD

At openshit The smallest compute unit that can be defined, deployed, and managed is called - **`Pod`**
A `pod` wraps a `container` and provide:
- CPU
- Memory
- Network
- Storage

You can view the pods on the cluster with this command:
``` oc get pods --all-namespaces  ```

To view a specipic pod and get see yaml:

```bash
$ oc describe pod <pod-name> -n <pod-namespace> 

$ oc get pod <pod-name> -n <pod namespace> -o yaml > <file-name>.txt
```
---

## Namespace
projects isolate apps across environments, teams, groups and departments via a concept named `Namespace`
This is a logical seperation, and each namespace has its own `Quata`, `limits` and resources.

<img width="599" alt="image" src="https://user-images.githubusercontent.com/100561043/167841550-9c88140b-bba3-4099-bb26-3b443d06b206.png">

---

## ReplicaSet V.S ReplicationController

Both are used to manage and ensure a specified number of pods are running at any given time at the cluster.
But `ReplicationController` does not support set-based selector requirements, and `ReplicaSet` do.

```
 oc get replicaset --all-namespaces
```
```
 oc get ReplicationController --all-namespaces
```

## Deployments

are responsible to define how to roll out new versions of Pods

``` oc get deploy --all-namespaces```

## Deployments V.S DeploymentConfig

| Deployments | DeploymentConfig |
| :---: | :---: |
| Deployments is driven by a controller manager | Use deployer pods for every new rollout |
| Deployments take availability over consistency The controller manager runs in high availability mode on masters and uses leader election algorithms to value availability over consistency | DeploymentConfigs prefer consistency If a node running a deployer pod goes down, it will not get replaced. The process waits until the node comes back online or is manually deleted  |
| Has an implicit ConfigChange trigger.  Every change in the deployment automatically triggers a new rollout | ConfigChange triggers are explicit |
| Do not yet support any lifecycle hooks | Support lifecycle hooks (pre, mid, post) |
| Rollback to the last ReplicatSet is not supported. | Support automatically rolling back to the last successfully deployed ReplicationController |


## daemonset
ensures that all (or some) nodes run a copy of a pod, it's a process that runs on every node.

## configmaps
allow you to decouple configuration artifacts from image content
```oc get configmaps```

<img width="749" alt="image" src="https://user-images.githubusercontent.com/100561043/167813905-11500275-6943-461e-881c-6e138a68915f.png">

## secrets
provide a mechanism to hold sensitive information such as passwords
```oc get secrets```


## Service and Route

```oc get svc --all-namespaces```
``` oc get routes --all-namespaces ```

`Service` provide internal load-balancing and service discovery across pods, it is the way of communicate internally between components.
`Route` make services accessible to clients outside the environment via real-world urls

<img width="815" alt="image" src="https://user-images.githubusercontent.com/100561043/167814698-eb229ff9-8238-4327-aaf3-14adcf7454f4.png">

`Route` make services accessible to clients outside the environment via real-world urls
`Pods` running on OpenShift don’t need to go through the routing layer and can interact with each other directly through the `services`.
After the `router` discovers the `Pod` endpoints via the `service`, it sends the `Pod` traffic directly to those endpoints and bypasses the `service` layer.
DNS resolution for a host name is handled separately from routing. Admin may have configured a DNS wildcard entry that will resolve to the node that is running the OpenShift Container Platform router

## Persistent Volume Claims

<img width="552" alt="image" src="https://user-images.githubusercontent.com/100561043/167820726-434cd2cf-1572-4d30-ae34-28e2ff0b74f7.png">


```
oc get pvc --all-namespaces
```

OpenShift and Kubernetes can handle stateful applications that require storage, too. Users create claims for storage, and OpenShift connects those claims to real storage volumes. When applications are deployed, OpenShift automatically connects the real storage into the container as specified by the end user. And, as applications move around in the cluster, the storage automatically follows them. Many different storage types are supported, from raw to block to file, both read-write-once and read-write-many.

## Liveness and Readiness

OpenShift includes two types of probes for evaluating the status of applications. 
A liveness probe examines whether an application is functioning properly and, if not, causes it to be restarted. 

A readiness probe examines whether an application is ready to receive traffic and, if not, causes it to be removed from the service endpoint list. 

Probes are powerful ways to increase the reliability and availability of applications in OpenShift.

