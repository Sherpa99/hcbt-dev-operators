# Building Golang Operator and RHOS Certification process

### Pre-requisites
- [Operator-sdk](https://sdk.operatorframework.io/docs/installation/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-coursescognitiveclassai-SkillsNetworkCoursesIBMDeveloperSkillsNetworkCO0201ENSkillsNetwork23008840-2021-01-01) v1.5.0+ installed </br>
- [Kubectl](https://kubernetes.io/docs/tasks/tools/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-coursescognitiveclassai-SkillsNetworkCoursesIBMDeveloperSkillsNetworkCO0201ENSkillsNetwork23008840-2021-01-01#kubectl) v1.17.0+ installed </br>
- Admin access to a ROKS cluster. 
- [Docker](https://docs.docker.com/get-docker/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-coursescognitiveclassai-SkillsNetworkCoursesIBMDeveloperSkillsNetworkCO0201ENSkillsNetwork23008840-2021-01-01) v3.2.2+ installed
- Access to a Docker image repository such as [Docker Hub](https://hub.docker.com) or [quay.io](https://quay.io/) </br>
- [Golang](https://golang.org/doc/install?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-coursescognitiveclassai-SkillsNetworkCoursesIBMDeveloperSkillsNetworkCO0201ENSkillsNetwork23008840-2021-01-01) v1.16.0+ installed
- [Create Account](https://sso.redhat.com/auth/realms/redhat-external/login-actions/registration?client_id=https%3A%2F%2Fconnect.redhat.com%2Fsimplesaml%2Fwww%2Fmodule.php%2Fsaml%2Fsp%2Fmetadata.php%2Fdefault-sp&tab_id=-F6r3dMSGl0) in RedHat Connect Portal
- [IBM Acess](https://ibm.ent.box.com/s/gf10aqcsycmjzq20nav5gtywmoitykj3) to Red Hat education from the Red Hat Partner Connect Website

## Step 01 : Create a project
Make a new directory in hour pc, and `init` a project in it.

 1. Create a new directory
      ```console
       mkdir memcached-operator
      ```
 2. Change directory to memcached-operator
      ```console
       cd memcached-operator
      ```
3. Intialize the project by calling following commands.
      ```console
       operator-sdk init --domain=example.com --repo=github.com/example/memcached-operator
      ```
## Step 02 : Create an API 
Use the create command to generate a CRD and a controller
1. Create API: </br>
     ```console
      operator-sdk create api --group cache --version v1alpha1 --kind Memcached --resource=true --controller=true 
     ```
2. Make changes in `api/v1alpha1/memcached_types.go` by replacing both the functions with following code.</br>
 ```console
// MemcachedSpec defines the desired state of Memcached
type MemcachedSpec struct {
	// INSERT ADDITIONAL SPEC FIELDS - desired state of cluster
	// Important: Run "make" to regenerate code after modifying this file

	// Foo is an example field of Memcached. Edit memcached_types.go to remove/update
	Foo  string `json:"foo,omitempty"`
	Size int32  `json:"size"`
}

// MemcachedStatus defines the observed state of Memcached
type MemcachedStatus struct {
	// Nodes are the names of the memcached pods
	Nodes []string `json:"nodes"`
}
 ```
3. Run generate commands to implement additional code changes.
   ```console
      make generate
   ```
4. Generate YAML manifests for CRD </br>
   ```console
      make manifests
   ```
## Step 03 : Create a controller
1. Repalce `controllers/memcached_controller.go` with following code:
- [Reference implementation](https://github.com/Sherpa99/hcbt-dev-operators/blob/main/limitations/controllers.md)
> Note: The specifics of implementing the reconcile loop is beyond this tutorial, and will be covered in the advanced module
2. Re-generate YAML manifests for CRD </br>
   ```console
      make manifests
   ```
## Step 04 : Build and deploy your operator
1. Build a operator image and push to docker registry
```console
export USERNAME=<docker-username>
make docker-build docker-push IMG=docker.io/$USERNAME/memcached-operator:v1.0.0
```
2. Deploy the operator in ROKS cluster
```console
make deploy IMG=docker.io/$USERNAME/memcached-operator:v1.0.0
```
## Step 05 : Cleanup
When you are done, clean the operator deployment in ROKS cluster
```console
make undeploy

```
## Step 06 : Red Hat Operator Certification 
[Operator Certification Process]()

## Step 07 : Build Operator Bundle Image and Certify
[Operator Certification Process]()
