## First thing first - Routes vs Ingress

<img width="778" alt="image" src="https://user-images.githubusercontent.com/100561043/167852263-01cb1f44-b3d2-41bb-a81a-d09b3b039f77.png">

## Routing and Load Balancing

- Pluggable routing architecture
 - HAProxy Router
 - F5 Router
- Multiple-routers with traffic sharding
- Router supported protocols
 - HTTP/HTTPS
 - WebSockets
 - TLS with SNI
- Non-standard ports via cloud load-balancers, external IP, and NodePort

### Router-based deployment methodologies

<img width="425" alt="image" src="https://user-images.githubusercontent.com/100561043/167852696-efdf90f7-6d2d-4c2b-8b8d-edb25318a9d5.png">

Split Traffic Between Multiple Services For A/B Testing, Blue/Green and Canary Deployments

## NodePort

- NodePort allow exposing non-standard ports for a service on all nodes in the cluster
- If the node receiving the traffic doesn't have the target service running, the traffic is redirected to a node which has the pod running
- While it is easier to configure than external IPs and Ingress (prev slide), it is a wasteful use of scarce port resources and it requires more privileges.

<img width="261" alt="image" src="https://user-images.githubusercontent.com/100561043/167853648-7ba305d1-6729-404b-b4bc-05a10f6b6c9e.png">

- `NodePort` binds a service to a unique port on all the nodes
- Traffic received on any node redirects to a node with the running service
- Ports in `30K-60K` range which usually differs from the service
- Firewall rules must allow traffic to all nodes on the specific port



## External IP

<img width="261" alt="image" src="https://user-images.githubusercontent.com/100561043/167854526-baa609cd-0d1f-40f8-970f-afc8d21f96ba.png">

- `External IP` allows assigning an external IP to a service so that client outside of the OpenShift cluster can connect to services inside the Openshift cluster (e.g. database, JMS broker) on that port. 
This is particularly useful for services that are running on ports other than 80/433, otherwise router is a more suitable alternative. 
It is also useful when the client needs to know a specific IP for the service in order to open firewalls or while-list it in their own system.

- Each external IP address is guaranteed to be assigned to a maximum of one service. 
  This ensures that each service can simply expose its chosen ports regardless of the ports exposed by other services.

- The external IP address itself is not managed by the underlying Kubernetes infrastructure and must be maintained and provided by a cluster administrator. Managing external IPs is not convenient since application users cannot create external IP addresses themselves, and must work with cluster administrators to allocate an address for their usage. 
There is also no protections in place to restrict the usage of a particular external address configured within the cluster. 

- `Ingress IP self-service` is a functionality within OpenShift to streamline the allocation of External IP’s for accessing to services in the cluster. Cluster administrators can designate a range of addresses using a CIDR notation which allows an application user to make a request against the cluster for an external IP address. 
When a service is configured with the type LoadBalancer, an External IP address will be automatically assigned from the designated range.

- Ingress is supported for non-cloud clusters. 
  For cloud (GCE, AWS, and OpenStack) clusters, cloud provider’s load balancer service can be used to automatically deploy a load balancer to target the service’s endpoints.

- The IP failover pods are responsible for providing the external IP range (Virtual IPs) and would run on one or more of the nodes (more than one for high availability) in the cluster or they can just run on the HAProxy (router) nodes.

- The OpenShift cluster administrator must ensure that the external IP address pool terminates at one or more nodes in the cluster.



## egress traffic 

### Controlling egress traffic - default kubernetes behavior

Pod traffic by default comes from the IP of the node that the pod is running on and therefore the external service needs to whitelist the IP of all nodes that a certain application might be scheduled on.

**Advantage**: simple to configure as not special configuration is needed on OpenShift other than making sure (via labeling) pods which need to access the external service are scheduled on the node that is whitelisted
**Disadvantage**: whitelisting is not very granular and any pod scheduled on the whitelisted node can access the external service

<img width="396" alt="image" src="https://user-images.githubusercontent.com/100561043/167855349-e4983de3-2356-47ff-bf9f-b0979810c5dd.png">


### Controlling egress traffic - Egress router

- Egress router runs a service that redirects (proxies) traffic to a specified remote server, using a private source IP address that is not used for anything else. 
The service allows pods to talk to servers that are setup to only allow access from whitelisted IP addresses. When deploying an egress router, an IP address from the physical network that the node itself is on is reserved by the cluster administrator to be used by the egress router.

- Application pods access the external service using the egress service on the service internal IP address or service name. 
The egress service redirects the traffic to the egress routers which in turn send the traffic to the external service with the egress IP (reserved on the node it’s running on) as the source IP. The external service accepts the traffic since the source IP (egress router IPs) is whitelisted.

- Multiple egress IPs (egress pods) can be configured (one on each node) so that if cluster detects that one egress IP has stopped working (node failure), it would fail over and use another egress IP on the other nodes that are preconfigured.

<img width="516" alt="image" src="https://user-images.githubusercontent.com/100561043/167856168-fb560a36-775f-49df-86bf-a16df1f28617.png">

### Controlling egress traffic - Egress IP high availability (multiple IPs)

`Egress IPs`, as opposed to egress router which is deployed as a pod, are OpenShift construct and are configured on the node via modifying the node object e.g. via:
```
oc patch hostsubnet node2 -p '{"egressIPs": ["IP 4"]}'
oc patch hostsubnet node3 -p '{"egressIPs": ["IP 5"]}'
```

The egress IPs have nothing to do with the IPs normally assigned to the host interfaces.

<img width="461" alt="image" src="https://user-images.githubusercontent.com/100561043/167856618-6f4e991f-06d2-4387-89e8-25b89a1d76b4.png">

Project admins can then assign one or multiple egress IPs (for failover) to a project, which would force all traffic from the pods in that project (regardless of which nodes they are running on) to use the primary egress IP as the source IP. Note that in this case, the OSV flows will be set up automatically to redirect any traffic from the project to the node that has the primary egress IP (Node 2 ) and then send to the destination from there. 



<img width="461" alt="image" src="https://user-images.githubusercontent.com/100561043/167857295-c3c4a9fe-d5ad-454e-a109-a514140affb2.png">


In case of failure (node crashes, etc) of the primary egress IP (IP 4), OSV will automatically configure the traffic flows so that traffic from all pods in the project go through the node that owns the secondary egress IP (Node 3) and use that egress IP (IP 5) as the source IP of all traffic. 

The current HA Egress source IP feature requires that multiple egress IP addresses be whitelisted on the service endpoint and/or edge router, an often valuable commodity in the data center.


We are targeting an optional variation of the functionality which enables the ability to use a single IP that can be migrated from its current, failed node to a healthy node (“fully automatic”).  While a single HA IP is more efficient use of the address space and easier to manage, additional steps are required in the IP-moving solution that would take several (valuable) seconds to complete, and may not be suitable for delay-sensitive customers: 
the master must update a HostSubnet object with the new IP assignments
the nodes would have to observe that change and react to it
For this reason, we’ll eventually make both options available as a choice.

## NetworkPolicy

`NetworkPolicy` allows project administrators to configure granular isolation rules using NetworkPolicy objects.
A network policy is a specification of how groups of pods (using labels) are allowed to communicate with each other and other network endpoints

<img width="266" alt="image" src="https://user-images.githubusercontent.com/100561043/167860735-4a871031-09e7-4a16-a40d-a71fd2b2c579.png">


## OpenShift SDN

OpenShift 4 clusters are using OpenShift SDN NetworkPolicy as the default networking solution. The default behavior is wide-open network across projects/namespaces (no policies applied). It is possible to install the cluster in a multitenant mode as an install-time configuration option. In this scenario, all projects/namespaces are network isolated from each other by default, but all can be accessed by the router and can access the cluster registry and other internal services. When installed in this mode, the SDN operator configures the cluster’s project request template to ensure that all new projects/namespaces have isolating policy objects.

**OPEN NETWORK  (Default)**
- All pods can communicate with each other across projects

**MULTI-TENANT NETWORK**
- Project-level network isolation
- Multicast support
- Egress network policies

### SDN configuration 

__Networking Advanced Settings__

These are the OpenShift SDN settings that can be tweaked at install-time:
- Mode: NetworkPolicy (default) , Multitenant, Subnet
-VXLAN Port Number
-MTU (autodetected, once)
-External OpenVSwitch

NOTE: Most network settings cannot be changed safely and affect the entire cluster.  The operator will prevent unsafe changes.   If you need to force a change to a non-production cluster, see the operator README for the command, but a cluster re-install is likely to be the better choice.


### OpenShift SDN high-level architecture

<img width="472" alt="image" src="https://user-images.githubusercontent.com/100561043/167860351-7dcc5112-a9e5-4064-8fce-33abbb34650b.png">

- Pods are connected via an overlay network which enables IP-per-Pod networking and direct pod-to-pod communication
- The Pod IPs are independent of the physical network the OpenShift Nodes are connected to

### OpenShift SDN packet flows container-container across hosts

<img width="444" alt="image" src="https://user-images.githubusercontent.com/100561043/167861048-89e77439-293a-4951-b82d-2950d2f16f88.png">


### OpenShift SDN packet flows container leaving the host

<img width="511" alt="image" src="https://user-images.githubusercontent.com/100561043/167861150-27dfe4fb-baf3-47c7-bc43-6bd926107172.png">
