# Getting started with the example application

> Create the tenants with the related IBM Cloud services

The getting setup is **only** for the **serverless** part with **Code Engine**!

The is objective to provide you a getting started to basic understanding of the achiteture of the sample application with running instances.

The image shows simplified what we are doing during the `Getting Started`.

![](../images/initial_automated_setup_for_serverless/multi-Tenancy-installation.png)


Here is an additional overview of tasks we automated for you with the getting started related to the diagram above.

* Create Code Engine projects

   * Configure secret to access the `IBM Cloud Container Registry`

   * Create secrets for the applications

   * Configure the environment variables used by the applications

       * App ID configurations

        * PostgresSQL database

       * Backend endpoint

* Build the container images and push them to the `IBM Cloud Container Registry`

* Create a PostgresSQL database service

    * Setup example data for each tenant

* Create an App ID service

    * Configure the service

### 
### Prerequisites  

* OS: `macOS` 
* Container tool: [`podman`](https://podman.io/getting-started)

### `SaaS-Tools` image

For the simplification of the getting started we provide you a [SaaS-Tools container image](https://quay.io/repository/tsuedbroecker/saas-tools?tab=info) that contains all needed commandline tools for the automation with the bash scripts.
#### Step 1: Open a terminal and start the `SaaS-Tools` image

```sh
podman run -it --rm  --privileged --name saas-tools "quay.io/tsuedbroecker/saas-tools:v1"    
```

* Example output:

```sh
root@7f535f968abc:/# 
```

### Using the `SaaS-Tools` image

From now we will only work inside the running `SaaS-Tools` container image.

#### Step 1: Open the home directory

```sh
cd home
```

#### Step 2: Clone the repositories into the home directory

```sh
git clone https://github.com/IBM/multi-tenancy 
git clone https://github.com/IBM/multi-tenancy-backend
git clone https://github.com/IBM/multi-tenancy-frontend && cd multi-tenancy
ROOT_FOLDER=$(pwd)
```

#### Step 3 : Verify the prerequisites for running the installation

```sh
cd $ROOT_FOLDER/installapp
bash ./ce-check-prerequisites.sh
```

The script stops when it notices any prerequisite is missing.

Example output:

```sh
Check prereqisites
...
Success! All prerequisites verified!
```

These are the tools we installed for you in the `SaaS-Tools` image:

* [ibmcloud cli](https://cloud.ibm.com/docs/cli?topic=cli-install-ibmcloud-cli)
* [ibmcloud plugin code-engine](https://cloud.ibm.com/docs/codeengine?topic=codeengine-install-cli)
* [ibmcloud plugin cloud-databases](https://cloud.ibm.com/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference)
* [ibmcloud plugin container-registry](https://cloud.ibm.com/docs/Registry?topic=container-registry-cli-plugin-containerregcli)
* [buildah](https://buildah.io/)
* [sed](https://en.wikipedia.org/wiki/Sed)
* [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/)
* [jq](https://lzone.de/cheat-sheet/jq)
* [grep](https://en.wikipedia.org/wiki/Grep)
* [libpq (psql)](https://www.postgresql.org/docs/9.5/libpq.html) 
* [cURL](https://curl.se/)
* [AWK](https://en.wikipedia.org/wiki/AWK)

### Define the needed configurations for the tenants

During the getting started we will setup two tenants of the sample application.
Therefor we need to configure two kinds of configuration files.

* Global configuration

Define the global configuration in [global.json](https://github.com/IBM/multi-tenancy/blob/main/configuration/global.json). It includes `IBM Cloud settings` such as region and resource group, `container registry information` and `image information`.

* Tenant-specific configuration

For each tenant define tenant-specific configuration in the folder 'configuration/tenants'. That configuration contains for example `App ID information`. `Postgres database information`, `application instance information`, and `Code Engine information`. Here you find an example configuration [tenant-a.json](https://github.com/IBM/multi-tenancy/blob/main/installapp/tenants-config/tenants/tenant-a.json).

#### Step 1: Configure `IBM Cloud Container Registry Namespace` in the global configuration

>The values for the names for a `IBM Cloud Container Registry Namespace` must unique in IBM Cloud for a region! To avoid problems during running the setup, please configure that name to your needs. **Don't change one of the other default values, if you do not known what you are going to change.**

Open the `globle.json`

```sh
nano ./configuration/global.json
```

Replace the value for the namespace with a value of your choose:
`"NAMESPACE":"multi-tenancy-example` to `"NAMESPACE":"YOUR_VALUE"`.

```json
{
  "IBM_CLOUD": {
    "RESOURCE_GROUP": "default",
    "REGION": "eu-de"
  },
  "REGISTRY": {
    "URL": "de.icr.io",
    "NAMESPACE": "multi-tenancy-example", #<- INSERT YOUR VALUE
    "TAG": "v2",
    "SECRET_NAME": "multi.tenancy.cr.sec"
  },
  "IMAGES": {
    "NAME_BACKEND": "multi-tenancy-service-backend",
    "NAME_FRONTEND": "multi-tenancy-service-frontend"
  }
}
```


#### Step 1: Configure  the `Code Engine project name` for each tenant

>The values for the names for a `IBM Cloud Code Engine` project must unique in IBM Cloud for a region! To avoid problems during running the setup, please configure these names to your needs. **Don't change one of the other default values, if you do not known what you are going to change.**

#### 1. Unique value in the _globel_ configuration

* Configure your `IBM Cloud Container Registry Namespace name`

In the [global.json](https://github.com/IBM/multi-tenancy/blob/main/configuration/global.json) file you need to change the value for the IBM Cloud Container Registry Namespace name to something like `multi-tenancy-example-mypostfix`.

```json
  "REGISTRY": {
    "URL": "de.icr.io",
    "NAMESPACE": "multi-tenancy-example-[YOUR_POSTFIX]",
    "SECRET_NAME": "multi.tenancy.cr.sec",
    "TAG": "v2"
  },
```

#### 2. Unique value in the _tenant-specific_ configuration

Configure your `Code Engine project names` for the two tenants

In the [tenant-a.json](https://github.com/IBM/multi-tenancy/blob/main/configuration/tenants/tenant-a.json) files you need to change the value for the `Code Engine project` to something like `multi-tenancy-example-mypostfix`.

Open the first tenant configuration `tenant-a.json`

```sh
nano ./configuration/tenants/tenant-a.json
```

Replace the value for the project name of the Code Engine project to one of your choise:
`"PROJECT_NAME":"multi-tenancy-serverless-a` to `"PROJECT_NAME":"YOUR_VALUE"`.

```json
{
  "APP_ID": {
    "SERVICE_INSTANCE": "multi-tenancy-serverless-appid-a",
    "SERVICE_KEY_NAME": "multi-tenancy-serverless-appid-key-a"
  },
  "POSTGRES": {
    "SERVICE_INSTANCE": "multi-tenancy-serverless-pg-ten-a",
    "SERVICE_KEY_NAME": "multi-tenancy-serverless-pg-ten-a-key",
    "SQL_FILE": "create-populate-tenant-a.sql"
  },
  "APPLICATION": {
    "CONTAINER_NAME_BACKEND": "multi-tenancy-service-backend-movies",
    "CONTAINER_NAME_FRONTEND": "multi-tenancy-service-frontend-movies",
    "CATEGORY": "Movies"
  },
  "CODE_ENGINE": {
    "PROJECT_NAME": "multi-tenancy-serverless-a-t" <- INSERT YOUR VALUE
  },
  "IBM_KUBERNETES_SERVICE": {
    "NAME": "niklas-heidloff3-fra04-b3c.4x16",
    "NAMESPACE": "tenant-a"
  },
  "IBM_OPENSHIFT_SERVICE": {
    "NAME": "roks-gen2-suedbro",
    "NAMESPACE": "tenant-a"
  },
  "PLATFORM": {
    "NAME": "IBM_OPENSHIFT_SERVICE"
  }
}
```

### Start the bash script automation

### Step 1: Open the folder for the `gettings started installation`

```sh
cd $ROOT_FOLDER/installapp
```

### Step 2: Log on with you IBM Cloud account

```sh
ibmcloud login --sso
```

### Step 3: Start the bash automation with following command

```sh
bash ./ce-create-two-tenancies.sh
```


![](../images/initial_automated_setup_for_serverless/Multi-Tenancy-automatic-running-example-04.gif)

The execution takes roughly 30 minutes. 
You will be asked to review some configurations and press enter to move forward in some steps.
The script will stop in some situations when it discovers a problem during the setup.

After this the URL of the frontend applications will be displayed. 

For both tenants the following test user can be used to log in:

* User:` thomas@example.com`#

* Password: `thomas4appid`

### Details to understand the autmation

Here are details related to the used bash scripts.

We use three bash scripts for the initial installation. The following diagram shows the simplified dependencies of these bash scripts used to create two tenants of the example application on IBM Cloud in Code Engine.

![](../images/initial_automated_setup_for_serverless/simplified-overview-bash-scripts-installation.png)

The scripts creating two tenants:

* Two Code Engine projects with two applications one frontend and one backend.
* Two App ID instance to provide a basic authentication and authorization for the two tenants.
* Two Postgres databases for the two tenants.

The table contains the script and the responsibility of the scripts.

| Script | Responsibility |
|---|---|
| [`ce-create-two-tenancies.sh`](https://github.com/IBM/multi-tenancy/blob/main/installapp/ce-create-two-tenancies.sh) | Build the container images for the frontend and backend, therefor it invokes the bash script [`ce-build-images-ibm-docker.sh`](https://github.com/IBM/multi-tenancy/blob/main/installapp/ce-build-images-ibm-docker.sh) and uploads the images to the IBM Cloud container registry. It also starts the creation of the tenant application instance, therefor it invokes the bash script [`ce-install-application.sh`](https://github.com/IBM/multi-tenancy/blob/main/installapp/ce-install-application.sh) twice. |
| [`ce-build-images-ibm-docker.sh`](https://github.com/IBM/multi-tenancy/blob/main/installapp/ce-build-images-ibm-docker.sh) | Creates two container images based on the given parameters for the backend and frontend image names. |
| [`ce-install-application.sh`](https://github.com/IBM/multi-tenancy/blob/main/installapp/ce-install-application.sh) | Creates and configures a `Code Engine project`. The configuration of the Code Engine project includes the `creation of the application`, the `IBM Cloud Container Registry access` therefor it also creates a `IBM Cloud API` and it creates the `secrets` for the needed parameter for the running applications. It creates an `IBM Cloud App ID instance` and configures this instance that includes the `application`, `redirects`, `login layout`, `scope`, `role` and `user`. It also creates an `IBM Cloud Postgres` database instance and creates the needed example data with tables inside the database. |
