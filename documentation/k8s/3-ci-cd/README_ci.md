# Create DevSecOps pipelines to update a Kubernetes application 

DevSecOps integrates a set of security and compliance controls into the DevOps processes, allowing frequent code delivery while maintaining a strong security posture and continuous state of audit-readiness.

This setion steps you through the creation of Continuous Integration (CI) toolchains using an IBM Cloud toolchain templates, which we will customize to deploy the sample application to an IBM Cloud Kuberenets cluster.

## Before you begin

Set up the following pre-requisites:

- Ensure you have cloned the following repos to your GitHub account:

    https://github.com/IBM/multi-tenancy
    https://github.com/IBM/multi-tenancy-backend
    https://github.com/IBM/multi-tenancy-frontend

- An instance of the IBM Cloud Secrets Manager service
- An IBM Cloud Kubernetes Service cluster or An IBM Cloud OpenShift cluster
- Create a namespace in the IBM Cloud Container Registry (access via hamburger menu->Container Registry)


## Configure branch protection for your GitHub repositories

In GitHub, go to the settings for your multi-tenancy-backend repo, select Branches and set Branch Protection Rules for the main branch as follows:

![](/documentation/images/cicd-k8s/CI-Backend/1.png)
![](/documentation/images/cicd-k8s/CI-Backend/1.png)


## Create an IBM Cloud API key in Secrets Manager

In IBM Cloud, navigate to Manage->Access (IAM)->API Keys.  Create a new IBM Cloud API Key and make a note of it.

In your Secrets Manager service, create a new Arbitary value and paste in the IBM Cloud API Key:

![](/documentation/images/cicd-k8s/CI-Backend/3.png)

## Create the CI Backend toolchain

Login to IBM Cloud and use the hamburger menu in the top left to navigate to `DevOps`.  Select the Resource Group and Region thenh `Create Toolchain` and select the `DevSecOps` filter:

![](/documentation/images/cicd-k8s/CI-Backend/4.png)

Click the CI tile to launch the setup wizard, and complete the fields by refering to the following screenshots (refering to your own GitHub repos):

![](/documentation/images/cicd-k8s/CI-Backend/5.png)
![](/documentation/images/cicd-k8s/CI-Backend/6.png)
![](/documentation/images/cicd-k8s/CI-Backend/7.png)
![](/documentation/images/cicd-k8s/CI-Backend/8.png)
![](/documentation/images/cicd-k8s/CI-Backend/9.png)
![](/documentation/images/cicd-k8s/CI-Backend/10.png)
![](/documentation/images/cicd-k8s/CI-Backend/11.png)
![](/documentation/images/cicd-k8s/CI-Backend/12.png)
![](/documentation/images/cicd-k8s/CI-Backend/13.png)
![](/documentation/images/cicd-k8s/CI-Backend/14.png)
![](/documentation/images/cicd-k8s/CI-Backend/15.png)
![](/documentation/images/cicd-k8s/CI-Backend/16.png)
![](/documentation/images/cicd-k8s/CI-Backend/17.png)
![](/documentation/images/cicd-k8s/CI-Backend/18.png)
![](/documentation/images/cicd-k8s/CI-Backend/19.png)


Add or update the Environmental Properties as follows:

![](/documentation/images/cicd-k8s/CI-Backend/20.png)
![](/documentation/images/cicd-k8s/CI-Backend/21.png)

Note that the Text fields `pipeline-config-branch` and `opt-in-sonar` need to be modified.

An additional Text Fields entitled `branch`, `multi-tenancy-frontend`, `multi-tenancy-backend` and `multi-tenancy` were added.

An additional Tool Integration field entitled `repository` was added.  When setting this field, you must specify a JSON filter of `parameters.repo_url`:

![](/documentation/images/cicd-k8s/CI-Backend/21a.png)

Select the `Trigger` tab.  Note the Git CI Trigger shows a hazard symbol.  Edit its properties and select a valid the branch name:

![](/documentation/images/cicd-k8s/CI-Backend/22.png)
![](/documentation/images/cicd-k8s/CI-Backend/23.png)
![](/documentation/images/cicd-k8s/CI-Backend/24.png)

## Create Dev Mode trigger for CI Backend

enable a Dev mode trigger which permits a faster pipeline run which does not invoke all compliance steps.  Select the CI pipeline tile, then Trigger.  Duplicate the existing Manual Trigger and set the properties as follows:

![](/documentation/images/cicd-k8s/CI-Backend/25.png)
![](/documentation/images/cicd-k8s/CI-Backend/26.png)

Add or update the Environmental Properties as follows:

![](/documentation/images/cicd-k8s/CI-Frontend/.png)
![](/documentation/images/cicd-k8s/CI-Frontend/.png)

Note that the Text fields `pipeline-config-branch` and `opt-in-sonar` need to be modified.

An additional Text Fields entitled `branch`, `multi-tenancy-frontend`, `multi-tenancy-backend` and `multi-tenancy` were added.

An additional Tool Integration field entitled `repository` was added.  When setting this field, you must specify a JSON filter of `parameters.repo_url`:

![](/documentation/images/cicd-k8s/CI-Frontend/.png)

Select the `Trigger` tab.  Note the Git CI Trigger shows a hazard symbol.  Edit its properties and select a valid the branch name:

![](/documentation/images/cicd-k8s/CI-Frontend/22.png)
![](/documentation/images/cicd-k8s/CI-Frontend/23.png)
![](/documentation/images/cicd-k8s/CI-Frontend/24.png)



## Create CI toolchain for frontend

Login to IBM Cloud and use the hamburger menu in the top left to navigate to `DevOps`.  Select the Resource Group and Region thenh `Create Toolchain` and select the `DevSecOps` filter:

![](/documentation/images/cicd-k8s/CI-Backend/4.png)

Click the CI tile to launch the setup wizard, and complete the fields by refering to the following screenshots (refering to your own GitHub repos):

![](/documentation/images/cicd-k8s/CI-Frontend/1.png)
![](/documentation/images/cicd-k8s/CI-Frontend/2.png)
![](/documentation/images/cicd-k8s/CI-Frontend/3.png)
![](/documentation/images/cicd-k8s/CI-Frontend/4.png)
![](/documentation/images/cicd-k8s/CI-Frontend/5.png)
![](/documentation/images/cicd-k8s/CI-Frontend/6.png)
![](/documentation/images/cicd-k8s/CI-Frontend/7.png)
![](/documentation/images/cicd-k8s/CI-Frontend/8.png)
![](/documentation/images/cicd-k8s/CI-Frontend/9.png)
![](/documentation/images/cicd-k8s/CI-Frontend/10.png)
![](/documentation/images/cicd-k8s/CI-Frontend/11.png)
![](/documentation/images/cicd-k8s/CI-Frontend/12.png)
![](/documentation/images/cicd-k8s/CI-Frontend/13.png)
![](/documentation/images/cicd-k8s/CI-Frontend/14.png)
![](/documentation/images/cicd-k8s/CI-Frontend/15.png)

Add or update the Environmental Properties as follows:

![](/documentation/images/cicd-k8s/CI-Frontend/16.png)
![](/documentation/images/cicd-k8s/CI-Frontend/17.png)

Note that the Text fields `pipeline-config-branch` and `opt-in-sonar` need to be modified.

An additional Text Fields entitled `branch`, `multi-tenancy-frontend`, `multi-tenancy-backend` and `multi-tenancy` were added.

An additional Tool Integration field entitled `repository` was added.  When setting this field, you must specify a JSON filter of `parameters.repo_url`:

![](/documentation/images/cicd-k8s/CI-Frontend/17a.png)

Select the `Trigger` tab.  Note the Git CI Trigger shows a hazard symbol.  Edit its properties and select a valid the branch name:

![](/documentation/images/cicd-k8s/CI-Frontend/18.png)
![](/documentation/images/cicd-k8s/CI-Frontend/19.png)


## Testing the CI pipelines


First test the backend CI pipelines by triggering a pipeline run, using `Dev Mode`:

Click the pipeline run name to view the progress.  After a few minutes, a successful result should look like this:

Test the front end CI pipeline by triggering a pipeline run, using `Dev Mode`:

Click the pipeline run name to view the progress.  After a few minutes, a successful result should look like this:

Next test boths CI pipelines with `Manual Trigger`.  If all steps complete successfully, you can configure and test the CD Toolchain.

