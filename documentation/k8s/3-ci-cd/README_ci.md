# Create DevSecOps pipelines to update a Kubernetes application 

DevSecOps integrates a set of security and compliance controls into the DevOps processes, allowing frequent code delivery while maintaining a strong security posture and continuous state of audit-readiness.

This setion steps you through the creation of Continuous Integration (CI) toolchains using an IBM Cloud toolchain templates, which we will customize to deploy the sample application to an IBM Cloud Kuberenets cluster.

### Before you begin

Set up the following pre-requisites:

- Ensure you have cloned the following repos to your GitHub account:

* https://github.com/IBM/multi-tenancy
* https://github.com/IBM/multi-tenancy-backend
* https://github.com/IBM/multi-tenancy-frontend

- An instance of the IBM Cloud Secrets Manager service
- An IBM Cloud Kubernetes Service cluster or an IBM Cloud OpenShift cluster
- Create a namespace in the IBM Cloud Container Registry (access via hamburger menu->Container Registry)
- Create Postgres and AppId service instances, and service API keys.  See section [Creation of managed IBM Cloud Services]()


### Create an IBM Cloud API key in Secrets Manager

In IBM Cloud, navigate to Manage->Access (IAM)->API Keys.  Create a new IBM Cloud API Key and make a note of it.

In your Secrets Manager service, create a new Arbitary value and paste in the IBM Cloud API Key:

![](../../images/cicd-k8s/CI-Backend/3.png)


## Update the sample application configuration

In your clone of https://github.com/IBM/multi-tenancy, navigate to the folder configuration/tenants.  This is the configuration file for each tenant of the SaaS application which the toolchains will deploy.  You should update both `tenant-a.json` and `tenant-b.json` with the name of the IKS cluster you are using for testing, and commit the changes:

![](../../images/cicd-k8s/CI-Backend/3a.png)

N.B. the wizard to create the toolchains (below) also requests the name of the kubernetes cluster, but the resulting toolchain environmental property is not used by this sample.  Also note the definition of `SERVICE_KEY_NAME` fields for the AppId and Postgres cloud services.  If you have previously deployed the sample code to serverless, the deployment scripts should have created appropriate API keys.

In folder configuration/global.json, update the values for the `RESOURCE_GROUP` and `REGION` where you created your Kubernetes cluster, and the container registry `NAMESPACE` you created for the container images:  

![](../../images/cicd-k8s/CI-Backend/3b.png)

It is not necessary to change the `IMAGES` section.


### Create the CI Backend toolchain

Login to IBM Cloud and use the hamburger menu in the top left to navigate to `DevOps`.  Select the Resource Group and Region thenh `Create Toolchain` and select the `DevSecOps` filter:

![](../../images/cicd-k8s/CI-Backend/4.png)

Click the `CI` tile to launch the setup wizard, and complete the fields by refering to the following screenshots (refering to your own GitHub repos):

![](../../images/cicd-k8s/CI-Backend/5.png)
![](../../images/cicd-k8s/CI-Backend/6.png)
![](../../images/cicd-k8s/CI-Backend/7.png)
![](../../images/cicd-k8s/CI-Backend/8.png)
![](../../images/cicd-k8s/CI-Backend/9.png)
![](../../images/cicd-k8s/CI-Backend/10.png)
![](../../images/cicd-k8s/CI-Backend/11.png)
![](../../images/cicd-k8s/CI-Backend/12.png)
![](../../images/cicd-k8s/CI-Backend/13.png)
![](../../images/cicd-k8s/CI-Backend/14.png)
![](../../images/cicd-k8s/CI-Backend/15.png)
![](../../images/cicd-k8s/CI-Backend/16.png)
![](../../images/cicd-k8s/CI-Backend/17.png)
![](../../images/cicd-k8s/CI-Backend/18.png)
![](../../images/cicd-k8s/CI-Backend/19.png)


The CI pipeline for backend also requires access to the multi-tenancy repostory, in addition to the multi-tenancy-backend repostory which has already been configured by the wizard.  Click the blue `Add tool` button and manually add the GitHub repostory for multi-tenancy by following these steps:

![](../../images/cicd-k8s/CI-Backend/19a.png)
![](../../images/cicd-k8s/CI-Backend/19b.png)

Add or update the toolchain's Environmental Properties as follows:

![](../../images/cicd-k8s/CI-Backend/20.png)
![](../../images/cicd-k8s/CI-Backend/21.png)

Note that the Text fields `pipeline-config-branch` and `opt-in-sonar` need to be modified.

An additional Text Fields entitled `branch`, `multi-tenancy-frontend`, `multi-tenancy-backend` and `multi-tenancy` were added.

An additional Tool Integration field entitled `repository` was added.  When setting this field, you must specify a JSON filter of `parameters.repo_url`:

![](../../images/cicd-k8s/CI-Backend/21a.png)

Select the `Trigger` tab.  Note the Git CI Trigger shows a hazard symbol.  Edit its properties and select a valid the branch name:

![](../../images/cicd-k8s/CI-Backend/22.png)
![](../../images/cicd-k8s/CI-Backend/23.png)
![](../../images/cicd-k8s/CI-Backend/24.png)

## Create Dev Mode trigger & Update Environmental Properties for CI Frontend

Enable a Dev mode trigger which permits a faster pipeline run which does not invoke all compliance steps.  Select the CI pipeline tile, then Trigger.  Duplicate the existing Manual Trigger and set the properties as follows:

![](../../images/cicd-k8s/CI-Backend/25.png)
![](../../images/cicd-k8s/CI-Backend/26.png)

Add or update the Environmental Properties as follows:

![TBD](../../images/cicd-k8s/CI-Frontend/.png)
![TBD](../../images/cicd-k8s/CI-Frontend/.png)

Note that the Text fields `pipeline-config-branch` and `opt-in-sonar` were updated.

Additional Text Fields entitled `branch`, `multi-tenancy-frontend`, `multi-tenancy-backend` and `multi-tenancy` were added.

An additional Tool Integration field entitled `repository` was added.  When setting this field, you must specify a JSON filter of `parameters.repo_url`:

![TBD](../../images/cicd-k8s/CI-Frontend/.png)

Select the `Trigger` tab.  Note the Git CI Trigger shows a hazard symbol.  Edit its properties and select a valid the branch name:

![TBD](../../images/cicd-k8s/CI-Frontend/22.png)
![TBD](../../images/cicd-k8s/CI-Frontend/23.png)
![TBD](../../images/cicd-k8s/CI-Frontend/24.png)



## Create CI toolchain for frontend

Login to IBM Cloud and use the hamburger menu in the top left to navigate to `DevOps`.  Select the Resource Group and Region thenh `Create Toolchain` and select the `DevSecOps` filter:

![](../../images/cicd-k8s/CI-Backend/4.png)

Click the CI tile to launch the setup wizard, and complete the fields by refering to the following screenshots (refering to your own GitHub repos):

![](../../images/cicd-k8s/CI-Frontend/1.png)
![](../../images/cicd-k8s/CI-Frontend/2.png)
![](../../images/cicd-k8s/CI-Frontend/3.png)
![](../../images/cicd-k8s/CI-Frontend/4.png)
![](../../images/cicd-k8s/CI-Frontend/5.png)
![](../../images/cicd-k8s/CI-Frontend/6.png)
![](../../images/cicd-k8s/CI-Frontend/7.png)
![](../../images/cicd-k8s/CI-Frontend/8.png)
![](../../images/cicd-k8s/CI-Frontend/9.png)
![](../../images/cicd-k8s/CI-Frontend/10.png)
![](../../images/cicd-k8s/CI-Frontend/11.png)
![](../../images/cicd-k8s/CI-Frontend/12.png)
![](../../images/cicd-k8s/CI-Frontend/13.png)
![](../../images/cicd-k8s/CI-Frontend/14.png)
![](../../images/cicd-k8s/CI-Frontend/15.png)

The CI pipeline for frontend also requires access to the multi-tenancy repostory, in addition to the multi-tenancy-backend repostory which has already been configured by the wizard.  Click the blue `Add tool` button and manually add the GitHub repostory for multi-tenancy by following these steps:

![](../../images/cicd-k8s/CI-Frontend/15a.png)
![](../../images/cicd-k8s/CI-Frontend/15b.png)

## Create Dev Mode trigger & Update Environmental Properties for CI Backend

Enable a Dev mode trigger which permits a faster pipeline run which does not invoke all compliance steps.  Select the CI pipeline tile, then Trigger.  Duplicate the existing Manual Trigger and set the properties as follows:

![](../../images/cicd-k8s/CI-Frontend/20.png)
![](../../images/cicd-k8s/CI-Frontend/21.png)


Add or update the toolchain's Environmental Properties as follows:

![](../../images/cicd-k8s/CI-Frontend/16.png)
![](../../images/cicd-k8s/CI-Frontend/17.png)

Note that the Text fields `pipeline-config-branch` and `opt-in-sonar` need to be modified.

An additional Text Fields entitled `branch`, `multi-tenancy-frontend`, `multi-tenancy-backend` and `multi-tenancy` were added.

An additional Tool Integration field entitled `repository` was added.  When setting this field, you must specify a JSON filter of `parameters.repo_url`:

![](../../images/cicd-k8s/CI-Frontend/17a.png)

Select the `Trigger` tab.  Note the Git CI Trigger shows a hazard symbol.  Edit its properties and select a valid the branch name:

![](../../images/cicd-k8s/CI-Frontend/18.png)
![](../../images/cicd-k8s/CI-Frontend/19.png)


## Testing the CI pipelines (Optional)

In this section, you can make a quick test of the CI pipelines.  For a more detailed explanation of how to use the pipelines to onboard a new tenant, see the [Observability]() section.

First test the backend CI pipelines by triggering a pipeline run, using `Manual Trigger`.  Click the pipeline run name to view the progress.  After a few minutes, a successful result should look like this:

![](../../images/cicd-k8s/CI-Backend/27.png)

Test the front end CI pipeline by triggering a pipeline run, using `Manual Trigger`.  Click the pipeline run name to view the progress.  After a few minutes, a successful result should look like this:

![](../../images/cicd-k8s/CI-Frontend/22.png)

If all steps complete successfully, you can configure and test the CD Toolchain.

