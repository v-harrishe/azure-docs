---
title: Deploy and configure Azure Firewall using Azure CLI
description: In this article, you learn how to deploy and configure Azure Firewall using the Azure CLI. 
services: firewall

ms.service: firewall
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.topic: how-to
#Customer intent: As an administrator new to this service, I want to control outbound network access from resources located in an Azure subnet.
---

# Deploy and configure Azure Firewall using Azure CLI

Controlling outbound network access is an important part of an overall network security plan. For example, you may want to limit access to web sites. Or, you may want to limit the outbound IP addresses and ports that can be accessed.

One way you can control outbound network access from an Azure subnet is with Azure Firewall. With Azure Firewall, you can configure:

* Application rules that define fully qualified domain names (FQDNs) that can be accessed from a subnet. The FQDN can also [include SQL instances](sql-fqdn-filtering.md).
* Network rules that define source address, protocol, destination port, and destination address.

Network traffic is subjected to the configured firewall rules when you route your network traffic to the firewall as the subnet default gateway.

For this article, you create a simplified single VNet with three subnets for easy deployment. For production deployments, a [hub and spoke model](https://docs.azure.cn/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) is recommended. The firewall is in its own VNet. The workload servers are in peered VNets in the same region with one or more subnets.

* **AzureFirewallSubnet** - the firewall is in this subnet.
* **Workload-SN** - the workload server is in this subnet. This subnet's network traffic goes through the firewall.
* **Jump-SN** - The "jump" server is in this subnet. The jump server has a public IP address that you can connect to using Remote Desktop. From there, you can then connect to (using another Remote Desktop) the workload server.

![Tutorial network infrastructure](media/tutorial-firewall-rules-portal/Tutorial_network.png)

In this article, you learn how to:

* Set up a test network environment
* Deploy a firewall
* Create a default route
* Configure an application rule to allow access to www.qq.com
* Configure a network rule to allow access to external DNS servers
* Test the firewall

If you prefer, you can complete this procedure using the [Azure portal](tutorial-firewall-deploy-portal.md) or [Azure PowerShell](deploy-ps.md).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

- This article requires version 2.0.4 or later of the Azure CLI. If using Azure local Shell, the latest version is already installed.

## Set up the network

First, create a resource group to contain the resources needed to deploy the firewall. Then create a VNet, subnets, and test servers.

### Create a resource group

The resource group contains all the resources for the deployment.

```azurecli
az group create --name Test-FW-RG --location chinaeast
```

### Create a VNet

This virtual network has three subnets.

> [!NOTE]
> The size of the AzureFirewallSubnet subnet is /26. For more information about the subnet size, see [Azure Firewall FAQ](firewall-faq.md#why-does-azure-firewall-need-a-26-subnet-size).

```azurecli
az network vnet create \
  --name Test-FW-VN \
  --resource-group Test-FW-RG \
  --location chinaeast \
  --address-prefix 10.0.0.0/16 \
  --subnet-name AzureFirewallSubnet \
  --subnet-prefix 10.0.1.0/26
az network vnet subnet create \
  --name Workload-SN \
  --resource-group Test-FW-RG \
  --vnet-name Test-FW-VN   \
  --address-prefix 10.0.2.0/24
az network vnet subnet create \
  --name Jump-SN \
  --resource-group Test-FW-RG \
  --vnet-name Test-FW-VN   \
  --address-prefix 10.0.3.0/24
```

### Create virtual machines

Now create the jump and workload virtual machines, and place them in the appropriate subnets.
When prompted, type a password for the virtual machine.

Create the Srv-Jump virtual machine.

```azurecli
az vm create \
    --resource-group Test-FW-RG \
    --name Srv-Jump \
    --location chinaeast \
    --image win2016datacenter \
    --vnet-name Test-FW-VN \
    --subnet Jump-SN \
    --admin-username azureadmin
az vm open-port --port 3389 --resource-group Test-FW-RG --name Srv-Jump
```



Create a NIC for Srv-Work with specific DNS server IP addresses and no public IP address to test with.

```azurecli
az network nic create \
    -g Test-FW-RG \
    -n Srv-Work-NIC \
   --vnet-name Test-FW-VN \
   --subnet Workload-SN \
   --public-ip-address "" \
   --dns-servers 209.244.0.3 209.244.0.4
```

Now create the workload virtual machine.
When prompted, type a password for the virtual machine.

```azurecli
az vm create \
    --resource-group Test-FW-RG \
    --name Srv-Work \
    --location chinaeast \
    --image win2016datacenter \
    --nics Srv-Work-NIC \
    --admin-username azureadmin
```

## Deploy the firewall

Now deploy the firewall into the virtual network.

```azurecli
az network firewall create \
    --name Test-FW01 \
    --resource-group Test-FW-RG \
    --location chinaeast
az network public-ip create \
    --name fw-pip \
    --resource-group Test-FW-RG \
    --location chinaeast \
    --allocation-method static \
    --sku standard
az network firewall ip-config create \
    --firewall-name Test-FW01 \
    --name FW-config \
    --public-ip-address fw-pip \
    --resource-group Test-FW-RG \
    --vnet-name Test-FW-VN
az network firewall update \
    --name Test-FW01 \
    --resource-group Test-FW-RG 
az network public-ip show \
    --name fw-pip \
    --resource-group Test-FW-RG
fwprivaddr="$(az network firewall ip-config list -g Test-FW-RG -f Test-FW01 --query "[?name=='FW-config'].privateIpAddress" --output tsv)"
```

Note the private IP address. You'll use it later when you create the default route.

## Create a default route

Create a table, with BGP route propagation disabled

```azurecli
az network route-table create \
    --name Firewall-rt-table \
    --resource-group Test-FW-RG \
    --location chinaeast \
    --disable-bgp-route-propagation true
```

Create the route.

```azurecli
az network route-table route create \
  --resource-group Test-FW-RG \
  --name DG-Route \
  --route-table-name Firewall-rt-table \
  --address-prefix 0.0.0.0/0 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address $fwprivaddr
```

Associate the route table to the subnet

```azurecli
az network vnet subnet update \
    -n Workload-SN \
    -g Test-FW-RG \
    --vnet-name Test-FW-VN \
    --address-prefixes 10.0.2.0/24 \
    --route-table Firewall-rt-table
```

## Configure an application rule

The application rule allows outbound access to www.qq.com.

```azurecli
az network firewall application-rule create \
   --collection-name App-Coll01 \
   --firewall-name Test-FW01 \
   --name Allow-QQ \
   --protocols Http=80 Https=443 \
   --resource-group Test-FW-RG \
   --target-fqdns www.qq.com \
   --source-addresses 10.0.2.0/24 \
   --priority 200 \
   --action Allow
```

Azure Firewall includes a built-in rule collection for infrastructure FQDNs that are allowed by default. These FQDNs are specific for the platform and can't be used for other purposes. For more information, see [Infrastructure FQDNs](infrastructure-fqdns.md).

## Configure a network rule

The network rule allows outbound access to two IP addresses at port 53 (DNS).

```azurecli
az network firewall network-rule create \
   --collection-name Net-Coll01 \
   --destination-addresses 209.244.0.3 209.244.0.4 \
   --destination-ports 53 \
   --firewall-name Test-FW01 \
   --name Allow-DNS \
   --protocols UDP \
   --resource-group Test-FW-RG \
   --priority 200 \
   --source-addresses 10.0.2.0/24 \
   --action Allow
```

## Test the firewall

Now, test the firewall to confirm that it works as expected.

1. Note the private IP address for the **Srv-Work** virtual machine:

   ```azurecli
   az vm list-ip-addresses \
   -g Test-FW-RG \
   -n Srv-Work
   ```

1. Connect a remote desktop to **Srv-Jump** virtual machine, and sign in. From there, open a remote desktop connection to the **Srv-Work** private IP address and sign in.

3. On **SRV-Work**, open a PowerShell window and run the following commands:

   ```
   nslookup www.qq.com
   nslookup www.microsoft.com
   ```

   Both commands should return answers, showing that your DNS queries are getting through the firewall.

1. Run the following commands:

   ```
   Invoke-WebRequest -Uri https://www.qq.com
   Invoke-WebRequest -Uri https://www.qq.com

   Invoke-WebRequest -Uri https://www.microsoft.com
   Invoke-WebRequest -Uri https://www.microsoft.com
   ```

   The `www.qq.com` requests should succeed, and the `www.microsoft.com` requests should fail. This demonstrates that your firewall rules are operating as expected.

So now you've verified that the firewall rules are working:

* You can resolve DNS names using the configured external DNS server.
* You can browse to the one allowed FQDN, but not to any others.

## Clean up resources

You can keep your firewall resources for the next tutorial, or if no longer needed, delete the **Test-FW-RG** resource group to delete all firewall-related resources:

```azurecli
az group delete \
  -n Test-FW-RG
```

## Next steps

* [Tutorial: Monitor Azure Firewall logs](./firewall-diagnostics.md)


<!-- Update_Description: new article about deploy cli -->
<!--NEW.date: 12/21/2020-->