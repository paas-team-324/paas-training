```bash
cd exercises/Ex4
export USER=<your name>
oc delete project $USER-test #cleanup
oc new-project $USER-test
oc apply -f ex4.yaml
oc apply -f ex4-b.yaml
```


```bash
POD=$(oc get pods | grep hello-openshift2 | awk '{print $1}')
oc rsh $POD
bash
curl hello-openshift.$USER-test.svc.cluster.local:8080
exit
exit

oc create route edge my-route --service=hello-openshift2
URL=$(oc get route my-route | awk '{print $2}' | grep apps)
curl https://$URL
```

### Look a little online about networkpolicies and understand the problem. 
### Also, allow access between pods in the same namespace only
