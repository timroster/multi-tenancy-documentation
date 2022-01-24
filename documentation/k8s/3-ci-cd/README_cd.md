# Create DevSecOps pipelines to update a Kubernetes application 

DevSecOps integrates a set of security and compliance controls into the DevOps processes, allowing frequent code delivery while maintaining a strong security posture and continuous state of audit-readiness.

This setion steps you through the creation of Continuous Integration (CD) toolchains using an IBM Cloud toolchain templates, which we will customize to deploy the sample application to an IBM Cloud Kubernetes cluster.


## Before you begin

Set up the following pre-requisites:

- Ensure you have cloned the following repos to your GitHub account:

    https://github.com/IBM/multi-tenancy
    https://github.com/IBM/multi-tenancy-backend
    https://github.com/IBM/multi-tenancy-frontend

- An instance of the IBM Cloud Secrets Manager service (the same one you used for CI pipelines)
- An IBM Cloud Kubernetes Service cluster or an IBM Cloud OpenShift cluster (the same one you used for CI pipelines)
- Create a namespace in the IBM Cloud Container Registry (the same one you used for CI pipelines)

## Create the toolchain

Click `Create Toolchain` and select the `DevSecOps` filter:

![](/documentation/images/cicd-k8s/CI-Backend/4.png)

## Create CD toolchain

Click the `CD` tile to launch the setup wizard, and complete the fields by refering to the following screenshots (refering to your own GitHub repos):

![](/documentation/images/cicd-k8s/CD/1.png)
![](/documentation/images/cicd-k8s/CD/2.png)
![](/documentation/images/cicd-k8s/CD/3.png)
![](/documentation/images/cicd-k8s/CD/4.png)
![](/documentation/images/cicd-k8s/CD/5.png)
![](/documentation/images/cicd-k8s/CD/6.png)
![](/documentation/images/cicd-k8s/CD/7.png)
![](/documentation/images/cicd-k8s/CD/8.png)
![](/documentation/images/cicd-k8s/CD/9.png)
![](/documentation/images/cicd-k8s/CD/10.png)
![](/documentation/images/cicd-k8s/CD/11.png)
![](/documentation/images/cicd-k8s/CD/12.png)
![](/documentation/images/cicd-k8s/CD/13.png)
![](/documentation/images/cicd-k8s/CD/14.png)
![](/documentation/images/cicd-k8s/CD/15.png)
![](/documentation/images/cicd-k8s/CD/16.png)
![](/documentation/images/cicd-k8s/CD/17.png)


## Create Dev Mode trigger & Update Environmental Properties for CD

The environmental properties for the frontend CD pipeline should be updated as follows:

Add or update the Environmental Properties so they look like thise (using your own GitHub multi-tenancy/backend/frontend repos):

![](/documentation/images/cicd-k8s/CD/18.png)
![](/documentation/images/cicd-k8s/CD/19.png)

Note that the Text fields `pipeline-config-branch` and `target-environment` were updated.

Additional Text Fields entitled `branch`, `multi-tenancy-frontend`, `multi-tenancy-backend`, `multi-tenancy` and `tenant` were added.

An additional Tool Integration field entitled `repository` was added.  When setting this field, you must specify a JSON filter of `parameters.repo_url`:

![](/documentation/images/cicd-k8s/CD/.png)

Select the `Trigger` tab.  You may need to correct the Git CD Trigger if it shows a hazard symbol:

![](/documentation/images/cicd-k8s/CD/20.png)

Edit its properties and select a valid the branch name:

![](/documentation/images/cicd-k8s/CD/21.png)
![](/documentation/images/cicd-k8s/CD/22.png)

Also, copy the existing `Manual CD Trigger` and use it to create a `Manual CD Trigger force redeploy` trigger which can be used to bypass some elements of the toolchain, for testing purposes.  Note that two properties have been added:

![](/documentation/images/cicd-k8s/CD/23.png)
![](/documentation/images/cicd-k8s/CD/24.png)
![](/documentation/images/cicd-k8s/CD/25.png)

## Testing the CD pipelines

If you have successfully executed the CI pipelines for backend and frontend, you can test the CD pipeline.

First you'll need to execute the `Manual Promotion Trigger`.  This creates a new branch and a merge request in the Inventory Repository.  You should approve that merge request before proceeding.

Finally you can trigger the pipeline with a `Manual CD Trigger'.  Click the pipeline run name to view the progress.  After a few minutes, a successful result should look like this:

![](/documentation/images/cicd-k8s/CD/.png)

In the logs for the `prod-deployment` `run-stage` you will find two outputs starting `Application URL:`.  These are the URLs to test the backend and frontend applications.  Give them a try!

![](/documentation/images/cicd-k8s/CD/.png)
