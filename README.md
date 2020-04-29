## Installing OC cli
[OC cli download](https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/)

## Deploy using deploying oc tool

* Creating a new project:

```
oc new-project anirondevops-1
```

* Creating a new application using oc cli:

```
oc new-app --name version \
    https://github.com/anirondevops/DO101-apps.git \
    --context-dir version

oc get pods
NAME              READY   STATUS    RESTARTS   AGE
version-1-build   1/1     Running   0          2m
```

* Getting running logs from the pods:

```
oc logs -f pods/version-1-build
```

* Applications created using the oc new-app command do not create a route resource by default. Run the following command to create a route and allow access to the application from the internet:

```
oc get svc
NAME      TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
version   ClusterIP   172.30.180.9   <none>        8080/TCP   48m

// we notice that there is no external IP

oc expose svc/version

// verify
oc get routes
```

* Deploying a new version of the app:

Openshift takes care of the build, container creation, stopping the pod of the older version and the changes are live with new pod on the same route.

```
oc get pods  
NAME               READY   STATUS      RESTARTS   AGE
version-2-build    0/1     Completed   0          5m1s
version-2-deploy   0/1     Completed   0          2m7s
version-2-nqljl    1/1     Running     0          98s
```