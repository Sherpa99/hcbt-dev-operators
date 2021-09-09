# Build Operator Bundle Image and Certiry
Prerequisites
- Login to partner account and create project
- Install docker in your local environment
- Container Registry

Step 01 : Create a bundle for your operator release
```console
export USERNAME=<docker-registry-userid>
```
```console
export OPERATOR_IMG=docker.io/$USERNAME/<image name>:<image version>
```
```console
make bundle IMG=$OPERATOR_IMG
```

Step 02 : Update bundle files for certification
- Open `bundle/manifests/memcached-operator.clusterserviceversion.yaml`
- Replace `mediatype: " "` with following line <br>
  ```console
  mediatype: "image/gif"
  ```
- Open `bundle/metadata/annotations.yaml`
- Add following line <br>
  ```console
  operators.operatorframework.io.bundle.channel.default.v1: alpha
  ```
- Open `bundle.Dockerfile`
- Add following lines <br>
  ```console
  LABEL operators.operatorframework.io.bundle.channel.default.v1=alpha
  LABEL com.redhat.openshift.versions="v4.6"
  LABEL com.redhat.delivery.operator.bundle=true
  ```
 
Step 03 : Build Bundle Image  
  
```console
export USERNAME=<docker-registry-userid>
```
```console
export BUNDLE_IMG=docker.io/$USERNAME/<image name>:<image version>
```
```console
make bundle-build IMG=$BUNDLE_IMG
```  
Step 04 : Push to container registry  
```console
make docker-push IMG=$BUNDLE_IMG
```
Step 05 : Upload operator bundle image to connect portal for certification

Login to Partnet connect portal
```console
docker login -u unused scan.connect.redhat.com
```
Get the ID of the bundle image
```console
docker images
```
Tag using your project ID
```console
docker tag <bundle image id> scan.connect.redhat.com/<ospid-id>/<bundle image name>:<version>
```
Push the iamge to the partnet connect portal for scanning
```console
docker push scan.connect.redhat.com/<ospid-id>/<bundle image name>:<version>
```
