```bash
cd exercises/Ex8
export USER=<your name>
oc delete project $USER-test #cleanup
oc new-project $USER-test
oc apply -f ex7.yaml
```
### This time there is no problem
### I want you to:
1. Create confipamp from the file my-new-page 
> use the command ```oc create configmap --help```
2. Mount this configmap into the pod to the path: /var/www/html/index.html
> use the command ```oc set volume --help```
