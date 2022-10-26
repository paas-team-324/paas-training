```bash
cd exercises/Ex2
export USER=<your name>
oc delete project $USER-test #cleanup
oc new-project $USER-test
oc apply -f ex2.yaml
oc get pods -w
```
#### Find the FIRST problem
```bash
oc delete -f ex2.yaml

oc apply -f ex2-b.yaml
```

###### Create Route for the application with the host $USER.apps.<OCP_SUB_DOMAIN>
###### Try Access the route

##### Find the problem

