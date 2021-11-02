# Push Bundle Image to partner connect portal for certificaiton

> Note: This Old method of certifying operator bundle image is deprecated!
### Prerequisites
* Login to partner account and create project

Step 01 : Upload operator bundle image to connect portal for certification

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
Step 02 : Check if image has been uploaded and started scanning
- Go to https://connect.redhat.com/project/<project_id>/images
- Sometimes it takes a while for the image to appear in the connect portal. Once the scanning starts, it will take time to complete
- If the test passed, continue next step. If not, then check the scan for the errors and fix in your application or in docker file start the process all over gain till you pass the test.

Step 03 : Complete the checklist
- Open the checklist 
```console
https://connect.redhat.com/project/<project_id>/checklist
```
- Complete any items that are pending to complete
- The image is now certify
