# Clean up

Th
### Step 1: Clean-up

The clean-up reuses the **configuration json files** you customized in the [getting started](https://ibm.github.io/multi-tenancy-documentation/serverless-via-ibm-code-engine/ce-setup-create-the-instances/) section.

To delete all created resource you execute following commands in the running `SaaS Container Image`:

```sh
cd $ROOT_FOLDER/installapp
ibmcloud login --sso
sh ./ce-clean-up-two-tenancies.sh
```

The deletion will also ask to review some configurations and press enter to move forward in some steps.

The table contains the scripts and the responsibilities of a script.

| Script | Responsibility |
|---|---|
| [`ce-clean-up-two-tenancies.sh`](https://github.com/IBM/multi-tenancy/blob/main/installapp/ce-clean-up-two-tenancies.sh) | It starts the clean-up for the tenant application instances, therefor it invokes the bash script [`ce-clean-up.sh`](https://github.com/IBM/multi-tenancy/blob/main/installapp/ce-clean-up.sh) twice with the json configuration files as parameters. |
| [`ce-clean-up.sh`](https://github.com/IBM/multi-tenancy/blob/main/installapp/ce-clean-up.sh) | Deletes all created resouce for the two tenants. |