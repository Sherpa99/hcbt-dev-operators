# Red Hat Container Images Certification Process
Step 01 : Build Docker Image

Example Dockerfile for Container Application
```console
FROM registry.redhat.io/rhel7
MAINTAINER NAME <EMAIL@ADDRESS>

### Required OpenShift Labels 
LABEL name="APPLICATION NAME" \
      maintainer="EMAIL@ADDRESS" \
      vendor="COMPANY NAME" \
      version="VERSION NUMBER" \
      release="RELEASE NUMBER" \
      summary="APPLICATION SUMMARY" \
      description="APPLICATION DESCRIPTION" \

### add licenses to this directory
COPY licenses /licenses

### Add necessary Red Hat repos here
RUN REPOLIST=rhel-7-server-rpms,rhel-7-server-optional-rpms \

### Add your package needs here
    INSTALL_PKGS="PACKAGES HERE" && \
    yum -y update-minimal --disablerepo "*" --enablerepo rhel-7-server-rpms --setopt=tsflags=nodocs \
      --security --sec-severity=Important --sec-severity=Critical && \
    yum -y install --disablerepo "*" --enablerepo ${REPOLIST} --setopt=tsflags=nodocs ${INSTALL_PKGS} && \

### Install your application here -- add all other necessary items to build your image
RUN "ANY OTHER INSTRUCTIONS HERE"
```
Step 02 : Build and push image to image registry
```console
   docker build -t <image name>:<version> .
```
```console
   docker push <image repository name>/<image name>:<version>
```

Step 03 : Test your Image on OpenShift (ROKS) Cluster
* [Getting any Docker image running in your own OpenShift cluster](https://cloud.redhat.com/blog/getting-any-docker-image-running-in-your-own-openshift-cluster)
