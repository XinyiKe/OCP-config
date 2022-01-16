Configure log forwarder 
===

1. Enabling the cluster-wide proxy
```
oc create configmap es-ca-bundle --from-file=/home/padm1/external-es/ca-bundle.crt -n openshift-config
```
```
oc edit proxy/cluster
apiVersion: config.openshift.io/v1
kind: Proxy
metadata:
  name: cluster
spec:
  trustedCA:
    name: "es-ca-bundle
```

2.  if you cannot use mutual TLS (mTLS) keys because a third party operates the Elasticsearch instance, you can use HTTP or HTTPS and set a secret that contains the username and password.
```
oc create -f secret_with_username_password.yaml
```
3. Create or edit a YAML file that defines the ClusterLogForwarder
```
oc create -f logforwarder.yaml
```
