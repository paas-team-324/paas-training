```bash
cd exercises/Ex5
export USER=<your name>
oc delete project $USER-test #cleanup
oc new-project $USER-test
oc apply -f ex5.yaml
```

#### Findout the problem
#### Fix it with the command in the command.sh file
> also run ```oc rollout restart deployment hello-openshift```
