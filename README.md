# acm-applicationdelivery
Custom role and clusterrole to use for user and groups that needs to manage only applications delivery using RHACM. 

## Requirements 

- self-provisioning disabled on the cluster HUB (https://docs.openshift.com/container-platform/4.12/applications/projects/configuring-project-creation.html#disabling-project-self-provisioning_configuring-project-creation)
- each application must be managed on a different namespace on the cluster hub 


## Test description

*Supposing the user testuser1 is the desired application delivery administrator.*

- Create the needed cluster roles. (oc apply -f rhacm-application-delivery-admin.yaml, oc apply -f  rhacm-application-manager.yaml)
- Add the rhacm-application-manager cluster role on the user testuser1. (oc adm policy add-cluster-role-to-user   rhacm-application-manager  testuser1)
- Create the following projects: app-a and app-b. 
- Add the rhacm-application-delivery-admin role on the user testuser1 on the projects app-a and app-b. ( oc adm policy add-role-to-user   rhacm-application-delivery-admin  testuser1 -n app-a,oc adm policy add-role-to-user   rhacm-application-delivery-admin  testuser1 -n app-b ) 

Once the above configurations have been completed, it is possible to login the cluster hub using testuser1 credentials and create the resources in the folders test-application-a and test-application-b with the following command on the root of this git repo. 

oc apply -k . 
