### CLI - Prerequisits
```bash
git clone <URL TO THIS REPO>
cd Openshift/Training
export USER=<YOUR NAME>
oc new-project $USER-test
oc describe ns $USER-test | grep -E 'sa|Annotations'
oc new-app openshift/hello-openshift --allow-missing-imagestream-tags
oc get all
oc get pods -w
CTRL+C
oc get all
oc get console cluster -o yaml
oc get console cluster -o json | jq '.status.consoleURL' | tr -d '"'

export OCP_SUB_DOMAIN=<OCP_SUB_DOMAIN>
```
# Deployment, Service, Pod, Route
### In WebUI - With Instractor
1. Administrator vs. Developer Prespective
2. Developer Prespective: Topology in specific Project

    i. Deployment, Service, Pods

    ii. Pod Logs

    iii. Events in UI
    
    iv. Volumes mounted to pod

    v. Terminal into Pod

    Commands to Run in pod:
    ```bash
    bash         # Move to more comfortable 
    ps aux       # Running Script of image?
    ls 
    whoami       # Run as ServiceAccount that ran the pod
    curl         # To check if a binary exists in image
    ```



### CLI - ServiceAccounts - WTF? - TODO With Instractor
```bash
oc get sa
oc get pod -o name | xargs -n 1 -I NAME oc get NAME -o json | jq '"IMAGE: " + .spec.containers[].image, "SERVICEACCOUNT: " + .spec.serviceAccount'
#oc get pod -o name | xargs -n 1 -I NAME oc get NAME -o json | jq '.spec.volumes[].projected.sources[].serviceAccountToken'
oc get pod -o name | xargs -n 1 -I NAME oc get NAME -o json | jq '.spec.containers[].volumeMounts'
oc rsh $(oc get pods -o name)
bash
ls /var/run/secrets/kubernetes.io/serviceaccount
cat /var/run/secrets/kubernetes.io/serviceaccount/token ; echo
exit
exit
oc get sa default -o json | grep token | awk -F':' '{print $2}' | tr -d "[:space:]" | tr -d '"'
oc get secret $(!!) -o name
oc extract $(!!) --to=-
```

### In WebUI
1. Go to Administrator View
2. Networking -> Services
3. Networking -> Route -> Create Route
* Name: $USER-route
* Hostname: $USER.apps.$OCP_SUB_DOMAIN
> apps.$OCP_SUB_DOMAIN <=> https://console-openshift-console.apps.$OCP_SUB_DOMAIN
* Path: "/"
* Service: "hello-openshift"
* Target Port: 8080
4. Access the URL
5. Delete Route
6. 
```bash
cat > route.yaml << EOF
kind: Route
apiVersion: route.openshift.io/v1
metadata:
  name: test
  namespace: $USER-test
spec:
  host: $USER.apps.$OCP_SUB_DOMAIN
  to:
    kind: Service
    name: hello-openshift
    weight: 100
  port:
    targetPort: 8080-tcp
EOF

oc apply -f route.yaml

curl $USER.apps.$OCP_SUB_DOMAIN

oc delete -f route.yaml

```
7. 
```bash
oc create route --help
oc create route edge --help
oc create route edge my-route --service=hello-openshift
oc get route my-route
oc get route my-route | awk '{print $2}' | grep apps
URL=$(echo "https://"`oc get route my-route | awk '{print $2}' | grep apps`)
echo $URL
curl $URL
oc delete route my-route
```
# RBAC Presentation!!!!!

# MCP & MC
#### Web UI
1. Administrator View
2. Compute -> MachineConfigPools -> Worker
   1. NodeSelector (Press) => Nodes List => Mark as Unschedulable => Mark as Schedulable
   * Go back to Worker MachineConfigPool page
   2. Current Configuration Source (Press) => 99-worker-dns-configuration (press) => Yaml => Copy all after `base64,` => Run `echo '<PASTE HERE>' | base64 -d`

#### CLI
```bash
oc get mcp
oc get mcp worker -o json | jq '.spec.configuration.source[]'
oc get mc 99-worker-ssh -o yaml
oc get mc 99-worker-dns-configuration -o json | jq '.spec.config.storage.files[]'
oc get mc 99-worker-dns-configuration -o json | jq '.spec.config.storage.files[0].contents.source' | awk -F"," '{print $2}' | tr -d '"' | base64 -d
oc get mc 99-worker-dns-configuration -o json | jq '.spec.config.storage.files[0].path'
oc get nodes | grep worker | awk '{print $1}' | tail -n 1
sudo su -
oc login .... 
ssh core@$(oc get nodes | grep worker | awk '{print $1}' | tail -n 1)
cat /etc/resolv.conf
exit
exit
```

# Cluster Operators
#### WebUI
1. Administrator View
2. Administration => Cluster Settings => ClusterOperators => etcd (Note the `Message`) => Related Objects

#### CLI
```bash
oc get co
oc get pods -n openshift-etcd
oc get pods -n openshift-etcd-operator
oc projects | grep operator
oc projects | grep openshift-
```

# Managing Nodes , NodeSelectors & Pods running on Node
#### WebUI
1. Administrator View
2. Compute => Node (Press a node) => Pods
3. Compute => Node (Press a node) => Details => Node Labels (Ctrl+F - "node-role.kubernetes.io/")
4. Compute => Node (Press a node) => Details => Node conditions
5. Compute => Node (Press a node) => Details => Images
6. CLI:
```bash
NODE=`oc get nodes | grep worker | tail -n 1 | awk '{print $1}'`
oc debug node/$NODE
chroot /host
bash
crictl images
crictl ps
exit
exit
exit
```
7. Workloads => Deployment => Project `openshift-ingress` => Node Selector
> In case you have infra nodes, run the following commands:
#### CLI
```bash
# How to find out where this field is in the json/yaml file
oc get deployment -n openshift-ingress router-default -o json | jq -c 'paths | select(.[-1] == "nodeSelector")'
# Finding the field value in the json path
oc get deployment -n openshift-ingress router-default -o json | jq '.spec.template.spec.nodeSelector'
# How to see all pods running in cluster
oc get pods -A
# How to find the pods running on a specific node
oc get pods -A -o wide | grep <NODE_UNIQUE_STRING>
```

#Maintanence
```bash
oc adm cordon node/<NODE_NAME>
#oc adm drain node/<NODE_NAME>
oc adm uncordon node/<NODE_NAME>
```

#StatefulSet
```bash
oc get statefulsets.apps -A
oc get statefulsets.apps prometheus-k8s -n openshift-monitoring ; oc get pods -n openshift-monitoring | grep prometheus-k8s
```

#Back to RBAC
#### WebUI
1. Administartor View
2. Workloads => StatefulSets => Project `openshift-monitoring` => prometheus-k8s => YAML => CTRL+F `serviceAccountName` => Copy Name => Details => Namespace (Press) => RoleBindings => Search by name (paste) => press Role Ref of `CR prometheus-k8s` => See Rules => YAML => See the SecurityContextConstraints `resourceName` 
3. Home => Search => Resources = SCC, Name = nonroot => Press => YAML => `allowPrivilegeEscalation: true` ==> Can run `sudo` inside container

#EVENTS
```bash
oc get events -n openshift-etcd
oc get events -n openshift-etcd --sort-by='.metadata.creationTimestamp'

alias events="oc get events --sort-by='.metadata.creationTimestamp'"
oc get events -n $USER-test
oc get deployment hello-openshift -o json | jq -c 'paths | select (.[-1] == "image")'
oc get deployment hello-openshift -o json | jq '.spec.template.spec.containers[0].image'
oc patch deployment hello-openshift --type='json' -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/image", "value": "quay.io/lol/lol/lol:1"}]'
oc get pods
events -n $USER-test
oc edit deployment hello-openshift
events -n $USER-test
```

### Quotas
```bash
oc get clusterresourcequotas.quota.openshift.io
oc describe clusterresourcequotas.quota.openshift.io 343-spark
```

### DaemonSet
```bash
oc get ds -A
oc get ds -n openshift-logging
oc get pods -n openshift-logging -o wide

```
---
# Do the [exercises](./exercises)
---

#Operators - OLM (OperatorHub) & Helm
```bash
oc get catalogsource,imagecontentsourcepolicy,packagemanifests -A
helm list -A
```
