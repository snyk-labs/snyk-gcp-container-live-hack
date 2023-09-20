+++
title = "Set up initial GCP and tools"
chapter = false
weight = 11
+++

## Create a Google Cloud Platform (GCP) Account and Project

### Account creation 
_(skip if you have a GCP account)_

Open https://cloud.google.com in your browser.

If don't already have a GCP account, click the "Start Free" button at the top-right and follow the prompts to create or log into your Google user account.

You can create an account with $300 of free Google Cloud credits good for 90 days, just follow the prompts to finish the account creation,.

### Download the gcloud CLI tool
_(skip if gcloud is already installed on your workstation)_

Follow the [instalation steps](https://cloud.google.com/sdk/docs/install) for your operating system to install the `gcloud` CLI tool.

Part of gcloud setup includes creating a new GCP Project (or selecting from an existing one),
whatever that project ID is, save it in an evironment variable here to simplify later steps
```shell
export GCP_PROJECT=snyk-workshop-project
```
### Associate your billing account to the new project
Get a list of billing account ID':
```
gcloud billing accounts list
```
... which should return something like:
```
ACCOUNT_ID            NAME                OPEN  MASTER_ACCOUNT_ID
010101-1A2B3C-98765A  My Billing Account  True
```

Copy the account ID value and past into the following to link it to your new project:
```
gcloud billing projects link $GCP_PROJECT --billing-account 010101-1A2B3C-98765A
```
```
billingAccountName: billingAccounts/010101-1A2B3C-98765A
billingEnabled: true
name: projects/snyk-devrel-workshop/billingInfo
projectId: snyk-devrel-workshop
```

### Enable GCP API's for GKE access
Finally, we need to approve access to the GCP API's needed for GKE work:
```
gcloud services enable container.googleapis.com
```
```
Operation "operations/acf.p2-344195889871-f2f72ad1-5e45-462b-9c28-26b9fc8f22ab" finished successfully.
```

### Configure your default region
From Google's [list](https://cloud.google.com/about/locations#lightbox-regions-map) of regions, chose the best one for you and use it in the following:
``` shell
export GCP_REGION=us-south1 # Replace this with your chosen region
gcloud config set artifacts/location $GCP_REGION
gcloud config set compute/region $GCP_REGION
```

## Install tools nessesary for this workshop

### kubectl and GKE plugin
You'll need to install `kubectl` command line tool to your local environment (if you don't already have it).

Follow the instructions for your operating system at https://kubernetes.io/docs/tasks/tools/

You also will need to install the `gke-gcloud-auth-plugin` for `kubectl``, follow the instructions for your operating system here: https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke

### git
You'll need git to clone the repository for this workshop. Most reading this will probably already have it installed but just in case you don't, see https://git-scm.com/book/en/v2/Getting-Started-Installing-Git for official instructions.

### Docker
This workshop assumes you have a Docker (or compatible) runtime available to build and push container images. Docker Desktop or simply the `docker` cli tool are common choices for this, see https://docker.com for install details.

### Python 3
At least one of the modules assumes you have [Python 3](https://www.python.org) available on your workstation to run a helper script

### envsubst (gettext)
If you do not already have [envsubst](https://www.gnu.org/software/gettext/manual/gettext.html#envsubst-Invocation) installed as part of the GNU [gettext](https://www.gnu.org/software/gettext/) package, (most Linux distros include it), you will need it for at least one step in this workshop.
* MacOS: `brew install gettext` (assuming you have [Homebrew](https://brew.sh/) installed)
* Debian/Ubuntu/Raspbian: `apt-get install gettext-base`
* Alpine: `apk add gettext`
* Arch: `pacman -S gettext`
* CentOS: `yum install gettext`
* Fedora: `dnf install gettext`

#### An envsubst alternative
If you are unable to install gettext via one of these means, you can also declare this one-line python script in your shell to emulate it:
```shell
envsubst() { python3 -c 'import os,sys;[sys.stdout.write(os.path.expandvars(l)) for l in sys.stdin]'; }
```

You can test this is working with a simple shell command like `export MYNAME=Patch; echo 'My name is $MYNAME' | envsubst`
```shell
export MYNAME=Patch; echo 'My name is $MYNAME' | envsubst
My name is Patch
```
Remember, though, if you switch to a new shell, you'll need to re-declair that function in the new one.