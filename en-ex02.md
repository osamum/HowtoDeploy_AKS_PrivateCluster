# Step 2: Create an Azure Kubernetes Service (AKS) Private Cluster

In this step, you will create an Azure Kubernetes Service (AKS) private cluster in an existing Azure virtual network.

Currently, [Azure Kubernetes Service (AKS) Automatic](https://learn.microsoft.com/en-us/azure/aks/intro-aks-automatic) is recommended when creating a general-purpose Azure Kubernetes Service (AKS) cluster. However, because using it would make this procedure more complex, this guide uses the traditional approach.

Note that the AKS cluster created in this step can only be managed through the Jump Box deployed in the virtual network.

\[**Steps**▶️\]

1. Sign in to the [Azure portal](http://portal.azure.com), and select the \[**+**\] Create a resource icon at the top of the portal. If the icon is not displayed, select the hamburger menu in the upper-left corner, and then select \[**Create a resource**\].

    ![Create a resource](img/EN-create_AzureResource.png)

2. On the \[**Create a resource**\] page, enter `kubernetes` in the search box, and then select the \[**Azure Kubernetes Service (AKS)**\] tile in the search results.

    ![Search for Azure Kubernetes Service](img/EN-AKS_tail.png)

3. On the \[**Azure Kubernetes Service (AKS)**\] page, select \[**Create**\].


4. On the \[**Basics**\] tab of the \[**Create Azure Kubernetes Service**\] page, configure each setting as follows.

    **Project details**

    |Setting|Value|
    |:---|:---|
    |Subscription \*|The subscription to use|
    |Resource group|\[*Any resource group*\]|

    **Cluster details**

    |Setting|Value|
    |:---|:---|
    |Preset configuration \*|\[**Dev/Test**\] (※1)|
    |Kubernetes cluster name \*|`PoC-private-aks`|
    |Region \*|\[**(Asia Pacific) Japan West**\]|
    |Fleet manager|\[**None**\]|
    |Availability zones|\[**None**\]|
    |AKS pricing tier \*|\[**Free**\] (※2)|
    |Enable long-term support|Not checked|
    |Kubernetes version|*Keep the default value*|
    |Automatic upgrade|\[**Node image**\]|
    |Security channel scheduler|\[**Weekly on Sunday (recommended)**\]|
    |Authentication and authorization|\[**Local accounts with Kubernetes RBAC**\]|
    
    ![Create AKS - Basics tab](img/EN-create-AKS-basic.png)

    (※1)(※2) Because this environment is for learning, the Dev/Test preset configuration and Free pricing tier are used to reduce costs. For a production environment, select an appropriate configuration based on your requirements.

    After configuring the settings, select \[**Next**\] at the bottom of the page.

5. On the **Node pools** page, configure each setting as follows.

    **Node pool auto-provisioning**

    |Setting|Value|
    |:---|:---|
    |Enable node auto-provisioning|**Not checked**|

    **Node pools**

    Add a node pool.

    If **userpool** has been added by default, you may use it. However, if you want fewer nodes to reduce costs for this learning environment, select the default **userpool**, and then select \[**Delete**\] to remove it.

    ![Create AKS - Node pools tab](img/EN-delete-userpool.png)

    Select \[**+ Add node pool**\], and then select \[**Add a Virtual Machine Scale Set node pool**\] from the displayed menu.

    ![Create AKS - Add node pool](img/EN-add-nodepool.png)

    On the **Add node pool** page, configure each setting as follows.

    |Setting|Value|
    |:---|:---|
    |Node pool name \*|`poclinux`|
    |Mode|\[**User**\]|
    |OS type|\[**Linux**\]|
    |OS SKU|\[**Ubuntu Linux**\]|
    |Availability zones|\[**None**\]|
    |Enable Azure Spot instances|**Not checked**|
    |Node size \*|\[**Standard D2ls v5, 2 vCPUs, 4 GiB memory**\]|
    |Scale method|**Autoscale** - recommended|
    |Minimum node count \*|1|
    |Maximum node count \*|20|

    ![Add node pool](img/EN-addnodepool-settings.png)

    After configuring the settings, select \[**Add**\] at the bottom of the page.

    When you return to the \[**Node pools**\] page, select \[**Next**\] at the bottom of the page.

6. On the **Networking** tab, configure each setting as follows.

    |Setting|Value|
    |:---|:---|
    |Enable private cluster|**Checked**|
    |Public access - Set authorized IP ranges|Disabled|
    |Container networking - Network configuration|\[**Azure CNI Overlay**\]|
    |Bring your own Azure virtual network|**Checked**|
    |Virtual network \*|\[`PoC-jpwest-vnet`\] (※1)|
    |Cluster subnet \*|\[`poc-aks-subnet`\] (※2)|
    |User-assigned managed identity \*|Select \[**Create new**\] to create one|
    |Kubernetes service address range \*|`172.16.0.0/16` (※3)|
    |Kubernetes DNS service IP address \*|`172.16.0.10` (※4)|
    |DNS name prefix \*|*Keep the default name*|
    |Enable Cilium data plane and network policy engine|Not checked|
    |Network policy engine|\[**None**\]|

    ![](img/EN-AKS-networkSettings.png)


    (※1) Select the previously created virtual network, `PoC-jpwest-vnet`.  

    (※2) Select the previously created subnet for the AKS cluster, `poc-aks-subnet`.

    (※3) The Kubernetes service address range must not overlap with the address space of any virtual network subnet. The default value, `10.10.0.0/16`, overlaps with the virtual network subnet address space, so this example specifies `172.16.0.0/16`.
    
    (※4) Specify the Kubernetes DNS service IP address within the Kubernetes service address range.

    After configuring the settings, select \[**Review + create**\] at the bottom of the page. When \[**Create**\] appears, select it to start creating the AKS cluster.

You have now created an Azure Kubernetes Service (AKS) private cluster. Cluster creation may take several minutes, so wait for it to finish before proceeding to the next step.

<br>

## Next

👉 [Step 3: Connect to the AKS Cluster from the Jump Box and Deploy an Application](en-ex03.md)

👈 [Step 1: Create a Subnet for AKS in an Existing Azure Virtual Network](en-ex01.md)

---

🏚️　[Back to README](README.md)