# Provisionning A Kuberntese Infrastructure

This document describes how to provision the back-end infrastructure for your project.

As a prerequisite, Terraform should be installed on the machine used for the following operations.

The infrastructure consists on;

- A "Virtual Private Cloud" (VPC) on IBM Cloud.
- Either an OpenShift or an IKS cluster inside the VPC with all the requirements (e.g.: subnets, cidr...).
- A Database for PostgreSQL managed service (to be implemented).
- An instance of AppID service (to be implemented).

## Setting up the infrastructure - OpenShift Cluster

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

- Log into IBM Cloud

  ```bash
  ibmcloud login --sso
  ```

- Run the following command from the original folder;

  ```shell
  ./iascable build -i ./examples/baseline-openshift.yaml
  ```

  

- Edit the "/iascable/output/baseline-openshift.auto.tfvars" and enter values for the following parameters;
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

- Open a terminal window and run the following commands:


```shell
    cd output/baseline-openshift/terraform 
    terraform init
    terraform plan
    terraform apply
```



## Setting up the infrastructure - IKS (IBM Kubernetes Services) Cluster

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

- Create the examples/baseline-iks.yaml Yaml file;

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

- Log into IBM Cloud

  ```bash
  ibmcloud login --sso
  ```

- Run the following command from the original folder;

  ```shell
  ./iascable build -i ./examples/baseline-iks.yaml
  ```


- Edit the "/iascable/output/baseline-iks.auto.tfvars" and enter values for the following parameters;
  - resource_group_name
  - ibmcloud_api_key
  - region
  
  ```properties
  ## resource_group_name: The name of the resource group
  resource_group_name="your-resource-group-name"
  ## region: The IBM Cloud region where the cluster will be/has been installed.
  region="eu-de" (or other IBM Cloud regions)
  ## ibmcloud_api_key: The IBM Cloud api token
  ibmcloud_api_key="your-ibm-cloud-api-key"
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

- Open a terminal window and run the following commands:


  ```shell
      cd output/baseline-openshift/terraform 
      terraform init
      terraform plan
      terraform apply
  ```




## Changing some default variables



### Number of Worker Pools and Worker Nodes

In both OpenShift and IKS, the defaut configuration provides a cluster with 3 worker pools and 3 worker nodes per pool. If a smaller cluster is needed, for **OpenShift** the default value can be changed in sub folder "../baseline-openshift/terraform/variables.tf" through the "worker-count" variable.

 

```properties
variable "worker_count" {
  type = number
  description = "The number of worker nodes that should be provisioned for classic infrastructure"
  default = 3
}
```

For **IKS** the default value can be changed in sub folder "../baseline-iks/terraform/variables.tf" through the "worker-count" same variable.

### Changing the default cluster flavor

For both OCP/IKS, the default values for the cluster falvor are set in the same "variables.tf" as mentioned above. For a change of the cluster flavor, modify the  "cluster_flavor" variable (https://cloud.ibm.com/docs/containers?topic=containers-clusters).



```properties
variable "cluster_flavor" {
  type = string
  description = "The machine type that will be provisioned for classic infrastructure"
  default = "bx2.4x16"
}
```

To obtain the list of available flavors for IKS clusters proceed as follows;

```shell
ibmcloud login (or ibmcloud login --sso if not done already)

ibmcloud target -g <resource_group_name>

ibmcloud ks locations

Example output:
❯ ibmcloud ks locations
VPC Infrastructure Zones

Zone         Metro                 Country               Geography   
eu-gb-3      London (lon)          United Kingdom (uk)   Europe (eu)   
jp-tok-2     Tokyo (tok)           Japan (jp)            Asia Pacific (ap)   
eu-de-1      Frankfurt (fra)       Germany (de)          Europe (eu)   
us-east-3    Washington DC (wdc)   United States (us)    North America (na)   
ca-tor-3     Toronto (tor)         Canada (ca)           North America (na)   
br-sao-3     Sao Paulo (sao)       Brazil (br)           South America (sa)   
ca-tor-2     Toronto (tor)         Canada (ca)           North America (na)   
jp-osa-2     Osaka (osa)           Japan (jp)            Asia Pacific (ap)   
us-east-2    Washington DC (wdc)   United States (us)    North America (na)   
au-syd-3     Sydney (syd)          Australia (au)        Asia Pacific (ap)   
au-syd-2     Sydney (syd)          Australia (au)        Asia Pacific (ap)   
jp-osa-1     Osaka (osa)           Japan (jp)            Asia Pacific (ap)   
br-sao-2     Sao Paulo (sao)       Brazil (br)           South America (sa)   
eu-gb-1      London (lon)          United Kingdom (uk)   Europe (eu)   
eu-de-3      Frankfurt (fra)       Germany (de)          Europe (eu)   
jp-tok-3     Tokyo (tok)           Japan (jp)            Asia Pacific (ap)   
au-syd-1     Sydney (syd)          Australia (au)        Asia Pacific (ap)   
br-sao-1     Sao Paulo (sao)       Brazil (br)           South America (sa)   
jp-osa-3     Osaka (osa)           Japan (jp)            Asia Pacific (ap)   
us-south-2   Dallas (dal)          United States (us)    North America (na)   
jp-tok-1     Tokyo (tok)           Japan (jp)            Asia Pacific (ap)   
ca-tor-1     Toronto (tor)         Canada (ca)           North America (na)   
eu-gb-2      London (lon)          United Kingdom (uk)   Europe (eu)   
us-east-1    Washington DC (wdc)   United States (us)    North America (na)   
us-south-1   Dallas (dal)          United States (us)    North America (na)   
us-south-3   Dallas (dal)          United States (us)    North America (na)   
eu-de-2      Frankfurt (fra)       Germany (de)          Europe (eu)   

Classic Infrastructure Zones

Zone    Metro                   Country               Geography   
mil01   Milan (mil)             Italy (it)            Europe (eu)   
osl01   Oslo (osl)              Norway (no)           Europe (eu)   
osa23   Osaka (osa)†            Japan (jp)            Asia Pacific (ap)   
lon06   London (lon)†           United Kingdom (uk)   Europe (eu)   
lon02   London (lon)†           United Kingdom (uk)   Europe (eu)   
che01   Chennai (che)           India (in)            Asia Pacific (ap)   
lon04   London (lon)†           United Kingdom (uk)   Europe (eu)   
seo01   Seoul (seo)             Korea (kr)            Asia Pacific (ap)   
dal12   Dallas (dal)†           United States (us)    North America (na)   
dal10   Dallas (dal)†           United States (us)    North America (na)   
wdc04   Washington DC (wdc)†    United States (us)    North America (na)   
osa21   Osaka (osa)†            Japan (jp)            Asia Pacific (ap)   
osa22   Osaka (osa)†            Japan (jp)            Asia Pacific (ap)   
sjc03   San Jose (sjc)          United States (us)    North America (na)   
mex01   Mexico City (mex-cty)   Mexico (mex)          North America (na)   
syd04   Sydney (syd)†           Australia (au)        Asia Pacific (ap)   
hkg02   Hong Kong (hkg-mtr)     Hong Kong (hkg)       Asia Pacific (ap)   
mon01   Montreal (mon)          Canada (ca)           North America (na)   
tok04   Tokyo (tok)†            Japan (jp)            Asia Pacific (ap)   
par01   Paris (par)             France (fr)           Europe (eu)   
syd01   Sydney (syd)†           Australia (au)        Asia Pacific (ap)   
wdc07   Washington DC (wdc)†    United States (us)    North America (na)   
ams03   Amsterdam (ams)         Netherlands (nl)      Europe (eu)   
fra04   Frankfurt (fra)†        Germany (de)          Europe (eu)   
tor01   Toronto (tor)           Canada (ca)           North America (na)   
fra05   Frankfurt (fra)†        Germany (de)          Europe (eu)   
sjc04   San Jose (sjc)          United States (us)    North America (na)   
tok02   Tokyo (tok)†            Japan (jp)            Asia Pacific (ap)   
hou02   Houston (hou)           United States (us)    North America (na)   
sao01   Sao Paulo (sao)         Brazil (br)           South America (sa)   
lon05   London (lon)†           United Kingdom (uk)   Europe (eu)   
tok05   Tokyo (tok)†            Japan (jp)            Asia Pacific (ap)   
wdc06   Washington DC (wdc)†    United States (us)    North America (na)   
syd05   Sydney (syd)†           Australia (au)        Asia Pacific (ap)   
dal13   Dallas (dal)†           United States (us)    North America (na)   
sng01   Singapore (sng-mtr)     Singapore (sng)       Asia Pacific (ap)   
fra02   Frankfurt (fra)†        Germany (de)          Europe (eu)   

† denotes the zone is in a multizone region.

>>ibmcloud ks flavors --zone eu-de-1

For more information about these flavors, see 'https://ibm.biz/flavors'
Name           Cores   Memory   Network Speed   OS             Server Type   Storage   Secondary Storage   Flavor Class   Provider   
bx2.16x64      16      64GB     16Gbps          UBUNTU_18_64   virtual       100GB     0B                  bx2            vpc-gen2   
bx2.2x8†       2       8GB      4Gbps           UBUNTU_18_64   virtual       100GB     0B                  bx2            vpc-gen2   
bx2.32x128     32      128GB    16Gbps          UBUNTU_18_64   virtual       100GB     0B                  bx2            vpc-gen2   
bx2.48x192     48      192GB    16Gbps          UBUNTU_18_64   virtual       100GB     0B                  bx2            vpc-gen2   
bx2.4x16       4       16GB     8Gbps           UBUNTU_18_64   virtual       100GB     0B                  bx2            vpc-gen2   
bx2.8x32       8       32GB     16Gbps          UBUNTU_18_64   virtual       100GB     0B                  bx2            vpc-gen2   
cx2.16x32      16      32GB     16Gbps          UBUNTU_18_64   virtual       100GB     0B                  cx2            vpc-gen2   
cx2.2x4†       2       4GB      4Gbps           UBUNTU_18_64   virtual       100GB     0B                  cx2            vpc-gen2   
cx2.32x64      32      64GB     16Gbps          UBUNTU_18_64   virtual       100GB     0B                  cx2            vpc-gen2   
cx2.48x96      48      96GB     16Gbps          UBUNTU_18_64   virtual       100GB     0B                  cx2            vpc-gen2   
cx2.4x8†       4       8GB      8Gbps           UBUNTU_18_64   virtual       100GB     0B                  cx2            vpc-gen2   
cx2.8x16       8       16GB     16Gbps          UBUNTU_18_64   virtual       100GB     0B                  cx2            vpc-gen2   
mx2.128x1024   128     1024GB   16Gbps          UBUNTU_18_64   virtual       100GB     0B                  mx2            vpc-gen2   
mx2.16x128     16      128GB    16Gbps          UBUNTU_18_64   virtual       100GB     0B                  mx2            vpc-gen2   
mx2.2x16†      2       16GB     4Gbps           UBUNTU_18_64   virtual       100GB     0B                  mx2            vpc-gen2   
mx2.32x256     32      256GB    16Gbps          UBUNTU_18_64   virtual       100GB     0B                  mx2            vpc-gen2   
mx2.48x384     48      384GB    16Gbps          UBUNTU_18_64   virtual       100GB     0B                  mx2            vpc-gen2   
mx2.4x32       4       32GB     8Gbps           UBUNTU_18_64   virtual       100GB     0B                  mx2            vpc-gen2   
mx2.64x512     64      512GB    16Gbps          UBUNTU_18_64   virtual       100GB     0B                  mx2            vpc-gen2   
mx2.8x64       8       64GB     16Gbps          UBUNTU_18_64   virtual       100GB     0B                  mx2            vpc-gen2 
```

Put the desired flavor in the file.

For the OpenShift clusters the command is (https://cloud.ibm.com/docs/containers?topic=containers-clusters);

```shell
ibmcloud oc flavors --zone ZONE --provider (classic | vpc-gen2) [--show-storage] [--output json] [-q]

Example:
ibmcloud oc flavors --zone us-south-1 --provider vpc-gen2
```

