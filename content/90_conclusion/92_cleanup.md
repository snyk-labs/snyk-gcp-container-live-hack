+++
title = "Cleanup"
chapter = false
weight = 92
+++

### Cleanup 
In order to prevent charges to your account we recommend cleaning up the infrastructure that was created. If you plan to keep things running so you can examine the workshop a bit more please remember to do the cleanup when you are done. It is very easy to leave things running in an GCP account, forget about it, and then accrue charges.

Assuming you set the `GCP_PROJECT` variable with the project name you created via `gcloud init`, tear down can be done by simply running `gcloud projects delete $GCP_PROJECT`.

```bash
# Delete CloudFormation Stacks
$ gcloud projects delete $GCP_PROJECT
Your project will be deleted.

Do you want to continue (Y/n)?

Deleted [https://cloudresourcemanager.googleapis.com/v1/projects/snyk-workshop-project].

You can undo this operation for a limited period by running the command below.
    $ gcloud projects undelete snyk-workshop-project

See https://cloud.google.com/resource-manager/docs/creating-managing-projects for information on shutting down projects.
```
