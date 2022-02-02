# Clean up
### Step 1: Clean-up

The clean-up reuses the configuration json files you defined for the setup/installatation.

To delete all created resource you execute following commands:

```sh
cd $ROOT_FOLDER/installapp
ibmcloud login --sso
sh ./ce-clean-up-two-tenancies.sh
```

The table contains the script and the responsibility of the scripts.

| Script | Responsibility |
|---|---|
| [`ce-clean-up-two-tenancies.sh`](https://github.com/IBM/multi-tenancy/blob/main/installapp/ce-clean-up-two-tenancies.sh) | It starts the clean-up for the tenant application instances, therefor it invokes the bash script [`ce-clean-up.sh`](https://github.com/IBM/multi-tenancy/blob/main/installapp/ce-clean-up.sh) twice with the json configuration files as parameters. |
| [`ce-clean-up.sh`](https://github.com/IBM/multi-tenancy/blob/main/installapp/ce-clean-up.sh) | Deletes all created resouce for the two tenants. |