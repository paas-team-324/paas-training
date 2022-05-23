# OPENSHIFT CONTAINER PLATFORM

<img width="911" alt="image" src="https://user-images.githubusercontent.com/100561043/167823305-e9cfbad9-1913-44e1-9003-b8f0bf07b4b7.png">


<img width="941" alt="image" src="https://user-images.githubusercontent.com/100561043/167823452-ca4f156e-1b11-42c8-b937-e69cd560568f.png">


Enterprise organizations need to manage across a multicluster and multicloud environment.
Red Hat provides solutions to address these challenges :)

Can I monitor multiple clusters & drive updates? 
Advanced Cluster Management allows you to monitor all your clusters, update cluster configurations and drive upgrades 

Can I deploy apps to multiple clusters?
(Advanced Cluster Management lets you deploy apps to multiple clusters from a single interface

Are my images free from vulnerabilities?
Advanced Cluster Security integrates with Clair to provide image scanning and vulnerability detection

Are all my clusters compliant with our security policies? 
Advanced Cluster Security allows you to monitor cluster security policies like CIS, PCI, HIPAA and more

How can I store images for connected & disconnected users? 
Red Hat Quay provides a global registry to manage images across all your environments

Can I integrate security into our dev process? 
ACS & Quay integrate with OpenShift to let you shift security left into your dev process and enable DevSecOps


## Openshift environment

<img width="761" alt="image" src="https://user-images.githubusercontent.com/100561043/167823917-b55fea80-e406-4a64-af2a-b00d34be20d3.png">

There are two types of hosts in an OpenShift environment. 
`Workers` are where user workload and various OpenShift components will land. 
`Workers` can be easily scaled and, on cloud providers, even scaled automatically based on cluster capacity.

The other type of host is the `Master`. 
OpenShift uses **3** `masters` for __high availability__ and cluster quorum. User workload **does not** run on the masters.

`etcd` is used to keep track of the state of everything in the cluster, from which users are logged in to where workload lives and more.
OpenShift is built on Kubernetes, and its core components are still there and directly accessible via Kubernetes’ APIs.

- OpenShift brings its own web console with special features for both administrators and developers. 
- OpenShift makes its features available through its own API endpoints which follow the same standard as other Kubernetes APIs using Custom Resource Definitions. 
- OpenShift also brings a full lifecycle management solution that’s deeply integrated which allows for seamless and automatic cluster upgrades from within the cluster itself.
- OpenShift includes a number of internal and support infrastructure services that make containers easier to use at scale And, because these services all run in pods as part of the platform, they can be orchestrated like any other workload and made to run across all hosts in the environment.
- OpenShift also includes a log aggregation solution based on Fluentd, Elasticsearch, and Kibana. This integrated solution makes it easy to visualize and corroborate log events for applications that are scaled to many instances, and, it, too, is tied into OpenShift’s role-based access control (RBAC) ensuring that only the right people see the logs they are supposed to.
- OpenShift extends Kubernetes ingress capabilities with an integrated router that bridges traffic from outside the cluster into the software defined network (SDN). This routing solution makes it easy for workload to be exposed and made accessible to consumers that are not inside the cluster.

**OpenShift runs on a variety of infrastructure platforms, from bare metal to private cloud to public cloud. **

There are two types of hosts in an OpenShift environment. 
Workers are where user workload and various OpenShift components will land. 
Workers can be easily scaled and, on cloud providers, even scaled automatically based on cluster capacity.

The other type of host is the master. OpenShift uses 3 masters for high availability and cluster quorum. User workload does not run on the masters.



**Because all these services run in pods as part of the platform, they can be orchestrated like any other workload and made to run across all hosts in the environment.**

- OpenShift includes an integrated container image registry.
This registry is tied into OpenShift’s own role-based access control, providing multi-tenancy benefits. 
Images that are built inside the platform using OpenShift’s native build features automatically land in this image registry. Being a fully OCI-compliant registry, images created outside of the environment can also be placed into the integrated registry as part of a larger software devleopment lifecycle (SDLC) process.

- OpenShift includes a cluster monitoring solution informed by the best practices of Red Hat’s own site reliability engineers (SREs). This preconfigured stack, based on Prometheus, Grafana, and Alertmanager helps platform administrators understand the health and capacity of the OpenShift cluster.

- OpenShift also includes a log aggregation solution based on Fluentd, Elasticsearch, and Kibana. This integrated solution makes it easy to visualize and corroborate log events for applications that are scaled to many instances, and, it, too, is tied into OpenShift’s role-based access control (RBAC) ensuring that only the right people see the logs they are supposed to.

- OpenShift extends Kubernetes ingress capabilities with an integrated router that bridges traffic from outside the cluster into the software defined network (SDN). This routing solution makes it easy for workload to be exposed and made accessible to consumers that are not inside the cluster.

Regardless of whether a consumer is an administrator or a developer, a rich set of web, CLI, and IDE integrations, all made possible by the Kubernetes and OpenShift APIs, means that people can work in the way they are most comfortable.


<img width="730" alt="image" src="https://user-images.githubusercontent.com/100561043/167830110-bbd3e258-4458-4907-8b57-acfd16145835.png">


