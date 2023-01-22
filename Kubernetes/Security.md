# Basic Linux Security Concepts

### Files & Dir premissions 

The basic:
 - **r** read - you may view the contents of the file
 - **w** write - you may change the contents of the file
 - **x** execute - you may execute or run the file if it is a program or script

For every file we define 3 sets of people for whom we may specify permissions.
- u: owner - a single person who owns the file. (typically the person who created the file but ownership may be granted to some one else by certain users)
- g: group - every file belongs to a single group.
- o: others - everyone else who is not in the group or the owner.

To view permissioms to file type:
``` ll  <path-to-file> ```
and watch at the start of the line:

<img width="100" alt="image" src="https://user-images.githubusercontent.com/100561043/169302574-35defe09-5cfb-44db-b32c-8ce2d24c3b27.png">

__Note that the order of permissions is always read, then write then execute__

- The first character identifies the file type. If it is a dash **( - )** then it is a normal file. If it is a **d** then it is a directory.

- The following 3 characters represent the permissions for the owner. A letter represents the presence of a permission and a dash ( - ) represents the absence of a permission.

- The following 3 characters represent the permissions for the group. In this example the group has the ability to read but not write or execute. 

- Finally the last 3 characters represent the permissions for others (or everyone else)

#### chmod
You can change a file (or a dir) permissions with ```chmod```:

``` chmod [permissions] [path] ```
- Who are we changing the permission for? [ugoa] - user (or owner), group, others, all
- Are we granting or revoking the permission - indicated with either a plus ( + ) or minus ( - )
- Which permission are we setting? - read ( r ), write ( w ) or execute ( x )


The method outlined above is not too hard for setting permissions but it can be a little tedious if we have a specific set of permissions we would like to apply regularly to certain files. Luckily, there is a shorthand way to specify permissions that makes this easy.

To understand how this shorthand method works we first need a little background in number systems. Our typical number system is decimal. It is a base 10 number system and as such has 10 symbols (0 - 9) used. Another number system is octal which is base 8 (0-7). Now it just so happens that with 3 permissions and each being on or off, we have 8 possible combinations (2^3). Now we can also represent our numbers using binary which only has 2 symbols (0 and 1). The mapping of octal to binary is in the table below.

|Octal|	Binary|
| -- | -- |
| 0	| 0 0 0 | 
| 1	| 0 0 1 |
| 2	| 0 1 0 |
| 3	| 0 1 1 |
| 4	| 1 0 0 |
| 5	| 1 0 1 |
| 6	| 1 1 0 |
| 7	| 1 1 1 |

so inorder to change permissions you can run:
```
chmod <number-for-owner><nubmer-for-group><number-for-others> [path]

```
like: ``` chmod 751 file.txt ```



### SELinux
- Everything in the operating system has a label
- Policy defines the interaction between a labeled process and labeled resources
- Policy comes with the distribution but you can add your own
- Policy is enforced by Kernel
- Enforcement is turned on by default
- Systems with SELinux enabled are less susceptible (e.g. container breakouts)
- Namespaces - isolation primitives

to drill down you can:
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/pdf/using_selinux/red_hat_enterprise_linux-8-using_selinux-en-us.pdf



## Certificates and Certificate Management

<img width="145" alt="image" src="https://user-images.githubusercontent.com/100561043/167843159-885ce328-8426-4880-9064-4fcf90a919d2.png">

- OpenShift provides its own internal CA
- __Certificates__ are used to provide secure connections to:
  - master (APIs) and nodes
  - Ingress controller and registry
  - etcd
- Certificate rotation is automated
- Optionally configure external endpoints to use custom certificates

## Service Certificates

<img width="851" alt="image" src="https://user-images.githubusercontent.com/100561043/167843368-74de0fd0-3453-41c3-82f9-62cee5c5c5b2.png">

Users create a `service` and then annotate with service.alpha.openshift.io/serving-cert-my -- the service serving cert signer then creates a keypair and puts it into a secret

Users create a ConfigMap and then annotate it with service.beta.openshift.io/inject-cabundle="true" -- the configmap cabundle injector then adds the CA bundle to the configmap

Users define pods, deployments, etc. and mount the secret and configmap as needed so that their service now has full TLS provided by the built-in CA, and other services that add the CA bundle can consume this service and trust its certificates
Note that this overlaps with Service Mesh’s ability to provide transparent mutual TLS -- use one or the other, not both.

## The Service CA Operator

- The Service Certificate Authority (CA) can sign server certificates for services running in the OpenShift cluster. 
- The service-ca operator manages 3 controllers
  - service-ca controller
  - configmap-cabundle-injector controller
  - apiservice-cabundle-injector controller
 
 
## Identity and Access Management

<img width="682" alt="image" src="https://user-images.githubusercontent.com/100561043/167845064-56a3abda-d49c-4bb1-af8c-06d42ca335cf.png">

OpenShift includes an OAuth server, which does three things: 
- Identifies the person requesting a token, using a configured identity provider
- Determines a mapping from that identity to an OpenShift user
- Issues an OAuth access token which authenticates that user to the API 


### Configuring an Identity provider

- Determines a mapping from that identity to an OpenShift user
  - Allows multiple identities to map to the same OpenShift user
  - Allows deconflicting between identity provider roles
- In many cases the credentials are validated against the identity provider directly (not necessarily given to the OAuth server)


## Fine-Grained RBAC

- Project scope & cluster scope available
- Matches request attributes (verb,object,etc)
- If no roles match, request is 
- denied ( deny by default )
- Operator- and user-level 
- roles are defined by default
- Custom roles are supported

In OpenShift we have a role based authorization component. 
There is a Project scope and a Cluster scope where either `RoleBindings` or `ClusterRolebindings` define the permissions a user will have once logged in. This role component matches attributes on the request like the verb on the objects for example  get projects or create pods. 
It matches those attributes to roles which have been grated to the requesting user. 
If no roles match the request is denied. 
By default everything is denied unless the role specifically allows it. Out of the box operator and user level roles are defined and custom roles are also supported. 	

 

## SCC - OpenShift Security Context Constraints

Similar to the way that RBAC resources control user access, administrators can use security context constraints (SCCs) to control permissions for pods. These permissions include actions that a pod can perform and what resources it can access. You can use SCCs to define a set of conditions that a pod must run with to be accepted into the system.

Security context constraints allow an administrator to control:

- Whether a pod can run privileged containers with the allowPrivilegedContainer flag.
- Whether a pod is constrained with the allowPrivilegeEscalation flag.
- The capabilities that a container can request
- The use of host directories as volumes
- The SELinux context of the container
- The container user ID
- The use of host namespaces and networking
- The allocation of an FSGroup that owns the pod volumes
- The configuration of allowable supplemental groups
- Whether a container requires write access to its root file system
- The usage of volume types
- The configuration of allowable seccomp profiles

plaese review to learn more about SCC:
https://docs.openshift.com/container-platform/4.6/authentication/managing-security-context-constraints.html


To view the existing SCC: 
```bash
$ oc get scc
$ oc describe scc restricted
$ oc describe scc privileged
```

 
