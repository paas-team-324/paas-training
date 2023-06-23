```bash
cd exercises/Ex3
export USER=<your name>
oc delete project $USER-test #cleanup
oc new-project $USER-test
oc apply -f ex3.yaml
oc apply -f ex3-b.yaml
```

### Find out the problem WITHOUT looking at the yaml files & fix it

#### When you think you fixed it, run one of the following:
```bash
oc scale --replicas=0 deployment hello-openshift
oc scale --replicas=1 deployment hello-openshift


#OR
oc rollout restart deployment hello-openshift 
```
