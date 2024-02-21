
# Landgriffon Google Marketplace Deployer

This repository contains the Google Marketplace Deployer configuration to deploy the Landgriffon platform on a Google Kubernetes Engine (GKE) cluster.

# Overview

Deforestation and water stress have a negative impact on agricultural supply chains, preventing agribusiness and food companies from becoming more sustainable. Advanced technology such as the Copernicus programme provides precise, timely and easily accessible data that improve environmental management and mitigate climate change effects. The EU-funded LAND GRIFFON project will develop digital decision-making instruments based on Copernicus data to observe, prognoses, analyse and follow environmental impacts on the entire agricultural supply chain. These innovative instruments will support agribusiness and food enterprises in becoming more sustainable and transparent.

For more information, visit the LandGriffon [official website](https://landgriffon.com/).

## About Google Click to Deploy

Popular open stacks on Kubernetes, packaged by Google.

## Architecture

-- TODO

This app offers "list of resources".

# Installation

## Quick install with Google Cloud Marketplace

Get up and running with a few clicks! To install LandGriffon to a
Google Kubernetes Engine cluster via Google Cloud Marketplace, follow the
[on-screen instructions]().

-- TODO

## Command-line instructions

### Prerequisites

#### Set up command-line tools

You'll need the following tools in your development environment. If you are
using Cloud Shell, then `gcloud`, `kubectl`, Docker, and Git are installed in
your environment by default.

- [gcloud](https://cloud.google.com/sdk/gcloud/)
- [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/)
- [mpdev](https://github.com/GoogleCloudPlatform/marketplace-k8s-app-tools/blob/master/docs/mpdev-references.md)
- [docker](https://docs.docker.com/install/)
- [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)


Configure `gcloud` as a Docker credential helper:

```shell
gcloud auth configure-docker
```

#### Create a Google Kubernetes Engine (GKE) cluster

Create a new cluster from the command-line:

```shell
export CLUSTER=$CLUSTER_NAME
export ZONE=$ZONE

gcloud container clusters create "${CLUSTER}" --zone "${ZONE}"
```

Configure `kubectl` to connect to the new cluster:

```shell
gcloud container clusters get-credentials "${CLUSTER}" --zone "${ZONE}"
```

#### Clone this repo

Clone this repo, and its associated tools repo:

```shell
git clone --recursive https://github.com/Vizzuality/lg-marketplace-deployer.git
```

#### Install the Application resource definition

An Application resource is a collection of individual Kubernetes components,
such as Services, Deployments, and so on, that you can manage as a group.

To set up your cluster to understand Application resources, run the following
command:

```shell
kubectl apply -f "https://raw.githubusercontent.com/GoogleCloudPlatform/marketplace-k8s-app-tools/master/crd/app-crd.yaml"
```

You need to run this command once.

The Application resource is defined by the
[Kubernetes SIG-apps](https://github.com/kubernetes/community/tree/master/sig-apps)
community. You can find the source code at
[github.com/kubernetes-sigs/application](https://github.com/kubernetes-sigs/application).

### Install the app

Navigate to the `lg-marketplace-deployer` directory:

```shell
cd lg-marketplace-deployer
```


#### Build and push the container image

To build the container image, run the following command:

```shell
docker build -t ${REPOSITORY}/${PROJECT_ID}/landgriffon-deployer:latest .
```

```shell
docker push ${REPOSITORY}/${PROJECT_ID}/landgriffon-deployer:latest
```



#### Configure the app with environment variables

The Deployer is configured following the 
[Google Marketplace Building Deployer Manuak](https://github.com/GoogleCloudPlatform/marketplace-k8s-app-tools/blob/master/docs/building-deployer.md)
Please, refer to the official documentation to understand how to configure the Deployer.

- File structure
 ```
.
├── manifest
|   ├── manifests.yaml.template
|   └── application.yaml.template
├── data-test
|   ├── manifest
|   |   ├── tester.yaml.template
|   ├── schema.yaml
├── Dockerfile
├── schema.yaml
```

The ``manifests.yaml.template`` file contains the Kubernetes manifests that will be deployed to the cluster. The ``application.yaml.template`` file contains the Application resource definition that will be used to deploy the application to the cluster.

The ``schema.yaml`` file contains the schema for the configuration form that will be used to configure the application. The ``Dockerfile`` is used to build the container image for the deployer.

 - Environment variables:
   All the enviroment variables are defined in the ``schema.yaml`` file. Please follow the official documentation to understand how to override the default values.
   The default values are used for the Integration Test. Properties that have no default value are auto-generated and encoded to `base64` format.
   You can override the default values by passing them to `mpdev` command. Note that all values that are part of a secret object must be passed as `base64` encoded strings.
   
  - TODO: Add more customizations

#### Install the app

 - To install the app, run the following command:

```shell
mpdev install --deployer=<YOUR DEPLOYER IMAGE>
```

 - You add a ``--parameters`` flag to provide required values with no default value. Additionally, you can override any variable passing it in the parameters object. The parameters must be passed as a `key=value` pair. Refer to `mpdev` documentation for more information.
   

#### Integration Test

The integration test is defined in the `data-test` directory. The `tester.yaml.template` file contains the Kubernetes manifests that will be deployed to the cluster. The `schema.yaml` file contains the schema for the configuration form that will be used to configure the application. The integration test is executed by the `mpdev` command.

```shell
mpdev verify --deployer=<YOUR DEPLOYER IMAGE>
```
 - The deployer integration will start using the default env values. It will create a random namespace and apply all resources to it
 - Ones that all resources become healthy, it will automatically delete the namespace and all resources inside it, passing the integration test.

### Delete the resources

> **NOTE:** We recommend that you use a `kubectl` version that is the same
> version as that of your cluster. Using the same versions for `kubectl` and
> the cluster helps to avoid unforeseen issues.

To delete the resources, delete the namespace which all the resource are bound to:

```shell
kubectl namespace $NAMESPACE
```


### Delete the GKE cluster

If you don't need the deployed app or the GKE cluster, delete the cluster
by using this command:

```shell
gcloud container clusters delete "${CLUSTER}" --zone "${ZONE}"
```