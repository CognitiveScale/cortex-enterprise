# Cortex Enterprise

[0613da0c]: https://portal.azure.com/#create/cognitive-scale.cognitivescale-cortex5-ai-previewcortex-ai-platform "Create Cortex Deployment"
[b6d1b728]: https://docs.microsoft.com/en-us/azure/architecture/cloud-adoption/getting-started/azure-resource-access "Azure Getting Started"


## What is Cortex Enterprise?

The Cortex platform can be deployed two ways: Cloud, the original deployment strategy and Enterprise, the newest deployment option. The table below highlights the differences between the two deployments.


|  | **Cortex Cloud** | **Cortex Enterprise** |
| --- | --- | --- |
| **Description** | CognitiveScale’s original Cortex deployment, a multi-tenant, CS-managed cloud solution. | CognitiveScale’s new Cortex deployment, a private-cloud, client-managed  solution. |
| **Who manages the cloud/stack?** | CognitiveScale | Client |
| **Where is the stack managed?** | CognitiveScale's AWS Cloud (Rancher) | Client's Azure Cloud (Kubernetes)|
| **What are the advantages of this deployment?** | CognitiveScale manages and monitors stack activity, updates, and infrastructure. Edge add-on option provides data-secure processing. | Clients manage their own individual deployments. Their data is not exposed to CognitiveScale, even indirectly. |


## Cortex Enterprise deployment sizes: 

The size of the Cortex Enterprise deployment is a key consideration. The table below shows the deployment size parameters

  | **Size** | **Number of Compute VMs** | **CPUs/Compute VM| Memory/Compute VM** | **Number of Backend VMs** | **CPUs/Backend VM** | **Memory/Backend VM** |
  | --- | --- | --- | --- | --- | --- | --- |
  | **Small** | 3 VMs | 4 CPUs/VM | 32 gigs RAM/VM | 3 VMs | 4 CPUs/VM | 32 gigs RAM/VM |
  | **Medium** | 3 VMs | 8 CPUs/VM | 64 gigs RAM/VM | 3 VMs | 4 CPUs/VM | 2 gigs RAM/VM |
  | **Large** | 3 VMs | 16 CPUs/VM | 128 gigs RAM/VM | 3 VMs | 8 CPUs/VM | 64 gigs RAM/VM|
  | **Extra-Large** | 3 VMs | 16 CPUs/VM | 128 gigs RAM/VM | 6 VMs | 8 CPUs/VM | 64 gigs RAM/VM |   


## Prerequisites

Instructions for completing these prerequisites are detailed below.

- Setup a "Pay as You Go" Azure subscription.
- Remove AZ vCPU limits in Azure.
- Create DNS Zone for subdomain in your Azure subscription.
- Create subdomain pointer NS for root domain (where ever root domain is hosted AWS, GoDaddy, etc.).
- Create Service Principal Account in Azure subscription.
- CognitiveScale invites you to the Cortex Enterprise page in Azure Marketplace and sends a welcome email that provides the `cortex enterprise license key`, which provides the proxy access to the Cortex Documentation and Cortex Marketplace web portals.

After the prerequisite steps are completed, the client configures their stack in Azure Marketplace, deploys it, and verifies the deployment by logging in to their Kubernetes dashboard.  Your CognitiveScale CS1 and SRE representatives provide assistance and direction throughout the Enterprise deployment process 

## Azure links

- [Getting Started with Azure][b6d1b728]
- [CognitiveScale Cortex Cognitive Platform the Cortex Enterprise deployment solution on Azure Marketplace][0613da0c]

