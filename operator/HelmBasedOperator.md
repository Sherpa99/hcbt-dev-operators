# Steps to build HELM Based Operator

### Pre-requisites
- [Operator-sdk](https://sdk.operatorframework.io/docs/installation/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-coursescognitiveclassai-SkillsNetworkCoursesIBMDeveloperSkillsNetworkCO0201ENSkillsNetwork23008840-2021-01-01) v1.5.0+ installed </br>
- [Kubectl](https://kubernetes.io/docs/tasks/tools/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-coursescognitiveclassai-SkillsNetworkCoursesIBMDeveloperSkillsNetworkCO0201ENSkillsNetwork23008840-2021-01-01#kubectl) v1.17.0+ installed </br>
- Admin access to a ROKS cluster. 
- [Docker](https://docs.docker.com/get-docker/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-coursescognitiveclassai-SkillsNetworkCoursesIBMDeveloperSkillsNetworkCO0201ENSkillsNetwork23008840-2021-01-01) v3.2.2+ installed
- Access to a Docker image repository such as [Docker Hub](https://hub.docker.com) or [quay.io](https://quay.io/) </br>
- [HELM](https://helm.sh/docs/intro/install/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-coursescognitiveclassai-SkillsNetworkCoursesIBMDeveloperSkillsNetworkCO0201ENSkillsNetwork23008840-2021-01-01) v3+ installed
- [Create Account](https://sso.redhat.com/auth/realms/redhat-external/login-actions/registration?client_id=https%3A%2F%2Fconnect.redhat.com%2Fsimplesaml%2Fwww%2Fmodule.php%2Fsaml%2Fsp%2Fmetadata.php%2Fdefault-sp&tab_id=-F6r3dMSGl0) in RedHat Connect Portal

## Step 01 : Create a project
Make a new directory in hour pc, and `init` a project in it.
> a. Find where the WordPress chart is hosted</br>
       ``` helm search hub wordpress ``` </br> 
> b. Add the Bitnami repository to the local repository</br>
       ``` helm repo add bitnami https://charts.bitnami.com/bitnami ```</br>
 1. Create a new directory
      ```console
       mkdir wordpress-operator
      ```
 2. Change directory to memcached-operator
      ```console
       cd wordpress-operator
      ```
3. Intialize the project by calling following commands to build sample WordPress operator.
      ```console
       operator-sdk init --plugins helm --helm-chart bitnami/wordpress --domain example.com --group helm-chart --version v1alpha1 --kind Wordpress
      ```
## Step 02 : Build and Deploy your operator 

1. Set environment variable for your docker image
   ```console
    export USERNAME=<docker-registry-username>
   ```
2. Build and push your docker image to external or internal docker registry 
   ```console
      make docker-build docker-push IMG=docker.io/$USERNAME/wordpress-operator:v1.0.0 
   ```
> I am using my personal docker registry here.
3. Deploy the operator</br>
   ```console
      make deploy IMG=docker.io/$USERNAME/wordpress-operator:v1.0.0
   ```
4. Check the CRD</br>
   ```console
      kubectl get crds | grep wordpress
   ```
5. Check the controller deployment and pods </br>
   ```console
      kubectl get deployment -n wordpress-operator-system
   ```
   ```console
      kubectl get pods -n wordpress-operator-system
   ```
6. Check the RBAC </br>
   ```console
      kubectl get clusterroles | grep wordpress
   ```  
   ```console
      kubectl get clusterrolebindings | grep wordpress
   ``` 
   ```console
      kubectl get roles -n wordpress-operator-system
   ``` 
   ```console
      kubectl get rolebindings -n wordpress-operator-system
   ``` 

## Step 03 : Deploy a WordPress Instance
1. Create a demo WordPress custom resource YAML manifest:
   ```console
      cp config/samples/helm-chart_v1alpha1_wordpress.yaml config/samples/wordpress-demo.yaml
   ```
   a) Update name: `wordpress-sample` to name: `wordpress-demo` </br>
   b) Change spec.wordpressBlogName to `The Demo Blog!`. We check this later when the server is running</br>
> Note: This is equivalent to a [Helm values override](https://helm.sh/docs/chart_template_guide/values_files/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkCO0201ENSkillsNetwork23008840-2021-01-01)
2. Create NameSpace called wordpress-demo
   ```console
      kubectl create ns wordpress-demo
   ```
3. Deploy WordPress
   ```console
      kubectl apply -f config/samples/wordpress-demo.yaml -n wordpress-demo --validate=false
   ```
> Note: You need to turn off validation `(--validate=false)` because the WordPress Helm chart uses null values for some defaults, and this can cause the Kubernetes schema validation to fail. This should not be done in production systems
4. Verify that WordPress is running
   ```console
      kubectl get pods -n wordpress-demo
   ```
 > Note: You can also check that Helm deployed the WordPress chart from the operator
 ```console
    helm ls -A
 ```
 
 5. Check that you can access WordPress
   ```console
      kubectl logs deployment.apps/wordpress-operator-controller-manager -n wordpress-operator-system -c manager
   ```  
 > Note: The details on how to access the WordPress site are in the chart notes when you deploy the chart. You can get the details in one of two ways
 6. Get the notes using the Helm client as follows
   ```console
      helm get notes wordpress-demo -n wordpress-demo
   ```    
   
## Step 04 : Clean up
> Warning: This command will remove the WordPress deployment, the operator, and all other resources created
1. Delete Deployment
```console
   kubectl delete -f config/samples/wordpress-demo.yaml -n wordpress-demo
```
2. Undeploy using make command
```console
   make undeploy
```
