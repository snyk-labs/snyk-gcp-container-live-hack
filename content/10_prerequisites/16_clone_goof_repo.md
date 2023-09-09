+++
title = "Clone the sample application repo"
chapter = false
weight = 16
+++

## Create your copy of the repo

A sample application, called Goof, is provided for this workshop as a GitHub template. Navigate to the [GitHub Repo for the Goof application](https://github.com/snyk-partners/goof) and click "Use this Template" and then "Create a new repository" to create a copy of the Repo to your personal GitHub account. 


![gh-template](/images/gh-use-template.png)

{{% notice info %}}
Be sure to name the new Repo `goof` otherwise things will break later on.
{{% /notice %}}

We recommend you make your copy have "public" visibility, it will simplify working with it during this workshop.

![gh-create-copy](/images/gh-create-copy.png)

To copy-paste the commands in the instructions set an environment variable with your GitHub ID. Your GitHub ID is displayed in the upper right corner in [GitHub.com](github.com).

![gh-id](/images/gh-id.png)

```sh
GithubId=<your_github_id>
```

## Clone the repo
After you create the Repo, clone the Repo to your local environment by using the `git clone` command. Once the clone completes, change to the repo's top level directory. 

```sh
git clone https://github.com/$GithubId/goof && cd goof
```

This copies the Repo files to your local environment. 
