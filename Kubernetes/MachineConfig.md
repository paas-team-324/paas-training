## Machine Config Operator

The Machine Config Operator provides cluster-level configuration, enables rolling upgrades, and prevents drift between new and existing nodes. The MCO is the heart of what makes RHCOS a kube-native operating system

- OS configuration is stored and applied across the cluster via the Machine Config Operator.
- Subset of ignition modules applicable post provisioning
  - SSH keys
  - Files
  - systemd units
  - kernel arguments
- Standard k8s YAML/JSON manifests
- Desired state of nodes is checked/fixed regularly
- Can be paused to suspend operations

The Machine Config Operator is responsible for deployment of three primary operands
- the Machine Config Controller (MCC)
- the Machine Config Server (MCS) 
- the Machine Config Daemon (MCD)



### MachineConfig

` MachineConfig ` are rendered into a single config that targets a MachineConfigPool, or a group of systems. 
This is the job of the Machine Config Controller.
- Default pools are: master & worker
- Custom MachineConfigPools can be created as needed
- Custom MachineConfigPools use inheritance to help scale

### MachineConfigPool

A `MachineConfigPool` has two things it targets 
- relevant nodes - relevant MachineConfig objects.

The `MachineConfigPool` has a nodeselector which determines which nodes the pool applies to.
The `MachineConfigPool` has a `machineConfigSelector` which determines which `MachineConfigs` belong / are relevant.


## Custom Machine Config Pools

Since all nodes must be workers, custom pools inherit from or override any Machine Configs that apply to workers. 
A **custom pool would be defined to target specific node labels** and would define which MachineConfig objects need to be considered. 
When the Machine Config Pool is rendered by the MCC, it evaluates the relevant MachineConfig objects’ names, and compares the strings in alphanumeric/lexical order. 
It then renders out the files section of the machineconfig in order. 
The ignition process (on first boot) and the Machine Config Daemon (when it is applying changes) will apply the files in the order presented, overwriting an existing file if there is a difference.

**Note**: Nodes can only have one custom MachineConfigPool that applies to them.


## Machine Config Server

Ignition configs will source the corresponding Machine Config Pool during the provisioning of RHEL CoreOS. 
In this way, when the node is provisioned, it has the correct configuration from the very beginning. 
The Machine Config Server runs as a daemonset on masters and is attached to the host network. 
It listens on the specific port that the Ignition process is instructed to reach out to when requesting ignition files

<img width="682" alt="image" src="https://user-images.githubusercontent.com/100561043/167851262-e5b09d3a-4ab9-475a-8936-c1bc553ad2ad.png">


## Machine Config Daemon

The `Machine Config Operator (MCO)` ensures that a `Machine Config Daemon (MCD)` runs on every node. 
The MCD pulls the rendered MachineConfig object for the MachineConfigPool that matches the node, and then ensures that the defined files match the desired state and contents. 
If changes are made to the MachineConfigs, the MCD will update the targeted files and configurations to match the desired state. In this way, the MCD prevents configuration drift.

<img width="269" alt="image" src="https://user-images.githubusercontent.com/100561043/167851330-c1efaa36-c539-4fa3-b9e0-46c65e9e2bbd.png">



The MCO coordinates with the MCD to perform the following actions, in a rolling manner, when OS updates and/or configuration changes are applied:
- Cordon / uncordons nodes
- Drain pods 
- Stage node changes
  - OS upgrade
  - config changes
  - systemd units
- Reboot
