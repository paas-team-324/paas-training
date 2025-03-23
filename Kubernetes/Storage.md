# PersistentVolume & Claim

## first lets see some storage that openshift support
|   |   |   |   |   |
| --- | --- | --- | --- | --- |
| nfs | OpenStack Cinder | iSCSI | Azure Disk | AWS EBS |
| Raw Block | Local Volumes | Fiber Channel | Azure File | GCE Persistent Disk |
| VMware vSphere VMDK | FlexVolume | Container Storage Interface (CSI) | S3 | ODF | 


- OpenShift Container Platform leverages the Kubernetes `persistent volume (PV)` framework to allow administrators to provision persistent storage for a cluster. Using `persistent volume claims (PVCs)`, developers can request `PV` resources without having specific knowledge of the underlying storage infrastructure.

- OpenShift Container Platform supports a growing list of storage plugins for using various storage solutions.
- Note that high-availability of storage in the infrastructure is left to the underlying storage provider.
- Shipped and supported by NetApp via TSANet

## So how it works?

A `PersistentVolumeClaim (PVC)` is bound to a `PersistentVolume (PV)`
and a `PersistentVolume` maps to real-world storage.

A user defines a workload that instructs OpenShift to attach the `PVC’s` storage to a mount point inside the container
When the kubelet is told by the master to run the pod, it first tells the host to mount the real-world storage into the OS securely for the container
Then kubelet launches the containers and tells CRI-O to attach the OS mountpoint as a volume inside the container

``` oc get pv --all-namespaces ```
``` oc get pvc --all-namespaces ```

## Static Storage Provisioning

Administrators can define a pool of `Persistent Volumes (PV)` which are backed by network storage solutions like NFS, iSCSO, AWS EBS, etc and make them globally available in the OpenShift cluster.

Users within their projects can create a `Persistent Volume Claim (PVC)` in order to request a PV to be available within their pods. 
In the pod definition, a developer can refer to the PVC and mount the requested persistent volume inside the pod at an arbitrary path. 
If a pod gets restarted, OpenShift mounts the same persistent volume into the pod again so that the pod data is available. 
PVs outlive the containers that were using them.

<img width="717" alt="image" src="https://user-images.githubusercontent.com/100561043/167838015-baaaba1a-d2f9-461d-bede-06bddea6a795.png">


## Dynamic Storage Provisioning

- Dynamic provisioning allows provisioning persistent volumes on-demand when users request it rather than admins predefining them in advance
- `StorageClass` is a blueprint of how to provision persistent volumes on a network storage. OpenShift provides a set of provisioners that determine what volume plugins should be used for provisioning the volumes. OpenShift also supports third-party plugins that are not part of Kubernetes, such as NetApp Trident
- Admins creates a catalog of StorageClasses available in the OpenShift cluster. StorageClass names are arbitrary names to communicate their characteristics
- Users can create a Persistent Volume Claim and specify the name of a StorageClass to instruct OpenShift on the type of persistent volume that should be provisioned for the them

<img width="828" alt="image" src="https://user-images.githubusercontent.com/100561043/167840230-5e029909-c0ea-491e-be1e-014681c523c9.png">



# CSI

## CSI Driver Paradigm
- CSI drivers and logic are provided by storage vendors
  - Each implementation may be different based on the vendor
- Controller logic is deployed to the OpenShift cluster as an Operator, deployment, or even a standalone Pod(s)
  - Responsible for interfacing with storage device to create and manage volumes, snapshots, clones, etc
  - Respond to events (create, delete PVC) for assigned StorageClass(es)
  - Sidecars assist with hooks for additional functionality - snapshots, resizing, etc
- Each node hosts, via a DaemonSet, one or more CSI node plugin Pods for the driver
  - Kubelet requests the node plugin to mount/unmount volumes, format block devices if needed, etc 
- `StorageClass` is a blueprint of how to provision persistent volumes on a network storage. OpenShift provides a set of provisioners that determine what volume plugins should be used for provisioning the volumes. OpenShift also supports third-party plugins that are not part of Kubernetes, such as NetApp Trident



<img width="535" alt="image" src="https://user-images.githubusercontent.com/100561043/167840738-9da28807-a469-4782-9132-c247906faf2d.png">
