# RHOS Container Image Certification Process

Step 02 : Build and push image to image registry
```console
   docker build -t <image name>:<version> .
```
```console
   docker push <image repository name>/<image name>:<version>
```

Step 03 : Test your Image on OpenShift (ROKS) Cluster
* [Getting any Docker image running in your own OpenShift cluster](https://cloud.redhat.com/blog/getting-any-docker-image-running-in-your-own-openshift-cluster)


Step 04 : Upload the image to the Red Hat project registry for the certification.
> Note : </br>
  (1) Use Registry Key from https://connect.redhat.com/project/<project_id>/images/upload-image as the password when prompted from the 'docker login' command below.</br>
  (2) On Mac PC, copy and paste might not work at the prompt. Use the --password-stdin flag instead.</br>
  (3) [Refer to the Docker docs for more details](https://docs.docker.com/engine/reference/commandline/login/provide-a-password-using-stdin)

* Login to the Red Hat Partner Connect Portal
```console
  docker login -u unused scan.connect.redhat.com
```
* Get the ID of the image
```console
docker images
```
* Use ospid-id from your partner connect portal account < https://connect.redhat.com/project/<project_id>/images/upload-image>
```console
docker tag <image id> scan.connect.redhat.com/<ospid-id>/<image name>:<version>
```
* Push the iamge to the Red Hat Connect portal for certification
```console
docker push scan.connect.redhat.com/<ospid-id>/<image name>:<version>
```

Step 05 : Verify if image has been sucessfully uploaded and start scanning
* Login in to your partner connect portal and navigate to https://connect.redhat.com/project/<project_id>/images
* It might take a while for the image to appear. You then need to wait for the certification process to finish.
* If "certification test" passed then continue to next step. Otherwise, check the scan logs for errors and update the image accordingly and continue the process untill all the errors gets fixed and test passes.
* Go back to "Checklist" (https://connect.redhat.com/project/<project_id>/checklist) and finish any items that are not complete. The image is now certified.
