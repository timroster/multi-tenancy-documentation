# Clean up

In this section you will clean up all IBM Cloud resources we created during the [getting started](https://ibm.github.io/multi-tenancy-documentation/serverless-via-ibm-code-engine/ce-setup-create-the-instances/).


> ATTENTION: The `IBM Cloud Code Engine Project Name` and the `IBM Cloud Container Registry Namespace Name` are unique inside an IBM Cloud region. 
If you delete a `Container Registry namespace` or a `Code Engine project` they can't just be recreated with the same name for a specific timeframe, they are now in a `trash` to give you a chance to restore them.

If you want to restore them with the same name you need to follow the steps in the IBM Cloud documentation:

* [IBM Cloud Container Registry - Cleaning up your namespaces](https://cloud.ibm.com/docs/Registry?topic=Registry-registry_retention)
* [IBM Cloud Code Engine `ibmcloud ce reclamation restore`](https://cloud.ibm.com/docs/codeengine?topic=codeengine-cli#cli-reclamation-restore)


>The Clean-Up scipts don't using hard deletion!

### Avoid project and namespace deletion


* Open the `ce-clean-up.sh` file

```sh 
cd $ROOT_FOLDER/installapp
nano ce-clean-up.sh
```

* Comment out following steps `cleanIBMContainerNamespace` and `cleanCodeEngineProject` in the bash script

```sh
# To avoid the deletion of the Container Registry Namespace 
# please comment out the `cleanIBMContainerNamespace`
cleanIBMContainerNamespace
...

# To avoid the deletion of the Code Engine project 
# please comment out the `cleanCodeEngineProject`
cleanCodeEngineProject
```

### Clean-up

The clean-up reuses the **configuration json files** you customized in the [getting started](https://ibm.github.io/multi-tenancy-documentation/serverless-via-ibm-code-engine/ce-setup-create-the-instances/) section.

To delete all created resources you execute following commands in the running `SaaS Container Image`:

#### Step 1: Ensure you are in the `installapp` folder:

```sh
cd $ROOT_FOLDER/installapp
```

#### Step 2: Ensure you are in the `logged on o IBM Cloud`:

```sh
ibmcloud login --sso
```

#### Step 3: Start the deletion with the execution of the `ce-clean-up-two-tenancies.sh` script:

```sh
sh ./ce-clean-up-two-tenancies.sh
```

### The review steps during bash automation

The deletion will also ask to review some configurations and press enter to move forward in some steps.


### Details to understand the autmation

The table contains the scripts and the responsibilities of a script.

| Script | Responsibility |
|---|---|
| [`ce-clean-up-two-tenancies.sh`](https://github.com/IBM/multi-tenancy/blob/main/installapp/ce-clean-up-two-tenancies.sh) | It starts the clean-up for the tenant application instances, therefor it invokes the bash script [`ce-clean-up.sh`](https://github.com/IBM/multi-tenancy/blob/main/installapp/ce-clean-up.sh) twice with the json configuration files as parameters. |
| [`ce-clean-up.sh`](https://github.com/IBM/multi-tenancy/blob/main/installapp/ce-clean-up.sh) | Deletes all created resouce for the two tenants. |