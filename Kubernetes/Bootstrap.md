## What is openshift Bootstrap?
This is a machine that holds resources for the creation of master nodes.



## Bootstrapping process step by step:
1. Bootstrap machine boots and starts hosting the remote resources required for master machines to boot. Runs one instance of etcd
2. Master machines fetch the remote resources from the bootstrap machine and finish booting.
3. Master machines use the bootstrap node to scale the etcd cluster to 3 instances.
4. The Etcd operator scales itself down off the bootstrap node, then scales back up to 3; all on the Masters
5. Bootstrap node starts a temporary Kubernetes control plane using the newly-created etcd cluster.
6. Temporary control plane schedules the production control plane to the master machines.
7. Temporary control plane shuts down, yielding to the production control plane.
8. Bootstrap node injects OpenShift-specific components into the newly formed control plane.
9. Installer then tears down the bootstrap node or if user-provisioned, this needs to be performed by the administrator.
10. Worker machines fetch remote resources from masters and finish booting. 

<img width="836" alt="image" src="https://user-images.githubusercontent.com/100561043/167831490-512d986f-5304-4ed8-817a-e0af6717cade.png">


## One Touch provisioning via Ignition

Ignition files are templates that holds the 'how' a machine should look like and what it needs to start with.

Ignition applies a declarative node configuration early in the boot process.  Unifies kickstart and cloud-init

Generated via openshift-install
Configures storage, systemd units, users, & remote configs

Executed in the initramfs
Configuration for masters & workers is served from the control plane and sourced from Machine Configs
