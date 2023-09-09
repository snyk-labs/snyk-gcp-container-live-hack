+++
title = "Deploy a GKE cluster"
chapter = false
weight = 12
+++

## Now that you have all of the tools installed, let's deploy a GKE Autopilot cluster so we have a platform to deploy our applications to.

### CLI Invocation
Perhaps the easiest way to setup a GKE cluster is to run the command as shown below in your local terminal: `gcloud container clusters create-auto snyk-cluster`

You should get output that looks something like the following:
```
$ gcloud container clusters create-auto snyk-cluster

Creating cluster snyk-cluster in us-south1... Cluster is being health-checked (master is healthy)...done.
Created [https://container.googleapis.com/v1/projects/snyk-devrel-workshop/zones/us-south1/clusters/snyk-cluster].
To inspect the contents of your cluster, go to: https://console.cloud.google.com/kubernetes/workload_/gcloud/us-south1/snyk-cluster?project=snyk-devrel-workshop-smalls-1
kubeconfig entry generated for snyk-cluster.
NAME          LOCATION   MASTER_VERSION  MASTER_IP      MACHINE_TYPE  NODE_VERSION    NUM_NODES  STATUS
snyk-cluster  us-south1  1.27.3-gke.100  34.174.41.138  e2-medium     1.27.3-gke.100  3          RUNNING
```
{{% notice note %}}
This may take a few minutes to complete while GCP provisions and does health checks on the cluster resources.
{{% /notice %}}

## Configure kubeconfig
Confirm that you have access to your GKE cluster by running the following command:  `kubectl get nodes`

If you have access you should see something like the following:
```
$ kubectl get nodes

NAME                                           STATUS   ROLES    AGE   VERSION
gk3-hello-cluster-default-pool-7544a6ec-2s0b   Ready    <none>   18m   v1.27.3-gke.100
gk3-hello-cluster-default-pool-f4aa75ff-905w   Ready    <none>   18m   v1.27.3-gke.100
```

### Troubleshooting
If you are getting timeouts or errors when running the `kubectl get nodes` command, or if it is returning node names that are obviously not from your new GKE cluster you may have the wrong context configured for kubectl.  Check this by running `kubectl config get-contexts`:
```
$ kubectl config get-contexts

CURRENT   NAME                                                       CLUSTER                                                    AUTHINFO                                                   NAMESPACE
          demo                                                       kind-kind                                                  demo-sa
          docker-desktop                                             docker-desktop                                             docker-desktop
*         gke_snyk-devrel-workshop_us-south1_snyk-cluster   gke_snyk-devrel-workshop_us-south1_snyk-cluster   gke_snyk-devrel-workshop_us-south1_snyk-cluster
          goof                                                       goof                                                       goof
```
If the `*` is not showing up next to the gke cluster, run `kubectl config use-context gke_snyk-devrel-workshop_us-south1_snyk-cluster` and try pulling the node list again.  (replace that name as approrpriate)

