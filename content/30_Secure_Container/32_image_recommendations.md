+++
title = "Step 4: Scan the Container Image for Vulnerabilities"
chapter = false
weight = 32
+++

## Scan the Container Image for Vulnerabilities

Vulnerable components like the version of ImageMagick present in our container image can be identified by Snyk. Developers using the Snyk CLI can run `snyk container test` to scan containers to get vulnerability information and base image upgrade guidance. 

Scan the image by running the following command (assuming you are cd'ed to the thumbnailer directory of your goof repo). 

```sh
snyk container test $REPO/thumbnailer:latest --file=Dockerfile --exclude-app-vulns
```

When the scan completes, review the list of vulnerabilities. There are quite a few!

```text
...
Package manager:   deb
Target file:       Dockerfile
Project name:      docker-image|us-south1-docker.pkg.dev/my-project/snyk-workshop/thumbnailer
Docker image:      us-south1-docker.pkg.dev/my-project/snyk-workshop/thumbnailer:latest
Platform:          linux/amd64
Base image:        python:3.11.1
Licenses:          enabled

Tested 427 dependencies for known issues, found 385 issues.

Base Image     Vulnerabilities  Severity
python:3.11.1  385              6 critical, 21 high, 71 medium, 287 low

Recommendations for base image upgrade:

Minor upgrades
Base Image        Vulnerabilities  Severity
python:3.12.0rc1  169              0 critical, 0 high, 1 medium, 168 low

Alternative image types
Base Image                      Vulnerabilities  Severity
python:3.12.0rc1-slim-bookworm  38               0 critical, 0 high, 0 medium, 38 low
python:3.12.0b4-slim            38               0 critical, 0 high, 0 medium, 38 low
python:3.12.0rc1-slim-bullseye  59               0 critical, 0 high, 0 medium, 59 low
python:3.11.4-bookworm          170              0 critical, 0 high, 2 medium, 168 low

...
```
As you can see, the Snyk engine is recommending we consider upgrading to newer base images that have fixes for many of the issues found in ours.  If you search the output of the scan, you also can see that our CVE is, indeed in this base image.

```text
✗ Medium severity vulnerability found in imagemagick/imagemagick-6-common
  Description: CVE-2022-44268
  Info: https://security.snyk.io/vuln/SNYK-DEBIAN11-IMAGEMAGICK-3314444
  Introduced through: imagemagick/libmagickcore-dev@8:6.9.11.60+dfsg-1.3, imagemagick/libmagickwand-dev@8:6.9.11.60+dfsg-1.3, imagemagick@8:6.9.11.60+dfsg-1.3
  From: imagemagick/libmagickcore-dev@8:6.9.11.60+dfsg-1.3 > imagemagick/imagemagick-6-common@8:6.9.11.60+dfsg-1.3
  From: imagemagick/libmagickwand-dev@8:6.9.11.60+dfsg-1.3 > imagemagick/imagemagick-6-common@8:6.9.11.60+dfsg-1.3
  From: imagemagick@8:6.9.11.60+dfsg-1.3 > imagemagick/imagemagick-6.q16@8:6.9.11.60+dfsg-1.3 > imagemagick/libmagickcore-6.q16-6@8:6.9.11.60+dfsg-1.3 > imagemagick/imagemagick-6-common@8:6.9.11.60+dfsg-1.3
  and 24 more...
  Image layer: Introduced by your base image (python:3.11.1)
  Fixed in: 8:6.9.11.60+dfsg-1.3+deb11u1
```

## Fixing our image
Let's take Snyk's recommendation and upgrade our base image:

Double click on the `Dockerfile` under the `thumbnailer` directory in your Cloud9 IDE sidebar and change the first line from:
```docker
FROM python:3.11.1
```
To:
```docker
FROM python:3.12.0rc1
```

Now, rebuild the image:
```bash
docker build -t $REPO/thumbnailer . --platform=linux/amd64
```
 ... and let's re-scan it:
```bash
snyk container test $REPO/thumbnailer --file=Dockerfile --exclude-app-vulns
```

The results now should show the lower vulnerability count.
```text
...
Package manager:   deb
Target file:       Dockerfile
Project name:      docker-image|us-south1-docker.pkg.dev/my-project/snyk-workshop/thumbnailer
Docker image:      us-south1-docker.pkg.dev/my-project/snyk-workshop/thumbnailer:latest
Platform:          linux/amd64
Base image:        python:3.12.0rc1
Licenses:          enabled

Tested 430 dependencies for known issues, found 169 issues.

Base Image        Vulnerabilities  Severity
python:3.12.0rc1  169              0 critical, 0 high, 1 medium, 168 low

Recommendations for base image upgrade:

Alternative image types
Base Image                      Vulnerabilities  Severity
python:3.12.0rc1-slim-bookworm  38               0 critical, 0 high, 0 medium, 38 low
python:3.12.0b4-slim            38               0 critical, 0 high, 0 medium, 38 low
python:3.12.0rc1-slim-bullseye  59               0 critical, 0 high, 0 medium, 59 low

```
If you search through the output, you should find our ImageMagick CVE is gone.

## About the recomendations
Snyk recommends less vulnerable base images grouped by how likely they are to be compatible:

- Minor upgrades are the most likely to be compatible with little work,
- Major upgrades can introduce breaking changes depending on image usage,
- Alternative architecture images are shown for more technical users to investigate.



