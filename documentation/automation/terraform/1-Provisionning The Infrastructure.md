# Provisionning The Infrastructure

This document describes how to provision the back-end infrastructure for your project.

The infrastructure consists on;

- A "Virtual Private Cloud" (VPC) on IBM Cloud.

- An **OpenShift** cluster inside the VPC with all the requirements.

- A Database for PostgreSQL managed service.

- An instance of AppID service.

  

## Setting up the infrastructure

The steps to follow to provision the infrastrure are,

- Open a terminal window in the current directory where the code reside.
- Log into IBM Cloud

```bash
ibmcloud login --sso
```

- Make a copy of "credentials.template" file and name it as "credential.properties".	

```shell
cp credentials.template credential.properties
```

- Edit the "credentials.properties" file and put your IBM Cloud Api Key.

```properties
# Add the values for the Credentials to access the IBM Cloud
# Instructions to access this information can be found in the README.MD
# This is a template file and the ./launch.sh script looks for a file based on this template named credentials.properties
ibmcloud.api.key="your-api-key"
```

Save and exit the file.

- Open a terminal window and run the following command:


```shell
./apply-all.sh`
```

