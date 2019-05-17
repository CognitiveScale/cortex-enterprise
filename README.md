# Dedicated Cortex Instance

[0613da0c]: https://portal.azure.com/#create/cognitive-scale.cognitivescale-cortex5-ai-previewcortex-ai-platform 
[b6d1b728]: https://docs.microsoft.com/en-us/azure/architecture/cloud-adoption/getting-started/azure-resource-access
[azure-portal]: https://portal.azure.com/#home
[Azure-account]: https://azure.microsoft.com/en-us/free/search/?&OCID=AID719825_SEM_TZhJDdn3&lnkd=Google_Azure_Brand&gclid=EAIaIQobChMI--_74KfS4QIVzMDACh2KVwTuEAAYASAAEgJLK_D_BwE
[dns-getting-started]: https://docs.microsoft.com/en-us/azure/dns/dns-getstarted-portal
[dns-faq]: https://docs.microsoft.com/en-us/azure/dns/dns-faq
[azure-cli]: https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest
[vCPU-quota-requests]: https://docs.microsoft.com/en-us/azure/azure-supportability/resource-manager-core-quotas-request
[lesssecureapps]: https://www.google.com/settings/security/lesssecureapps
[disablecaptcha]: https://accounts.google.com/b/0/displayunlockcaptcha
[subscription-help]: https://docs.microsoft.com/en-us/azure/billing/billing-getting-started
[service principal]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-auth-service-principal
[setup-smtp-server]: http://guidestomicrosoft.com/2016/02/17/configure-a-smtp-server-in-azure/
[[resource-group-cli]]: https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest

## Table of Contents

[What is Dedicated Cortex Instance?](https://github.com/CognitiveScale/cortex-enterprise#what-is-dedicated-cortex-instance)

[Azure and Dedicated Cortex Instance architecture](https://github.com/CognitiveScale/cortex-enterprise#azure-and--dedicated-cortex-instance-architecture)

[Prerequisites for launching Dedicated Cortex Instance](https://github.com/CognitiveScale/cortex-enterprise#prerequisites-for-launching-dedicated-cortex-instance)

* [Before you begin working in Azure](https://github.com/CognitiveScale/cortex-enterprise#before-you-begin-working-in-azure)

* [Azure permissions required to perform an installation](https://github.com/CognitiveScale/cortex-enterprise#azure-permissions-required-to-perform-an-installation)

* [Deployment details](https://github.com/CognitiveScale/cortex-enterprise#deployment-details)

* [Prerequisite steps](https://github.com/CognitiveScale/cortex-enterprise#prerequisite-steps)

[Configure Cortex Dedicated Instance](https://github.com/CognitiveScale/cortex-enterprise#configure-cortex-dedicated-instance)

[Monitor your deployment](https://github.com/CognitiveScale/cortex-enterprise#monitor-your-deployment)

[Determine your Base Domain](https://github.com/CognitiveScale/cortex-enterprise#determine-your-base-domain)

[Set up ./kube/config to view cluster through Kubernetes dashboard](https://github.com/CognitiveScale/cortex-enterprise#set-up-kubeconfig-to-view-cluster-through-kubernetes-dashboard)

[View the modsec logs for debugging WAF issues](https://github.com/CognitiveScale/cortex-enterprise#view-the-modsec-logs-for-debugging-waf-issues)

[Troubleshoot Google domains](https://github.com/CognitiveScale/cortex-enterprise#troubleshooting-google-domains)

[Upgrade your DCI cluster](https://github.com/CognitiveScale/cortex-enterprise#how-to-upgrade-your-dci-cluster)

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

### Before you begin working in Azure:

- Create DNS domain at domain registrar like Google, GoDaddy, Name Cheap, or Route 53. You will need this to create the main DNS Zone. (BASE DOMAIN)
- Verify that you have the following CLI utilities installed: (On Mac use homebrew installer to install the utilities: `brew tap derailed/k9s && brew install kubectl azure-cli jq k9s`)

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

Before you begin you must have a registered Internet domain name (e.g. example.com) with a domain name registrar (e.g. AWS, godaddy.com, name.com, etc.). This will be used to configure your main DNS Zone (BASE DOMAIN).


1. Learn about Azure: [Getting Started with Azure][b6d1b728].
2. Go to Azure Marketplace and create a [subscription][subscription-help] - Select a "Pay" Azure subscription or use your existing Azure subscription.
3. Get started in the Azure portal:

   - a. [Launch the Azure CLI][azure-cli]. 
   - b. Select your subscription and opt to create storage account.
   - c. Select to run `-bash` (or `-powershell` Instructions for Powershell users can be found in MS Azure help.
   - d. When the terminal opens, list your subscriptions: `az account list`.
   - e. Set your subscription for future sessions: `az account set --subscription my-subscription-name`.

4. Create your main Resource Group - [Use the Azure CLI to Create a Resource Group][resource-group-cli]. 
  
  **Create the Resource Group using Azure CLI**: 
  
   `az group create --location
                  --name
                  [--subscription]
                  [--tags]`
          
   Where the parameters are as follows:
   `--location -l Location.` 
   Values from: az account list-locations. You can configure the default location using az configure --defaults location=<location>.

  `--name --resource-group -g -n`
    Name of the new resource group.

  Optional Parameters
  `--subscription`
  Name or ID of subscription. You can configure the default subscription using az account set -s NAME_OR_ID.

  `--tags`
  Space-separated tags in 'key[=value]' format. Use "" to clear existing tags.
  
  - OR -
  
  **Create the Resource Group in the Azure UI**:
  
  - a. Click **Resource Group** in the left panel.
  - b. Click **Add** in the tool bar above the table.
  - c. In the form select your Subscription, enter your main Resource Group name, and select the "CentralUS" region.
  - d. Click **Review and Create** at the bottom of the page.
  - e. Click **Create** at the bottom of the page.
  
5. Under this Resource Group create your main DNS zone (Domain). [Create your main DNS Zone][dns-getting-started] ([More information about DNS Zones in Azure][dns-faq]
  - a. Click **Create Resource** at the top of the left panel and enter "DNS Zone" in the search bar.
  - b. Select **DNS Zone** from the list and click **Create**.
  - c. In the form enter the following:

   - **Subscription**: Your subscription should be pre-selected. If it is not, select it.
   - **Resource group**: Click **Create New** and enter the name of the Resource Group you created above.
    - **Instance Details: Name**: Enter your registered Internet domain name as the DNS Zone name.
    
  - d. Click **Review and Create** at the bottom of the page.
  - e. Click **Create** at the bottom of the page.

**NOTE**: Later, when you create your Dedicated Cortex instance you will create another Resource Group and DNS Zone; these will be specifically for your Cortex Instantiation. The Resource Group and DNS Zone that your create as prerequisites will be referenced in the Dedicated Cortex Instantiation setup.

6. Remove AZ vCPU limits in Azure: See [Resource Manager vCPU quota increase requests][vCPU-quota-requests]. 
7. [Setup your Azure AD Service Principal][service principal]. 

  - a.) Sign in to your Azure Account through the Azure portal.
  - b.) Select **Azure Active Directory** in the left panel.
  - c.) Select **App registrations** in the center panel.
  - d.) Select **New application registration** in the top tool bar.
  - e.) Enter the **Name** (The user-facing display name for this application, which can be changed later)
  - f.) Select the **Supported account types** (Who can use this application or access this API?) Typically, select the first option: "Accounts in this organizational directory only."
  - g.) Enter **Redirect URI** for the application. Select "Web" as the type of URI and enter domain you created earlier. 
  - h.) Click **Create**.
  - i.) In the middle panel click **API permissions** and add the following:
    - 1) In the table displayed click **Microsoft graph**. A list will open on the right.
    - 2) At the top of the list click the box labelled **Delegated permissions**.
    - 3) Click **Directory** in the list to open the selectable options.
    - 4) Check **Directory.Read.All**. 
    - 5) At the bottom of the list click **Update Permissions**. The Directory.Read.All permission is added to the table on the left.
    - 6) In the table click **Microsoft graph** again. 
    - 7) At the top of the list click the box labelled **Application permissions**.
    - 8) Click **Directory** in the list to open the selectable options.
    - 9) Check **Directory.Read.All**. 
    - 10) At the bottom of the list click **Update Permissions**. The Directory.Read.All permission is added to the table on the left.
    - **NOTE**: The **User.Read** permission is added to Microsoft Graph by default.
      
  - j.) Under the permissions table click the **Grant admin consent for < account >** button.

    Your API permissions should be as follows:

    | **API/Permission Name** | **Type** | **Description** | **Admin Consent Required** |
    | --- | --- | --- | --- |
    | Directory.Read.All | Delegated | Read directory data | Yes. Granted for CognitivesScale |
    | Directory.Read.All | Application | Read directory data | Yes. Granted for CognitivesScale |
    | User.Read | Delegated | Sign in and read user profile | Yes. Granted for CognitivesScale |
 
  - k.) Return to **App registration** via the breadcrumbs and find your App name in the list. Click to open your app details.
  - l.) From the metadata at the top of the page copy the **Application ID (client)** and paste in a text file. You will enter this as the **Azure AD Service Principal App ID** when you configure your DCI cluster.
  - m.) In the middle panel click **Certificates and secrets**.
  - n.) Under "Client Secrets" click **New Client Secret**.
  - o.) In the modal's Description field enter "Azure AD Service Principal App password" or any other description.
  - p.) Select and expiration period.
  - q.) Click **Add**
  - r.) Copy the value from the table displayed and paste it in a text file. You will enter this as the **Azure AD Service Principal App Password** when you configure your DCI cluster.

8. Receive CognitiveScale welcome email that provides the `cortex license key`, which provides the proxy access to the Cortex Documentation and Cortex Marketplace web portals.

After these prerequisite steps are completed, you are ready to configure your Dedicated Cortex Instance in Azure Marketplace, deploy it, and verify the deployment by logging in to their Kubernetes dashboard.  Your CognitiveScale CS1 and SRE representatives are available to provide assistance and direction throughout the Enterprise deployment process.

## Configure Cortex Dedicated Instance  

1. Login to your [Azure portal][azure-portal] 

  - a. Click "Create a resource" at the top of the left panel
  - b. Search for "Cortex Cognitive Platform" and click the first option listed
  - c. OR go to [CognitiveScale Cortex Cognitive Platform][0613da0c]
  
2. Right click on the browser tab and select "Duplicate". You will need to have two Azure portal tabs open to complete the instantiation process in the proper order. 
3. (In the first tab) Click **Create**.
4. (In the first tab) On the BASICS page enter:

  - **Cortex Cluster Name**: User-created name for the cluster, used as an organization name for the deployment
  - **VM SSH Public Key**: The Administrator SSH Public Key to install on AKS nodes. 
  
    - In your local terminal run`cat ~/.ssh/id_rsa.pub`
    - You will be prompted to enter a location where the key will be saved
    - Go to the local folder and open it
    - Open the public key in a text editor window and copy it
    - Past the key in the Azure UI field
    
  - **Cortex License Key**: License Key for your Dedicated Cortex Instance sent from your CognitiveScale representative. 
  
    - Copy from the Cortex Welcome email you received and paste in the Azure UI field
    
  - **Subscription**: The name of your subscription created as a prerequisite.
  - **Resource Group**: A collection that shares the same lifecycle, permissions, and policies.
  
    - a. Click the "Create New" link.
    - b. Enter a RG name (you may use the same name used for the Cortex Cluster name). 
    - c. Click "OK".
    - d. Jot down the Resource Group name you entered. (you will need to enter it again exactly in step 8 below and you cannot return to the tab after you leave it.
    
  - **Location**: A selectable list of geographic locations for your account/business. 
    - Use `centralus` for pay-as-you-go.
  
5. (In the first tab) Click **OK**

6. (In the first tab) On the ADMIN DETAILS page enter:

  - **Cortex Administrator Email Address**: The administrator must have the permissions described in Prerequisites.
  - **Cortex Administrator Password**: Use a secure password and make note of it.
  - **Confirm Password**: Duplicated the password exactly.
  - **Cortex Account Name**: Enter your desired Account Name.

7. (In the first tab) Click OK. **STOP**

8. (In the second tab) Click **Create Resource** at the top of the left panel and enter "DNS Zone" in the search bar.
9. (In the second tab) Select **DNS Zone** from the list and click **Create**.
10. (In the second tab) In the form enter the following:

  - **Subscription**: Your subscription should be pre-selected. If it is not, select it.
  - **Resource group**: Click **Create New** and enter the SAME characters you entered for the Resource Goup in the first tab. **NOTE**: DO NOT move back through the pages in the first tab; you will invalidate the Resource Group, and you will need to start over.
  - **Instance Details: Name**: Enter the DNS Zone name you to use as the subdomain for this cluster plus the main DNS ZONE Domain you created in the prerequisites. my-rd-dns.main-dns-zone 
  
11. (In the second tab) Click **Review and Create** at the bottom of the page.
12. (In the second tab) Click **Create** at the bottom of the page.
13. (In the second tab) When the DNS Zone is added to the list of resources click on it to go to your DNS Zone detail page. Copy the DNS Zone name to a text editor. You will need to enter it in the first tab. 
  
  Two records are listed in the table, NS and SOA types. You need to populate the table with the 4 Name Servers that are listed in the metadata at the top right. 
  
14. Duplicate the second tab and in the third tab click **Add Record Set**.
15 (In the third tab) Enter the following in the modal fields for each of the Name Servers 1-4:
  - **Name**: Copy the value of **Name server #**
  - **Type**: Select "NS"
  - **TTL and TTL Unit**: Enter the "time-to-live" TTL, a setting for each DNS record that specifies how long a resolver is supposed to cache the DNS query before the query expires and a new one needs to be done.
  - **Name Server**: Enter the main DNS Zone Domain from your prerequisites setup.
  - Click **OK** at the bottom of the modal.

16. Close the second and third tabs.

17. (On the first tab) On the SERVICE PRINCIPLE DETAILS page enter:

  - **Azure AD Service Principle App ID**: The Service Principle Application ID to use for Azure resource creation from Kubernetes.
  - **Azure AD Service Principle Password**: The Service Principle Password to use for Azure resource creation from Kubernetes.

18. Click **OK**.

19. On the DEPLOYMENT DETAILS page enter:

  - **DNS Zone**: Copy the Zone name from the text editor where you saved it when you copied it from the second tab.
  - **Virtual Machine Count**: select the number of VMs to be used as AKS nodes.
  - **Size: Change size** (link): displays a selectable table of all of the deployment size options and price estimations.
  - **Deployment style: Standard/Extended** (toggle): The size of the Cortex deployment - Standard (All DB’s are embedded in the cluster) and Extended (Cortex with select managed services from Azure)
  - **Second Region Location**: for Extended deployment style only. Provides a failover location.

20. Click **OK**.
21. View and verify your configuration details on the SUMMARY tab.
22. Click **OK**.
23. On the BUY tab enter the admin name and phone number for verification.
24. Click **OK**.

**NOTE**: Your Cortex Dedicated Instance deployment process usually takes 20-30 minutes, but sometimes requires over an hour, which is a known Azure specific issue.

## Monitor your deployment

1. Click Resource Groups in the far left panel and search for your Resource Group.
2. Click your group and wait for "provision" to be displayed.
3. Click "provision" in the list.
4. Click "Containers" in the near left panel. An the top left monitor the "Number of Restarts". You will need to click _Refresh_ periodically to monitor this. 

If your deployment has >5 restarts, it is likely to fail. You should abort the process, return to the Resource Groups list, find the groups associated with the your Resource Group and delete them. (Click the three dots on the far right to expose the Delete function.)

Additionally, you can monitor the provisioning logs after the "provision" item has been created in your Resource Group container. 
To monitor the logs, either click the **Logs** link in the Azure UI on the PROVISION page. You need to scroll to follow the progress of the logs

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
  
  NOTE: The URLs for the above Cortex tools are displayed in the output of the provision logs.
  
## Set up ./kube/config to view cluster through Kubernetes dashboard

1. Login to Azure in your terminal `az login` (a web  browser opens where you select your subscription)
2. Run the following command to update your `~/.kube/config` file with your azure credentials: `az aks get-credentials --resource-group <ResourceGroupName>centralus --name aks_cluster_backend --admin` (The only variable you replace in this script is the ResourceGroupName
3. Run the following command to gain access to the Kubernetes dashboard: `kubectl create clusterrolebinding kubernetes-dashboard -n kube-system --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard' 
4. Run the following command to start the proxy for the Kubernetes dashboard: `Kubectl proxy --port=9090`

  The Kubernetes dashboard can then be reached from this URL: http://localhost:9090/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/overview?namespace=default

5. After you open the Kubernetes dashboard you must set the Namespace (open it from the left panel) to "All Namespaces".

## Debugging WAF issues

1. Login to the Azure `az login`.
2. Open your Kubernetes dashboard using the link above. 
3. Verify that you have selected "All Namespaces."
3. Run the following: `kubectl exec -it $(kubectl get pods --all-namespaces | grep nginx-ingress-controller | awk '{print $2}') -- /bin/bash`
4. Find your log located here: `/var/log/modsec_audit.log`

If the SMTP is set up with Google:

**NOTE**: [How to set up an SMTP server in Azure][setup-smtp-server]. 

1. [Enable less secure apps][lesssecureapps]
2. [Disable Captcha temporarily so you can connect the new device/server][disablecaptcha]
3. Add the following 3 environment variables to your accounts deployment: 

 - SMTP_FROM
 - SMTP_EMAIL
 - SMTP_FROM_PSWD 

This will cause the accounts pod to recycle, which is expected.

## Upgrade your DCI cluster

**WARNING**: Upgrades should be performed during a scheduled downtime. If the upgrade process fails, the cluster reverts to a pre-upgrade snapshot, in which case data loss may occur if processes are running during the upgrade. 

A galaxy admin performs the upgrade from the bastion server. 

1. SSH into the bastion host `azureuser@<bastion-host-ip>`
2. Authenticate with the public SSH key entered during provisioning.  

  **NOTE**: The `bastion-host-ip`0000 is available under the primary resource group used to deploy the environment, under object name `bastion-nic`.

3. Execute `./upgrade_cortex.sh`

  To upgrade to a specific version you may add the `-v <version #>` variable.(`./upgrade_cortex.sh -v 1.1.0`)
