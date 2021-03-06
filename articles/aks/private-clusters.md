---
title: Create a private Azure Kubernetes Service cluster
description: Learn how to create a private Azure Kubernetes Service (AKS) cluster
services: container-service
ms.topic: article
ms.date: 7/17/2020

---

# Create a private Azure Kubernetes Service cluster

In a private cluster, the control plane or API server has internal IP addresses that are defined in the [RFC1918 - Address Allocation for Private Internet](https://tools.ietf.org/html/rfc1918) document. By using a private cluster, you can ensure network traffic between your API server and your node pools remains on the private network only.

The control plane or API server is in an Azure Kubernetes Service (AKS)-managed Azure subscription. A customer's cluster or node pool is in the customer's subscription. The server and the cluster or node pool can communicate with each other through the [Azure Private Link service][private-link-service] in the API server virtual network and a private endpoint that's exposed in the subnet of the customer's AKS cluster.

## Region availability

Private cluster is available in public regions, Azure Government, and Azure China 21Vianet regions where [AKS is supported](https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service).

> [!NOTE]
> Azure Government sites are supported, however US Gov Texas isn't currently supported because of missing Private Link support.

## Prerequisites

* The Azure CLI version 2.2.0 or later
* The Private Link service is supported on Standard Azure Load Balancer only. Basic Azure Load Balancer isn't supported.  
* To use a custom DNS server, add the Azure DNS IP 168.63.129.16 as the upstream DNS server in the custom DNS server.

## Create a private AKS cluster

### Create a resource group

Create a resource group or use an existing resource group for your AKS cluster.

```azurecli-interactive
az group create -l westus -n MyResourceGroup
```

### Default basic networking 

```azurecli-interactive
az aks create -n <private-cluster-name> -g <private-cluster-resource-group> --load-balancer-sku standard --enable-private-cluster  
```
Where `--enable-private-cluster` is a mandatory flag for a private cluster. 

### Advanced networking  

```azurecli-interactive
az aks create \
    --resource-group <private-cluster-resource-group> \
    --name <private-cluster-name> \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id <subnet-id> \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 
```
Where `--enable-private-cluster` is a mandatory flag for a private cluster. 

> [!NOTE]
> If the Docker bridge address CIDR (172.17.0.1/16) clashes with the subnet CIDR, change the Docker bridge address appropriately.

### Configure Private DNS Zone

The default value is "system", if the --private-dns-zone argument is omitted. AKS will create a Private DNS Zone in the Node Resource Group. Passing the "none" parameter means AKS will not create a Private DNS Zone.  This relies on Bring Your Own DNS Server and configuration of the DNS resolution for the Private FQDN.  If you don't configure DNS resolution,  DNS is only resolvable within the agent nodes and will cause cluster issues after deployment.

## No Private DNS Zone Prerequisites
No PrivateDNSZone
* The Azure CLI version 0.4.67 or later
* The api version 2020-11-01 or later

## Create a private AKS cluster with Private DNS Zone

```azurecli-interactive
az aks create -n <private-cluster-name> -g <private-cluster-resource-group> --load-balancer-sku standard --enable-private-cluster --private-dns-zone [none|system]
```
## Options for connecting to the private cluster

The API server endpoint has no public IP address. To manage the API server, you'll need to use a VM that has access to the AKS cluster's Azure Virtual Network (VNet). There are several options for establishing network connectivity to the private cluster.

* Create a VM in the same Azure Virtual Network (VNet) as the AKS cluster.
* Use a VM in a separate network and set up [Virtual network peering][virtual-network-peering].  See the section below for more information on this option.
* Use an [Express Route or VPN][express-route-or-VPN] connection.

Creating a VM in the same VNET as the AKS cluster is the easiest option.  Express Route and VPNs add costs and require additional networking complexity.  Virtual network peering requires you to plan your network CIDR ranges to ensure there are no overlapping ranges.

## Virtual network peering

As mentioned, virtual network peering is one way to access your private cluster. To use virtual network peering, you need to set up a link between virtual network and the private DNS zone.
    
1. Go to the node resource group in the Azure portal.  
2. Select the private DNS zone.   
3. In the left pane, select the **Virtual network** link.  
4. Create a new link to add the virtual network of the VM to the private DNS zone. It takes a few minutes for the DNS zone link to become available.  
5. In the Azure portal, navigate to the resource group that contains your cluster's virtual network.  
6. In the right pane, select the virtual network. The virtual network name is in the form *aks-vnet-\**.  
7. In the left pane, select **Peerings**.  
8. Select **Add**, add the virtual network of the VM, and then create the peering.  
9. Go to the virtual network where you have the VM, select **Peerings**, select the AKS virtual network, and then create the peering. If the address ranges on the AKS virtual network and the VM's virtual network clash, peering fails. For more information, see  [Virtual network peering][virtual-network-peering].

## Hub and spoke with custom DNS

[Hub and spoke architectures](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) are commonly used to deploy networks in Azure. In many of these deployments, DNS settings in the spoke VNets are configured to reference a central DNS forwarder to allow for on-premises and Azure-based DNS resolution. When deploying an AKS cluster into such a networking environment, there are some special considerations that must be taken into account.

![Private cluster hub and spoke](media/private-clusters/aks-private-hub-spoke.png)

1. By default, when a private cluster is provisioned, a private endpoint (1) and a private DNS zone (2) are created in the cluster-managed resource group. The cluster uses an A record in the private zone to resolve the IP of the private endpoint for communication to the API server.

2. The private DNS zone is linked only to the VNet that the cluster nodes are attached to (3). This means that the private endpoint can only be resolved by hosts in that linked VNet. In scenarios where no custom DNS is configured on the VNet (default), this works without issue as hosts point at 168.63.129.16 for DNS that can resolve records in the private DNS zone because of the link.

3. In scenarios where the VNet containing your cluster has custom DNS settings (4), cluster deployment fails unless the private DNS zone is linked to the VNet that contains the custom DNS resolvers (5). This link can be created manually after the private zone is created during cluster provisioning or via automation upon detection of creation of the zone using event-based deployment mechanisms (for example, Azure Event Grid and Azure Functions).

> [!NOTE]
> If you are using [Bring Your Own Route Table with kubenet](https://docs.microsoft.com/azure/aks/configure-kubenet#bring-your-own-subnet-and-route-table-with-kubenet) and Bring Your Own DNS with Private Cluster, the cluster creation will fail. You will need to associate the [RouteTable](https://docs.microsoft.com/azure/aks/configure-kubenet#bring-your-own-subnet-and-route-table-with-kubenet) in the node resource group to the subnet after the cluster creation failed, in order to make the creation successful.

## Limitations 
* IP authorized ranges can't be applied to the private api server endpoint, they only apply to the public API server
* [Azure Private Link service limitations][private-link-service] apply to private clusters.
* No support for Azure DevOps Microsoft-hosted Agents with private clusters. Consider to use [Self-hosted Agents](https://docs.microsoft.com/azure/devops/pipelines/agents/agents?view=azure-devops&tabs=browser&preserve-view=true). 
* For customers that need to enable Azure Container Registry to work with private AKS, the Container Registry virtual network must be peered with the agent cluster virtual network.
* No support for converting existing AKS clusters into private clusters
* Deleting or modifying the private endpoint in the customer subnet will cause the cluster to stop functioning. 
* Azure Monitor for containers Live Data isn't currently supported.
* After customers have updated the A record on their own DNS servers, those Pods would still resolve apiserver FQDN to the older IP after migration until they're restarted. Customers need to restart hostNetwork Pods and default-DNSPolicy Pods after control plane migration.
* In the case of maintenance on the control plane, your [AKS IP](https://docs.microsoft.com/azure/aks/limit-egress-traffic#:~:text=By%20default%2C%20AKS%20clusters%20have%20unrestricted%20outbound%20%28egress%29,be%20accessible%20to%20maintain%20healthy%20cluster%20maintenance%20tasks.) might change. In this case you must update the A record pointing to the API server private IP on your custom DNS server and restart any custom pods or deployments using hostNetwork.

<!-- LINKS - internal -->
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[private-link-service]: ../private-link/private-link-service-overview.md#limitations
[virtual-network-peering]: ../virtual-network/virtual-network-peering-overview.md
[azure-bastion]: ../bastion/tutorial-create-host-portal.md
[express-route-or-vpn]: ../expressroute/expressroute-about-virtual-network-gateways.md
[devops-agents]: /azure/devops/pipelines/agents/agents?view=azure-devops
[availability-zones]: availability-zones.md