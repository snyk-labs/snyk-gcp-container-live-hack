+++
title = "Step 2: Deploy the application to GKE"
chapter = false
weight = 22
+++


For this workshop we created a GKE cluster where we run the Goof apps and you are now going to deploy the applications we build into it.

## Create and set context to a namespace

We will be running these applications in a specific namespace.

```sh
# Create a namespace
kubectl create ns snyk-gke

# Set the current context to use the new namespace
kubectl config set-context --current --namespace snyk-gke
```

## Deploy the applications

Ensure the `REPO` variable is still set from the build step and run this command. (it uses the `envsubst` utilities to plug your repository server into each of the deployment's image tags)
```
cat manifests/*.yaml | envsubst | kubectl apply -f -
```

To check the status of the pods as the application comes up, use the following command:

### Validate they are running
```sh
kubectl get all
```
The output should look something like this:
```sh
$ kubectl get all
NAME                               READY   STATUS    RESTARTS   AGE
pod/thumbnailer-679f46cbb8-f64vd   1/1     Running   0          2m50s
pod/todolist-ff547c66d-st55p       1/1     Running   0          2m50s

NAME                  TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)        AGE
service/thumbnailer   LoadBalancer   34.118.225.150   34.174.63.243    80:32052/TCP   2m53s
service/todolist      LoadBalancer   34.118.236.48    34.174.105.160   80:32217/TCP   2m52s

NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/thumbnailer   1/1     1            1           2m53s
deployment.apps/todolist      1/1     1            1           2m53s

NAME                                     DESIRED   CURRENT   READY   AGE
replicaset.apps/thumbnailer-679f46cbb8   1         1         1       2m52s
replicaset.apps/todolist-ff547c66d       1         1         1       2m52s
```

{{% notice info %}}
The pods should all show **"Running"** in their **STATUS** field, and services with a **LoadBalancer** type should have an IP or hostname for their **EXTERNAL-IP**.
<br>If either show a pending state, then wait a moment and re-run the command until they finish starting up. 
{{% /notice %}}

The following will save the `LoadBalancer` services `EXTERNAL_IP` values for later use:
```
THUMBNAILER_LB=$(kubectl get svc thumbnailer -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
TODOLIST_LB=$(kubectl get svc todolist -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
```

Once both are running, the application is accessible from the web. Get the DNS name for your app running the following command. 

```bash
echo $THUMBNAILER_LB
echo $TODOLIST_LB 
``` 

### Validate our Log4Shell exploit server is running
The eagle-eyed amoung you probably noticed that only two deployments and services are shown in the above output but we built and deployed three images. Well, the third, as it's name reveals, is a Log4Shell malicious LDAP server we will be using in a later section.  We'll discuss it more in a later section, but for now, just make sure it's running by listing the deployments in the `darkweb` namespace:
```sh
kubectl get all -n darkweb
```
```sh
$ kubectl get all -n darkweb
NAME                             READY   STATUS    RESTARTS   AGE
pod/log4shell-695d97d7bd-7bwkq   1/1     Running   0          6m5s

NAME           TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
service/evil   ClusterIP   34.118.226.121   <none>        9999/TCP   6m8s
service/ldap   ClusterIP   34.118.239.145   <none>        80/TCP     6m8s

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/log4shell   1/1     1            1           6m10s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/log4shell-695d97d7bd   1         1         1       6m8s
```
Note: services in this namespace will not get external ips as they are not running as a loadbalancer type.

## Success!

If you got here without issues, you've successfully built and deployed all of the applications and they are now live on GKE. We can open and interact with it, and while they looks harmless enough! In the next module we'll demonstrate how a vulnerable open source compoenents can create an invisible risk that can comprimise our application.