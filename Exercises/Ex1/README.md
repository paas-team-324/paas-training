```bash
cd exercises/Ex1
export USER=<your name>
oc delete project $USER-test #cleanup
oc new-project $USER-test
oc apply -f ex1.yaml
```

#### Troubleshoot the issue!

> Use the following commands:
```bash
oc get pods

oc get all
```

##### WebUI
1. Administrator View
2. Home => Events => Project `$USER-test` => Understand the problem
3. Workloads => Deployment => `hello-openshift` => Image & Conditions => Understand the problem

##### CLI
```bash
oc describe deployment hello-openshift

oc describe replicaset hello-openshift

alias events="oc get events --sort-by='.metadata.creationTimestamp'"
events -n $USER-test

grep image ex1.yaml
```

##### Fix the issue!!!
> Use the image: `docker.io/httpd:2.4` and the command `oc edit deployment hello-openshift -n $USER-test`

### Check if you have a running pod

### oooo.... CrashLoopBackOff... try figure out why 
###### WebUI
1. Administrator View
2. Workloads => Deployment => `hello-openshift` => Pods => Logs  => Understand the problem

###### CLI
```bash
oc get pods -o name
oc logs -f $(!!)
```

### explain the instructor why privileged container are forbidden

##### Fix the issue!!!
> Use the image: `registry.access.redhat.com/ubi8/httpd-24:latest` and the command `oc edit deployment hello-openshift -n $USER-test`

