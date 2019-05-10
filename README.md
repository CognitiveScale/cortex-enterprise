# Dedicated Cortex Instance

[0613da0c]: https://portal.azure.com/#create/cognitive-scale.cognitivescale-cortex5-ai-previewcortex-ai-platform 
[b6d1b728]: https://docs.microsoft.com/en-us/azure/architecture/cloud-adoption/getting-started/azure-resource-access 
[Azure-account]: https://azure.microsoft.com/en-us/free/search/?&OCID=AID719825_SEM_TZhJDdn3&lnkd=Google_Azure_Brand&gclid=EAIaIQobChMI--_74KfS4QIVzMDACh2KVwTuEAAYASAAEgJLK_D_BwE
[dns-getting-started]: https://docs.microsoft.com/en-us/azure/dns/dns-getstarted-portal
[dns-faq]: https://docs.microsoft.com/en-us/azure/dns/dns-faq
[azure-cli]: https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest
[vCPU-quota-requests]: https://docs.microsoft.com/en-us/azure/azure-supportability/resource-manager-core-quotas-request
[lesssecureapps]: https://www.google.com/settings/security/lesssecureapps
[disablecaptcha]: https://accounts.google.com/b/0/displayunlockcaptcha
[subscription-help]: https://docs.microsoft.com/en-us/azure/billing/billing-getting-started
[service principal]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-auth-service-principal

## Table of Contents

[What is Dedicated Cortex Instance?](https://github.com/CognitiveScale/cortex-enterprise#what-is-dedicated-cortex-instance)

[Prerequisites for Launching Dedicated Cortex Instance](https://github.com/CognitiveScale/cortex-enterprise#prerequisites-for-launching-dedicated-cortex-instance)

* [Before you begin working in Azure](https://github.com/CognitiveScale/cortex-enterprise#before-you-begin-working-in-azure)

* [Azure permissions required to perform an installation](https://github.com/CognitiveScale/cortex-enterprise#azure-permissions-required-to-perform-an-installation)

* [Deployment details](https://github.com/CognitiveScale/cortex-enterprise#deployment-details)

* [Prerequisite Steps](https://github.com/CognitiveScale/cortex-enterprise#prerequisite-steps)

[Configure Cortex Dedicated Instance](https://github.com/CognitiveScale/cortex-enterprise#configure-cortex-dedicated-instance)

[Monitor your deployment](https://github.com/CognitiveScale/cortex-enterprise#monitor-your-deployment)

[Determine your Base Domain](https://github.com/CognitiveScale/cortex-enterprise#determine-your-base-domain)

[Set up ./kube/config to view cluster through Kubernetes dashboard](https://github.com/CognitiveScale/cortex-enterprise#set-up-kubeconfig-to-view-cluster-through-kubernetes-dashboard)

[View the modsec logs for debugging WAF issues](https://github.com/CognitiveScale/cortex-enterprise#view-the-modsec-logs-for-debugging-waf-issues)

[SMTP Setup](https://github.com/CognitiveScale/cortex-enterprise#smtp-setup)

[Troubleshooting Google domains](https://github.com/CognitiveScale/cortex-enterprise#troubleshooting-google-domains)

## What is Dedicated Cortex Instance?

A Dedicated Cortex Instance is a deployment strategy for CognitiveScale's Cortex platform. The Cortex platform can be deployed two major ways: on the SaaS Cloud, the original deployment strategy or as a Dedicated Instance, the newest deployment option. The table below highlights the differences between the two deployment methods.

|  | **Cortex Cloud** | **Dedicated Cortex Instance** |
| --- | --- | --- |
| **Description** | CognitiveScale’s original Cortex deployment, a multi-tenant, CS-managed cloud solution. | CognitiveScale’s new Cortex deployment, a private-cloud, client-managed  solution. |
| **Who manages the cloud/stack?** | CognitiveScale | Client |
| **Where is the stack managed?** | CognitiveScale's Cloud | Client's Azure Cloud (Kubernetes)|
| **What are the advantages of this deployment?** | CognitiveScale manages and monitors stack activity, updates, and infrastructure. Edge add-on option provides data-secure processing. | Clients manage their own individual deployments. Their data is not exposed to CognitiveScale, even indirectly. |



## Azure and Dedicated Cortex Instance architecture

After the prerequisite steps are completed, the client configures their stack in Azure Marketplace, deploys it, and verifies the deployment by logging in to their Kubernetes dashboard.  Your CognitiveScale CS1 and SRE representatives provide assistance and direction throughout the deployment process.

Your Dedicated Cortex Instance is configured as follows:

  - **Subscription**: Create a pay-as-you-go Azure Marketplace subscription (prerequisite)
   - **Main Resource Group**: Within your subscription you must have a main Resource Group (prerequisite) - NOTE: This Resource Group must be NEW with no existing resources. 
    - **Main DNS Zone**: Within your main resource group you must configure your main DNS Zone (prerequisite)
     - **DCI Resource Group**: In the DCI Create setup you must create a NEW empty resource group for your DCI deployment. 
      - **DCI DNS Zone** (subdomain): Within the DCI Resource Group you must create a DNS subdomain that references the Main DNS Zone. Your URLs and access to Cortex tools and UIs are based on this DNS Zone-Base Domain
      
NOTE: the nested structure of the 

### Before you begin working in Azure:

- Create DNS domain at domain registrar like Google, GoDaddy, Name Cheap, or Route 53. You will need this to create the main DNS Zone.
- Verify that you have the follwing CLI utilities installed: (On Mac use homebrew installer to install the utilities: `brew tap derailed/k9s && brew install kubectl azure-cli jq k9s`)

  - kubectl
  - azure-cli
  - jq
  - k9s

### Azure permissions required to perform an installation

To complete the Dedicated Cortex Instance deployment you must have the abiltiy to perform the following actions in Azure:

  - Create a subscription
  - Create Resource Groups
  - Create DNS Zones
  - View Azure logs
  - Create a subdomain NS record
  - Manage (create, delete, and edit) DNS record sets

### Deployment details  

You will require the following details to complete the instantiation process:

  - Subscription: (created by user as part of the prerequisites process)
  - Resource Group (main): (created as part of the prerequisites process)
  - DNS Zone (main): (created as part of the prerequisite process)
  - Location: (centralUS)  
  - Cortex Lisence Key: (provided by CognitiveScale in welcome email)
  - Cortex Cluster Name: (user created)
  - VM SSH Public Key: (user generated) 
  - Resource Group (subordinate): (created as part of the create cluster process) 
  - DNS Zone (subordinate): (created as part of the create cluster process)
  - Azure AD Service Principal ID and password (created as part of the prerequisites process)
  - Cortex Administrator: Email Address (provided by user)
  - Cortex Administrator: Password (created by user)
  - Cortex Account Name: (created by user) 
  - Cortex Administrator phone number: (provided by user)

### Prerequisite Steps

1. Go to Azure Marketplace and create a [subscription][subscription-help] - Select a "Pay" Azure subscription or use your existing Azure subscription.
 - a. Create your main Resource Group
 - b. [Create your main DNS Zone][dns-getting-started] ([More information about DNS Zones in Azure][dns-faq]
2. Learn about Azure: [Getting Started with Azure][b6d1b728].
3. Recieve CognitiveScale welcome email that provides the `cortex license key`, which provides the proxy access to the Cortex Documentation and Cortex Marketplace web portals.
4. Get started in the Azure portal:

   - a. [Launch the Azure CLI][azure-cli]. 
   - b. Select your subscription and opt to create storage account.
   - c. Select to run `-bash` (or `-powershell` Instructions for Powershell users can be found in MS Azure help.
   - d. When the terminal opens, list your subscriptions: `az account list`.
   - e. Set your subscription for future sessions: `az account set --subscription my-subscription-name`.
   
5. Remove AZ vCPU limits in Azure: See [Resource Manager vCPU quota increase requests][vCPU-quota-requests]. 
6. Create a Resource Group - [Use the Azure CLI to Create a Resource Group][resource-group-cli]. You may also create a Resource Group from the Basics tab as part of the configuration steps.

  NOTE: Use this Resource Group when you create the DNS zone.

7. [Setup your Azure AD Service Principal App ID and password][service principal]. These values will be used for all clusters you deploy in this subscription.
 
After these prerequisite steps are completed, you are ready to configure your Dedicated Cortex Instance in Azure Marketplace, deploy it, and verify the deployment by logging in to their Kubernetes dashboard.  Your CognitiveScale CS1 and SRE representatives are available to provide assistance and direction throughout the Enterprise deployment process.

## Configure Cortex Dedicated Instance  

1. Login to your Azure Marketplace subscription and go to [CognitiveScale Cortex Cognitive Platform][0613da0c] or search for "Cortex Cognitive Platform."
2. Click CREATE.
3. On the BASICS tab enter:

  - **Cortex Cluster Name**: User-created name for the cluster, used as an organization name for the deployment
  - **VM SSH Public Key**: The Administrator SSH Public Key to install on AKS nodes. (In your local terminal run`cat ~/.ssh/id_rsa.pub`and go to the local folder where the key was created. Open it in a text editor window and copy the entire key into the field.)
  - **Cortex License Key**: License Key for your Dedicated Cortex Instance sent from your CognitiveScale representative in an email.  
  - **Subscritption**: The name of your subscription.
  - **Resource Group**: A collection that shares the same lifecycle, permissions, and policies. Click the CREATE RESOURCE GROUP link and follow the instructions in the Azure UI. You can name this Resource Group the same as your Cortex Cluster name for simplicity.
  - **Location**: A selectable list of geographic locations for your account/business. Use `centralus` for pay-as-you-go.
  
4. Click **OK**
5. In the ADMIN DETAILS tab enter:

  The details entered in this section will be used to authenticate to the Kubernetes dashboard and Cortex UI and tools following deployment of the cluster.
  
  - **Cortex Administrator Email Address**: The administrator must have the permissions described in the Prerequisites section above.
  - **Cortex Administrator Password**: Use a secure password and make note of it. 
  - **Confirm Password**: Reenter the password.
  - **Cortex Account Name**: Enter your desired Account Name. (NOTE: Do NOT use capital letters or special characters aside from underscore or dash in this name.)

6. Click OK.

7. On the SERVICE PRINCIPLE DETAILS tab enter:

  - **Azure AD Service Principle App ID**: The Azure Service Principle Application ID to use for Azure resource creation for your subscription.
  - **Azure AD Service Principle Password**: The Azure Service Principle Application Password to use for Azure resource creation for your subscription.

8. Click OK.

9. On the DEPLOYMENT DETAILS tab enter:

  - **DNS Zone** or Domain name for Cortex: The DNS root or domain name to which all Cortex subdomains are attached (e.g. console.domain.com). To create this you must: _add steps here_ 
  - **Virtual Machine Count**: select the number of VMs to be used as AKS nodes.
  - **Size: Change size** (link): displays a selectable table of all of the deployment size options and price estimations.
  - **Deployment style: Standard/Extended** (toggle): The size of the Cortex deployment - Standard (All DB’s are embedded in the cluster) and Extended (Cortex with select managed services from Azure)
  - **Second Region Location**: for Extended deployment style only. Provides a failover location.

10. Click OK.
11. View and verify your configuration details on the SUMMARY tab.
12. Click OK.
13. On the BUY tab enter a phone number for verification.
14. Click OK.

NOTE: Your Cortex Dedicated Instance deployment process usually takes 20-30 minutes, but sometimes requires over an hour, which is a known Azure specific issue.

## Monitor your deployment

1. Click Resource Groups in the far left panel and search for your Resource Group.
2. Click your group and wait for "provision" to be displayed.
3. Click "provision" in the list.
4. Click "Containers" in the near left panel. An the top left monitor the "Number of Restarts". You will need to click _Refresh_ periodically to monitor this. 

If your deployment has >5 restarts, it is likely to fail. You should abort the process, return to the Resource Groups list, find the groups associated with the your Resource Group and delete them. (Click the three dots on the far right to expose the Delete function.)

Additionally, you can monitor the provisioning logs after the "provision" item has been created in your Resource Group container. 
To monitor the logs, either click the _Logs_ link in the Azure UI on the "Provision" page OR run the following Azure command in your terminal window: `az container logs --follow -g <ResourceGroup> -n provision`.

When your deployment is completely provisioned, a list of the URL logins and other account details are listed in the logs display. Copy this information to a text editor and save it where you can access it later.

## Determine your Base Domain

Your BASE DOMAIN is the DNS Zone. 

1. Go to your Azure portal. 
2. Navigate to your Resource Group from the left panel. 
3. The DNS zone/BASE DOMAIN is displayed.
4. Use the base domain and the following URLs to access Cortex tools and interfaces:

  - **Cortex Admin Console**: https://console.<base_domain>
  - **Cortex Studio Connection Url**: https://api.<base_domain>
  - **Cortex Private registry**: private-registry.<base_domain>
  - **CognitiveScale Marketplace**: https://marketplace.<base_domain>
  - **Cortex CLI**: https://api.<base_domain>
  
## Set up ./kube/config to view cluster through Kubernetes dashboard

1. From your local teminal remove all lines from the .kube/config OR move the file to another name to back it up.
2. Login to Azure portal in your terminal (will direct you to web  browser to login)
3. Run the following command to update your `~/.kube/config` file with your azure credentials: `az aks get-credentials --resource-group <ResourceGroupName>centralus --name aks_cluster_backend --admin`
4. Run the following command to gain access to the Kubernetes dashboard: `kubectl create clusterrolebinding kubernetes-dashboard -n kube-system --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard'
5. Run the following command to start the proxy for the Kubernetes dashboard: `Kubectl proxy --port=9090`

The Kubernetes dashboard can then be reached from here: http://localhost:9090/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/overview?namespace=default

NOTE: Set the namespace to "All Namespaces".

## View the modsec logs for debugging WAF issues

1. Login to your Azure portal.
2. Verify your ./kube/config set up (above)
3. Run the following: `kubectl exec -it $(kubectl get pods --all-namespaces | grep nginx-ingress-controller | awk '{print $2}') -- /bin/bash`
4. Find your log located here: `/var/log/modsec_audit.log`

## SMTP setup

Add the following 3 environment variables to your accounts deployment: 

- SMTP_FROM
- SMTP_EMAIL
- SMTP_FROM_PSWD 

This will cause the accounts pod to recycle, which is expected.

## Troubleshooting Google domains

If the SMTP is set up with Google:

1. [Enable less secure apps][lesssecureapps]
2. [Disable Captcha temporarily so you can connect the new device/server][disablecaptcha]

