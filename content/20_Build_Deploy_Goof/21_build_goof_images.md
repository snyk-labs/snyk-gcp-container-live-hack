+++
title = "Step 1: Build the Goof container image"
chapter = false
weight = 21
+++

Now that you've cloned the repo to your working environment, we'll build a few container images that run our application examples in Google Kubernetes Engine (GKE) but before we can do anything, we need to create an Google Artifact Registry (GAR) repository to house the build images so GKE has somewhere to pull them from.

## Create the repository
The following command will create a repository for the images we will be building:

```sh
gcloud artifacts repositories create snyk-workshop --repository-format=Docker
```
After it finishes you should get a reponse with the following:
```
Created repository [snyk-workshop].
```

## Log into your repositories
Next, we log into our repositories (one command logs you into any of your repositories)
```sh
gcloud auth configure-docker $GCP_REGION-docker.pkg.dev
```
This should return a `Login Succeeded` repsonse.

## Set a variable up to hold the long Google repository string
Google Artifact Registry repositories have fairly long names so to make like easier for the rest of this workshop, let's create am environment variable to hold it.  In the following, replace "PROJECT-NAME" and "REGION" with whatver your info:
```shell
export REPO="$GCP_REGION-docker.pkg.dev/$GCP_PROJECT/snyk-workshop"
```

## Build container images

Now we will build the images. Be sure you are cd'ed into the top level of the cloned goof repo directory and  run the following commands to build the container images.

{{% notice info %}}
The following commands include `--platform=linux/amd64` to ensure the correct CPU architecture is used for your GKE cluster. This is nessesary if your local workstation is not Intel/AMD based, i.e. Apple Silicon or Raspberry Pi.  If you are on an Intel/AMD based workstation, you may omit that portion if you like. 
{{% /notice %}}


```sh
docker build -t $REPO/thumbnailer:latest --platform=linux/amd64 thumbnailer

docker build -t $REPO/todolist:latest --platform=linux/amd64  todolist

docker build -t $REPO/log4shell-server:latest --platform=linux/amd64 todolist/exploits/log4shell-server

```

When all of the build processes are complete, if you run `docker images` you should see three rows like this:
```
$ docker images                                                                                                                                                             
REPOSITORY         TAG       IMAGE ID       CREATED          SIZE
us-south1-docker.pkg.dev/my-project/snyk-workshop/log4shell-server   latest    a42b0d443129   10 minutes ago   535MB
us-south1-docker.pkg.dev/my-project/snyk-workshop/todolist           latest    57ad8c044dbd   15 minutes ago   612MB
us-south1-docker.pkg.dev/my-project/snyk-workshop/thumbnailer        latest    3406c6d949b4   20 minutes ago   941MB
```

## Push container images to your Google Artifcat Registry
Next, we will push the images to our registry:

```sh
docker push $REPO/thumbnailer:latest
docker push $REPO/todolist:latest
docker push $REPO/log4shell-server:latest

```

Once the pushes complete, log in to your [Google Artifact Registry](https://console.cloud.google.com/artifacts) to see your new image repositories (you'll need to open the correct project and click on the `snyk-workshop` repositry to see the images). 

![gar-repos](/images/gar-repos.png)