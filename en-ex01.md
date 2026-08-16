# Step 1: Create a Subnet for AKS in an Existing Azure Virtual Network

In this step, you will create a subnet for an Azure Kubernetes Service (AKS) cluster in an existing Azure virtual network. The AKS cluster must run in a dedicated subnet.

This step assumes that the virtual network and Jump Box created by following the guide below already exist. Complete that guide before proceeding.

* [**Build a Jump Box Environment for Secure Access to an Isolated Azure Virtual Network**](https://github.com/osamum/HowtoMake-Az-JumpBox-Env)

Follow these steps to create a subnet for AKS in an existing Azure virtual network.

\[**Steps**▶️\]

1. In the [Azure portal](https://portal.azure.com/), open the page for the target virtual network.
   
2. From the menu on the left, select \[Settings\] - \[**Subnets**\], and then select \[**+ Subnet**\] from the menu at the top of the page.

   ![Add a subnet to the virtual network](img/EN-vnet-jpwest-addSubnet.png)

3. The \[**Add subnet**\] pane appears on the left. Configure each setting as follows.

    |Setting|Value|
    |:---|:---|
    |Purpose|Default|
    |Name \*|`poc-aks-subnet`|
    |Include an IPv4 address space|**Checked**|
    |IPv4 address range|(Keep the default value)|
    |Starting address \*|(Keep the default value)|
    |Size|(Keep the default value)|
    |Include an IPv6 address space|Not checked|
    |Enable private subnet (no default outbound access)|Checked|
    |NAT gateway|Select `internet-gatway` (※)|
    |Network security group|None|
    |Route table|None|
    |Service endpoints|Do not specify|
    |Subnet delegation|None|
    |Private endpoint network policy|Disabled|

    ![Subnet settings](img/EN-vnet-add-subnet.png)

    (※) Select the NAT gateway created by following [Create a Virtual Network Environment](https://github.com/osamum/HowtoMake-Az-JumpBox-Env/blob/main/en-ex01.md).

    After configuring the settings, select \[**Add**\] at the bottom of the page to create the subnet.


You have now added a subnet for AKS to the existing virtual network.

<br>

## Next

👉　[**Step 2: Create an Azure Kubernetes Service (AKS) Private Cluster**](en-ex02.md)

---

🏚️　[Back to README](README.md)