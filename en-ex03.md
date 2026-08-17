# Step 3: Connect to the AKS Cluster from the Jump Box and Deploy an Application

Because both API and management connections to an Azure Kubernetes Service (AKS) private cluster are accessible only through private IP addresses, you cannot access the cluster directly over the internet.

To manage the AKS private cluster, connect to the desktop environment of the Jump Box in the same virtual network. From there, either use Azure CLI in a terminal or access the Azure portal using a web browser on the Jump Box.

This guide explains how to install the tools required to connect to the AKS private cluster from the Jump Box, connect to the cluster, and deploy an application.

## Preparation: Install Tools on the Jump Box

Connect to the Jump Box in the virtual network where the AKS private cluster is deployed. Open a terminal and run the following commands to install the two tools required to connect to the cluster.

```powershell
# Install Azure CLI
winget install --exact --id Microsoft.AzureCLI
```

```powershell
# Install kubectl
winget install --exact --id Kubernetes.kubectl
```

After the installation is complete, restart the terminal and run the following commands to verify that Azure CLI and kubectl are installed correctly.

```powershell
# Check the Azure CLI version
az --version
```

```powershell
# Check the kubectl version
kubectl version --client
```

The Jump Box is now ready to connect to the AKS private cluster.

You only need to complete this setup once. Afterward, you can connect to the AKS private cluster by using Azure CLI and kubectl from the Jump Box terminal.

<br>

## Connect to the AKS Private Cluster and Deploy an Application

From the Jump Box terminal, connect to the AKS private cluster and deploy the application provided in the Azure Kubernetes Service (AKS) [quickstart](https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-portal).

Follow these steps.

\[**Steps**▶️\]

1. Run the following command from the Jump Box terminal to sign in to Azure.

    ```powershell
    az login
    ```

2. After signing in to Azure, run the following command to retrieve the AKS private cluster credentials and configure them for use with kubectl.

    ```powershell
    az aks get-credentials --resource-group <resource-group-name> --name <AKS-cluster-name>
    ```
    The [az aks get-credentials](https://learn.microsoft.com/en-us/cli/azure/aks#az-aks-get-credentials) command downloads the credentials and configures the Kubernetes CLI to use them.

3. After retrieving the credentials, run the following command to verify that you can connect to the AKS private cluster.

    ```powershell
    kubectl get nodes
    ```

    Confirm that the \[STATUS\] of each node is **Ready**.

    ![Output of get nodes](img/result-get-node.png)

4. Create the YAML file used to deploy the application.
   
   Copy the contents of [assets/deployApp-aks-private.yaml](./assets/deployApp-aks-private.yaml) from this repository (※), and save the file as `deploy.yaml` in any directory on the Jump Box.

   Verify the following settings in the YAML file:

   * The `image` value in each `Deployment` references GitHub Container Registry (GHCR).
   * Find the comment `# Use internal load-balancer.` and confirm that the load balancer is configured to use an internal load balancer as follows.

    ```yaml
    # Use internal load-balancer.
    annotations:
      service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    ```
    (※) Be sure to use the YAML file from this repository. The YAML file provided in the Azure quickstart configures a public load balancer and therefore cannot be used with an AKS private cluster. It also does not work because of an issue with the version specified in the `order-service` configuration.

5. Run the following command to deploy the application.

    ```powershell
    kubectl apply -f deploy.yaml
    ```
    The Deployments and Services specified in the YAML file are displayed.

    ![Output of apply](img/result-apply.png)

6. Use the [kubectl get pods](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) command to check the status of the deployed pods.

    ```powershell
    kubectl get pods
    ```
    Wait until the \[STATUS\] of every pod is **Running**.

    ![Output of get pods](img/result-get-pods.png)

7. Check the public IP address of the `store-front` application. Use the [kubectl get service](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) command with the `--watch` argument to monitor the progress.

    ```powershell
    kubectl get service store-front --watch
    ```

    Confirm that the `EXTERNAL-IP` value is a local IP address.

    ![Output of get service](img/result-get-service.png)

8. Using a web browser on the Jump Box, access the application at the IP address shown under `EXTERNAL-IP`.

    Example: `http://<EXTERNAL-IP>`

    ![store-front application](img/result-store-front.png)

You have now connected to the AKS private cluster from the Jump Box and deployed an application.

This guide used Azure CLI and kubectl from a terminal in the local Jump Box environment to manage AKS. The same restriction applies when using the Azure portal: you must access the portal from a web browser on the Jump Box.

<br>

👈 [Step 2: Create an Azure Kubernetes Service (AKS) Private Cluster](en-ex02.md)

---

🏚️　[Back to README](README.md)