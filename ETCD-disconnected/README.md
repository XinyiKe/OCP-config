ETCD-backup cronjob
====

Step1
1. Pull image for ETCD-backup cronjob
```
podman pull registry.redhat.io/openshift4/ose-cli:latest --authfile /opt/registry/configs/<pull-combined.json>
```
2. tag the image
```
podman tag registry.redhat.io/openshift4/ose-cli:latest mirrorregistry:5000/ocp4/openshift4/ose-cli:v4.9.0
```
3. Check the image
```
podman images
```
4. Push the image to local registry
```
podman push mirrorregistry:5000/ocp4/openshift4/ose-cli:v4.9.0 --authfile /opt/registry/configs/<pull-combined.json>
```
5. Check the registry (sometimes need to use 'podman stop' and 'podman start' to restart)
```
podman ps
```
#

Step2
1. Create new project
```
oc new-project etcd-backup
```
2. Create RBAC role
```
oc create -f rbac.yaml
```
3. Create PV and PVC to store backup files
```
oc apply -f pv.yaml

oc get pv 

oc apply -f pvc.yaml -n etcd-backup

oc get pvc -n etcd-backup
```
4. Create the cronjob
```
oc create -f cronjob.yaml -n etcd-backup
```
#

Test 
====
Create a job in etcd-backup namespace to test
```
oc create job --from=cronjob/etcd-backup test-job

oc logs -l job-name=test-job -n etcd-backup
```
