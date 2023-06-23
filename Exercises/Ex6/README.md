```bash
cd exercises/Ex6
export USER=<your name>
oc delete project $USER-test #cleanup
oc new-project $USER-test
oc apply -f ex6.yaml
```
#### Try create the configmap as the new serviceaccount test2
```bash
oc create configmap test --from-file=lol --as=system:serviceaccount:$USER-test:test2
```

#### Findout the problem


Give the serviceaccount `test2` the `edit` permission with one command and run the command from above again
