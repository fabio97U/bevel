[//]: # (##############################################################################################)
[//]: # (Copyright Accenture. All Rights Reserved.)
[//]: # (SPDX-License-Identifier: Apache-2.0)
[//]: # (##############################################################################################)

Install Pre-requisites
=====================

Before we begin, if you haven't already done so, you may wish to check that
you have all the prerequisites below installed on the platform(s)
on which you'll be deploying blockchain networks from and/or operating
Hyperledger Bevel.

## Git Repository
As you may have read in the [key concepts](keyconcepts), Hyperledger Bevel  uses GitOps method for deployment to Kubernetes clusters, so a Git repository is needed for Bevel (this can be a [GitHub](https://github.com/) repository as well).
Fork or import the [Bevel GitHub repo](https://github.com/hyperledger/bevel) to this Git repository.

The Operator should have a user created on this repo with full access to the Git Repository.

---
**NOTE:** Install Git Client Version > **2.31.0**

---

## Kubernetes
Hyperledger Bevel  deploys the DLT/Blockchain network on [Kubernetes](https://kubernetes.io/) clusters; so to use Bevel, at least one Kubernetes cluster should be available.
Bevel recommends one Kubernetes cluster per organization for production-ready projects. 
Also, a user needs to make sure that the Kubernetes clusters can support the number of pods and persistent volumes that will be created by Bevel.

---
**NOTE:** For the current release Bevel has been tested on Amazon EKS with Kubernetes version **1.19**.

Bevel has been tested on Kubernetes >= 1.14 and <= 1.19

Also, install kubectl Client version **v1.19.8**

---

Please follow [Amazon instructions](https://aws.amazon.com/eks/getting-started/) to set-up your required Kubernetes cluster(s).
To connect to Kubernetes cluster(s), you will also need kubectl Command Line Interface (CLI). Please refer [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/) for installation instructions, although Hyperledger Bevel configuration code (Ansible scripts) installs this automatically.

## HashiCorp Vault
In this current release, [Hashicorp Vault](https://www.vaultproject.io/) is mandatory for Hyperledger Bevel  as the certificate and key storage solution; so to use Bevel, at least one Vault server should be available. Bevel recommends one Vault per organization for production-ready projects. 

Follow [official instructions](https://www.vaultproject.io/docs/install/) to deploy Vault in your environment. 

---
**NOTE:** Recommended approach is to create one Vault deployment on one VM and configure the backend as a cloud storage.

Vault version should be **1.7.1**

---
## Ansible

Hyperledger Bevel configuration is essentially Ansible scripts, so install Ansible on the machine from which you will deploy the DLT/Blockchain network. This can be a local machine as long as Ansible commands can run on it.

Please note that this machine (also called **Ansible Controller**) should have connectivity to the Kubernetes cluster(s) and the Hashicorp Vault service(s). And it is essential to install the [git client](https://git-scm.com/download) on the Ansible Controller. 

---
**NOTE:** Minimum **Ansible** version should be **2.10.5** with **Python3** 

Also, Ansible's k8s module requires the **openshift python package (>= 0.12.0)** and collection **kubernetes.core**.

```
pip3 install openshift==0.12.0
ansible-galaxy collection install kubernetes.core
```

**NOTE (MacOS):** Ansible requires GNU tar. Install it on MacOS through Homebrew `brew install gnu-tar`

---
### Configuring Ansible Inventory file

In Hyperledger Bevel, we connect to Kubernetes cluster through the **Ansible Controller** and do not modify or connect to any other machine directly. Hyperledger Bevel's sample inventory file is located [here](https://github.com/hyperledger/bevel/tree/main/platforms/shared/inventory/ansible_provisioners). 

Add the contents of this file in your Ansible host configuration file (typically in file /etc/ansible/hosts).

Read more about Ansible inventory [here](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html).

---
### NPM

Hyperledger Bevel provides the feature of automated validation of the configuration file (network.yaml), this is done using ajv (JSON schema validator) cli. The deployment scripts install ajv using npm module which requires npm as prerequisite.

You can install the latest NPM version from offical [site](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm).

---
## Docker

Hyperledger Bevel provides pre-built docker images which are available on [Docker Hub](https://hub.docker.com/u/hyperledgerlabs). If specific changes are needed in the Docker images, then you can build them locally using the Dockerfiles provided. A user needs to install [Docker CLI](https://docs.docker.com/install/) to make sure the environment has the capability of building these Dockerfiles to generate various docker images. Platform specific docker image details are mentioned [here](./operations/configure_prerequisites.md).

---
**NOTE:** Hyperledger Bevel uses minimum Docker version **18.03.0**

---

You can check the version of Docker you have installed with the following
command from a terminal prompt:
```
    docker --version
```

For storing private docker images, a private docker registry can be used. Information such as registry url, username, password, etc. can be configured in the configuration file like [Fabric configuration file](./operations/fabric_networkyaml.md).

### Docker Build for dev environments

Hyperledger Bevel is targetted for Production systems, but, in case, a developer environment is needed, you can create a containerized Ansible machine to deploy the dev DLT/Blockchain network using docker build.  

The details on how to create a containerized Ansible machine is mentioned [here](./developer/docker-build.md).

---
**NOTE:** This containerized machine (also called **Ansible Controller**) should have connectivity to the Kubernetes cluster(s) and the Hashicorp Vault service(s).

---

## Internet Domain
As you may have read in the [Kubernetes key concepts](keyConcepts/kubernetes), Hyperledger Bevel uses [Ambassador](https://www.getambassador.io/about/why-ambassador/) or [HAProxy Ingress Controller](https://www.haproxy.com/documentation/hapee/1-9r1/traffic-management/kubernetes-ingress-controller/) for inter-cluster communication. So, for the Kubernetes services to be available outside the specific cluster, at least one DNS Domain is required. This domain name can then be sub-divided across multiple clusters and the domain-resolution configured for each.
Although for production implementations, each organization (and thereby each cluster), must have one domain name.

---
**NOTE:** If single cluster is being used for all organizations in a dev/POC environment, then domain name is not needed.
