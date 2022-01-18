# Provisionning The Minimal Infrastructure

This document describes how to provision the back-end infrastructure for your project.

As a prerequisite, Terraform should be installed on the machine used for the following operations.

The infrastructure consists on;

- A "Virtual Private Cloud" (VPC) on IBM Cloud.
- An OpenShift cluster inside the VPC with all the requirements.

## Setting up the infrastructure

The steps to follow to provision the infrastrure are,

- Clone the following Github repo;

  ```shell
  git clone https://github.com/cloud-native-toolkit/iascable`
  ```

- Go to the cloned folder;

  ```shell
  cd iascable
  ```

- Install the required modules and packages

```shell
npm install and npm run build
```

- Create the examples/baseline-openshift.yaml Yaml file;

  ```yaml
  apiVersion: cloud.ibm.com/v1alpha1
  kind: BillOfMaterial
  metadata:
   name: baseline-openshift
  spec:
   modules:
  
    - name: ibm-resource-group
    - name: ibm-vpc
    - name: ibm-vpc-gateways
    - name: ibm-vpc-subnets
      alias: cluster-subnets
      variables:
      - name: subnet_count
        value: 1
      - name: subnet_label
        value: cluster
    - name: ibm-ocp-vpc
      dependencies:
      - name: subnets
        ref: cluster-subnets
  ```

  

- Run the following command from the original folder;

```shell
./iascable build -i ./examples/baseline-openshift.yaml
```

- Edit the "baseline-openshift.auto.tfvars" and enter values for the following parameters;
  - resource_group_name
  - ibmcloud_api_key
  - region
  - name_prefix 
  - namespace_name

```properties
## resource_group_name: The name of the resource group
resource_group_name="your-resource-group-name"

## region: The IBM Cloud region where the cluster will be/has been installed.
region="eu-de" (or other IBM Cloud regions)

## ibmcloud_api_key: The IBM Cloud api token
ibmcloud_api_key="your-ibm-cloud-api-key"

## namespace_name: The namespace that should be created
namespace_name="your-namesspace-name"
```

​	Save and quit the file.

- Edit the "credential.properties" file and complete it as the following;

  ```properties
  # Add the values for the Credentials to access the IBM Cloud
  # Instructions to access this information can be found in the README.MD
  classic.username="your-ibm-cloud-account-ID"
  classic.api.key="your-ibm-cloud-classic-api-key"
  ibmcloud.api.key="your-ibm-cloud-api-key"
  
  # Authentication to OCP can either be performed with username/password or token
  # If token is provided it will take precedence
  login.user=""
  login.password=""
  login.token=""
  server.url=""
  ```

  Save and quit the file.

- Open a terminal window and run the following command:


```shell
    cd output/baseline-openshift/terraform 
    terraform init
    terraform plan
    terraform apply
```



