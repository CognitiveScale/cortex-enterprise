# Cortex Dedicated Instance

[0613da0c]: https://portal.azure.com/#create/cognitive-scale.cognitivescale-cortex5-ai-previewcortex-ai-platform 
[b6d1b728]: https://docs.microsoft.com/en-us/azure/architecture/cloud-adoption/getting-started/azure-resource-access 
[Azure-account]: https://azure.microsoft.com/en-us/free/search/?&OCID=AID719825_SEM_TZhJDdn3&lnkd=Google_Azure_Brand&gclid=EAIaIQobChMI--_74KfS4QIVzMDACh2KVwTuEAAYASAAEgJLK_D_BwE
[azure-cli]: https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest
[resource-group-cli]: https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest
[dns-zone-cli]: https://docs.microsoft.com/en-us/cli/azure/network/dns/zone?view=azure-cli-latest
[vCPU-quota-requests]: https://docs.microsoft.com/en-us/azure/azure-supportability/resource-manager-core-quotas-request
[lesssecureapps]: https://www.google.com/settings/security/lesssecureapps
[disablecaptcha]: https://accounts.google.com/b/0/displayunlockcaptcha

## What is Cortex Dedicated Instance?

Cortex Enterprise is deployment strategy for CognitiveScale's Cortex platform. The Cortex platform can be deployed two major ways: on the SaaS Cloud, the original deployment strategy or as a Dedicated Instance, the newest deployment option. The table below highlights the differences between the two deployment methods.

|  | **Cortex Cloud** | **Cortex Dedicated Instance** |
| --- | --- | --- |
| **Description** | CognitiveScale’s original Cortex deployment, a multi-tenant, CS-managed cloud solution. | CognitiveScale’s new Cortex deployment, a private-cloud, client-managed  solution. |
| **Who manages the cloud/stack?** | CognitiveScale | Client |
| **Where is the stack managed?** | CognitiveScale's AWS Cloud (Rancher) | Client's Azure Cloud (Kubernetes)|
| **What are the advantages of this deployment?** | CognitiveScale manages and monitors stack activity, updates, and infrastructure. Edge add-on option provides data-secure processing. | Clients manage their own individual deployments. Their data is not exposed to CognitiveScale, even indirectly. |

## Prerequisites for Launching Cortex Dedicated

After the prerequisite steps are completed, the client configures their stack in Azure Marketplace, deploys it, and verifies the deployment by logging in to their Kubernetes dashboard.  Your CognitiveScale CS1 and SRE representatives provide assistance and direction throughout the Enterprise deployment process.

  **NOTE**: Instructions for completing prerequisite processes are detailed below.

To complete the instantiation process documented below you must have permissions and ability to:

  - Create a pay-as-you-go subscription
  - Create Resource Groups
  - Create DNS Zones
  - View Azure logs
  - Register an Internet domain name
  - Create a subdomain NS record
  - Manage (create, delete, and edit) DNS record sets

You will require the following details to complete the instantiation process:

  - Subscription: (created by user during subscription setup)
  - Location: (centralUS) 
  - Subdomain: (provided by user - registered internet domain name obtained prior to beginning deployment setup in Azure (e.g. example.com) with a domain name registrar (e.g. AWS, godaddy.com, name.com, etc.)
  - Cortex Lisence Key: (provided by CognitiveScale in welcome email)
  - Cortex Cluster Name: (user created)
  - VM SSH Public Key: (user generated)
  - Resource Group: (created as part of the process)
  - DNS Zone: (created as part of the process)
  - Cortex Administrator: Email Address (provided by user)
  - Cortex Administrator: Password (created by user)
  - Cortex Account Name: (created by user) 
  - Cortex Administrator phone number: (provided by user)

1. Go to [Azure Marketplace][Azure-account] to create a Subscription - Select a "Pay" Azure subscription or use your existing Azure subscription.
2. Learn about Azure: [Getting Started with Azure][b6d1b728].
3. Recieve CognitiveScale welcome email that provides the `cortex license key`, which provides the proxy access to the Cortex Documentation and Cortex Marketplace web portals.
4. Get started in the Azure portal:

   - a. Launch the Azure CLI. 
   - b. Select your subscription and opt to create storage account.
   - c. Select to run `-bash` (or `-powershell` Instructions for Powershell users can be found in MS Azure help.
   - d. When the terminal opens, list your subscriptions: `az account list`.
   - e. Set your subscription for future sessions: `az account set --subscription my-subscription-name`.
   
5. Remove AZ vCPU limits in Azure: See [Resource Manager vCPU quota increase requests][vCPU-quota-requests]. 
6. Create a Resource Group - [Use the Azure CLI to Create a Resource Group][resource-group-cli].
7. Create a DNS Zone and subdomain in your Azure subscription - [Use the Azure CLI to create a DNS Zone][dns-zone-cli].
  
After the prerequisite steps are completed, the client configures their stack in Azure Marketplace, deploys it, and verifies the deployment by logging in to their Kubernetes dashboard.  Your CognitiveScale CS1 and SRE representatives provide assistance and direction throughout the Enterprise deployment process.

### Create Resource Group and DNS Zones using the Azure Portal:
1. Login into Azure Portal
2. At top of left panel click ADD RESOURCE.
3. Search Resource Group.
4. On the Create Resource Group page click CREATE.
5. Enter:

  - Subscription: This is prefilled but can be changed.
  - Resource Group: Enter the name you want to use for the Resource Group for this deployment.
  - Location: This is prefilled but can be changed. (Central US)

6. Click REVIEW AND CREATE
7. Click CREATE
8. At top of left panel click ADD RESOURCE.
9. Search "DNS Zone" or select from Marketplace list.
10. Click CREATE
11. Enter: 

  - Subscription: (This is prefilled but can be changed.)
  - Resource Group: Select the one you have just created. 
  - Instance Details - Name: The name of the subdomain. Location (optional for larger deployments)

12. Click REVIEW AND CREATE
13. Click CREATE
14. On the DNS Zone details page capture the Server Names in the right column
15. After running the script, a DNS zone is identified. Take note of it. (Example: DNS_ZONE=cortex-7664-92-200.<resource-group-name>-aks.insights.ai
  
NOTE: The same Resource Group must be used to create the DNS zone (above) and in the Configuration Setup for the deployment (below).

## Configure Cortex Dedicated Instance  

1. Login to your Azure Marketplace subscription and go to [CognitiveScale Cortex Cognitive Platform][0613da0c]
2. Click CREATE.
3. On the BASICS tab enter:

  - **Cortex Cluster Name**: User-created name for the cluster that is used as an organization name for the Cortex deployment
  - **VM SSH Public Key**: The Administrator SSH Public Key to install on AKS nodes. (In your local terminal run`cat ~/.ssh/id_rsa.pub`)
  - **Cortex License Key**: License Key for Cortex sent from your CognitiveScale representative.
  - **DNS Zone** or Domain name for Cortex (created as a prerequisite): The DNS root or domain name to which all Cortex subdomains are attached, e.g. console.domain.com.  
  - **Subscritption**: The name of your subscription.
  - **Resource Group**: A collection that shares the same lifecycle, permissions, and policies.
  - **Location**: A selectable list of geographic locations for your account/business. Use `centralus` for pay-as-you-go.
  
4. Click **OK**
5. In the ADMIN DETAILS tab enter:

  - **Cortex Administrator Email Address**: The administrator must have the permissions described about in Prerequisites.
  - **Cortex Administrator Password**: Use a secure password and make note of it.
  - **Confirm Password**
  - **Cortex Account Name**: Enter your desired Account Name.

6. Click OK.

7. On the DEPLOYMENT DETAILS tab enter:
  
  - **Virtual Machine Count**: select the number of VMs to be used as AKS nodes.
  - **Size: Change size** (link): displays a selectable table of all of the deployment size options and price estimations.
  - **Deployment style: Standard/Extended** (toggle): The size of the Cortex deployment - Standard (All DB’s are embedded in the cluster) and Extended (Cortex with select managed services from Azure)
  - **Second Region Location**: for Extended deployment style only. Provides a failover location.

8. Click OK.
9. View and verify your configuration details on the SUMMARY tab.
10. Click OK.
11. On the BUY tab enter a phone number for verification.
12. Click OK.

Your Cortex Dedicated Instance deployment process usually takes 20-30 minutes, but sometimes requires over an hour, which is a known Azure specific issue.

## Monitor your deployment

To monitor the logs, use the following Azure command for your Resource Group: `az container logs --follow -g <YourSubscription> -n provision`

## Determine your Base Domain

Your base domain is the DNS zone name. 

1. Go to your Azure portal. 
2. Navigate to your Resource Group from the left panel. 
3. The DNS zone/BASE DOMAIN is displayed.
4. Use the base domain and the following URLs to access Cortex tools and interfaces:

  - **Cortex Admin Console**: https://console.<base_domain>
  - **Cortex Studio Connection Url**: https://api.<base_domain>
  - **Cortex Private registry**: private-registry.<base_domain>
  - **CognitiveScale Marketplace**: https://marketplace.<base_domain>
  - **Cortex CLI**: https://api.<base_domain>
  
## Set up .kube/config to view cluster

1. From your local teminal remove all lines from the .kube/config OR move the file to another name to back it up.
2. Login to Azure portal in your terminal (will direct you to web  browser to login)
3. Run the following command to update your `~/.kube/config` file with your azure credentials: `az aks get-credentials --resource-group <ResourceGroupName>centralus --name aks_cluster_backend --admin`
4. Run the following command to gain access to the Kubernetes dashboard: `kubectl create clusterrolebinding kubernetes-dashboard -n kube-system --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard'
5. Run the following command to start the proxy for the Kubernetes dashboard: `Kubectl proxy --port=9090`

The Kubernetes dashboard can then be reached from here: http://localhost:9090/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/overview?namespace=default

## View the modsec logs for debugging WAF issues

1. Login to your Azure portal.
2. Verify your ./kube/config set up (above)
3. Run the following: `kubectl exec -it $(kubectl get pods --all-namespaces | grep nginx-ingress-controller | awk '{print $2}') -- /bin/bash`
4. Find your log located here: `/var/log/modsec_audit.log`

## Troubleshooting Google domains

If the SMTP is set up with Google:

1. [Enable less secure apps][lesssecureapps]
2. [Disable Captcha temporarily so you can connect the new device/server][disablecaptcha]

