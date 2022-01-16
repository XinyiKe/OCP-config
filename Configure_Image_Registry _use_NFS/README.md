Configure OpenShift Image Registry to use NFS
===

1. Create pv and pvc
```
oc create -f pv.yaml

oc create -f pvc.yaml -n openshift-image-registry
```
2. Edit the image registry custom resource by changing below properties
```
oc edit configs.imageregistry.operator.openshift.io
...
storage:
  pvc:
    claim: registry-storage-pvc
...
managementState: Managed
...
rolloutStrategy: Recreate
```

Verification
===
```
oc get pods -n openshift-image-registry

oc rsh image-registry-xxxx
sh-4.2$ mount | grep registry
```
