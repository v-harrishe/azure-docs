---
title: Quickstart - Create an internal load balancer - Azure CLI
titleSuffix: Azure Load Balancer
description: This quickstart shows how to create an internal load balancer using the Azure CLI
services: load-balancer
documentationcenter: na

manager: KumudD
Customer intent: I want to create a load balancer so that I can load balance internal traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.custom: mvc, devx-track-js, devx-track-azurecli
---
# Quickstart: Create an internal load balancer to load balance VMs using Azure CLI

Get started with Azure Load Balancer by using Azure CLI to create a public load balancer and three virtual machines.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)] 

- This quickstart requires version 2.0.28 or later of the Azure CLI. If using Azure local Shell, the latest version is already installed.

## Create a resource group

An Azure resource group is a logical container into which Azure resources are deployed and managed.

Create a resource group with [az group create](https://docs.azure.cn/cli/group#az_group_create):

* Named **CreateIntLBQS-rg**. 
* In the **chinaeast** location.

```azurecli
  az group create \
    --name CreateIntLBQS-rg \
    --location chinaeast
```
---

# [**Standard SKU**](#tab/option-1-create-load-balancer-standard)

>[!NOTE]
>Standard SKU load balancer is recommended for production workloads. For more information about SKUs, see **[Azure Load Balancer SKUs](skus.md)**.

## Configure virtual network

Before you deploy VMs and deploy your load balancer, create the supporting virtual network resources.

### Create a virtual network

Create a virtual network using [az network vnet create](https://docs.azure.cn/cli/network/vnet#az_network_vnet_createt):

* Named **myVNet**.
* Address prefix of **10.1.0.0/16**.
* Subnet named **myBackendSubnet**.
* Subnet prefix of **10.1.0.0/24**.
* In the **CreateIntLBQS-rg** resource group.
* Location of **chinaeast**.

```azurecli
  az network vnet create \
    --resource-group CreateIntLBQS-rg \
    --location chinaeast \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name myBackendSubnet \
    --subnet-prefixes 10.1.0.0/24
```
### Create a network security group

For a standard load balancer, the VMs in the backend address for are required to have network interfaces that belong to a network security group. 

Create a network security group using [az network nsg create](https://docs.azure.cn/cli/network/nsg#az_network_nsg_create):

* Named **myNSG**.
* In resource group **CreateIntLBQS-rg**.

```azurecli
  az network nsg create \
    --resource-group CreateIntLBQS-rg \
    --name myNSG
```

### Create a network security group rule

Create a network security group rule using [az network nsg rule create](https://docs.azure.cn/cli/network/nsg/rule#az_network_nsg_rule_create):

* Named **myNSGRuleHTTP**.
* In the network security group you created in the previous step, **myNSG**.
* In resource group **CreateIntLBQS-rg**.
* Protocol **(*)**.
* Direction **Inbound**.
* Source **(*)**.
* Destination **(*)**.
* Destination port **Port 80**.
* Access **Allow**.
* Priority **200**.

```azurecli
  az network nsg rule create \
    --resource-group CreateIntLBQS-rg \
    --nsg-name myNSG \
    --name myNSGRuleHTTP \
    --protocol '*' \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 200
```

## Create backend servers

In this section, you create:

* Network interfaces for the backend servers.
* A cloud configuration file named **cloud-init.txt** for the server configuration.
* Two virtual machines to be used as backend servers for the load balancer.

### Create network interfaces for the virtual machines

Create two network interfaces with [az network nic create](https://docs.azure.cn/cli/network/nic#az_network_nic_create):

#### VM1

* Named **myNicVM1**.
* In resource group **CreateIntLBQS-rg**.
* In virtual network **myVNet**.
* In subnet **myBackendSubnet**.
* In network security group **myNSG**.

```azurecli
  az network nic create \
    --resource-group CreateIntLBQS-rg \
    --name myNicVM1 \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
#### VM2

* Named **myNicVM2**.
* In resource group **CreateIntLBQS-rg**.
* In virtual network **myVNet**.
* In subnet **myBackendSubnet**.
* In network security group **myNSG**.

```azurecli
  az network nic create \
    --resource-group CreateIntLBQS-rg \
    --name myNicVM2 \
    --vnet-name myVnet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```

### Create cloud-init configuration file

Use a cloud-init configuration file to install NGINX and run a 'Hello World' Node.js app on a Linux virtual machine. 

In your current shell, create a file named cloud-init.txt. Copy and paste the following configuration into the shell. Ensure that you copy the whole cloud-init file correctly, especially the first line:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```
### Create virtual machines

Create the virtual machines with [az vm create](https://docs.azure.cn/cli/vm#az_vm_create):

#### VM1
* Named **myVM1**.
* In resource group **CreateIntLBQS-rg**.
* Attached to network interface **myNicVM1**.
* Virtual machine image **UbuntuLTS**.
* Configuration file **cloud-init.txt** you created in step above.
* In **Zone 1**.

```azurecli
  az vm create \
    --resource-group CreateIntLBQS-rg \
    --name myVM1 \
    --nics myNicVM1 \
    --image UbuntuLTS \
    --admin-user azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --zone 1 \
    --no-wait
    
```
#### VM2
* Named **myVM2**.
* In resource group **CreateIntLBQS-rg**.
* Attached to network interface **myNicVM2**.
* Virtual machine image **UbuntuLTS**.
* Configuration file **cloud-init.txt** you created in step above.
* In **Zone 2**.

```azurecli
  az vm create \
    --resource-group CreateIntLBQS-rg \
    --name myVM2 \
    --nics myNicVM2 \
    --image UbuntuLTS \
    --admin-user azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --zone 2 \
    --no-wait
```

It may take a few minutes for the VMs to deploy.

## Create standard load balancer

This section details how you can create and configure the following components of the load balancer:

  * A frontend IP pool that receives the incoming network traffic on the load balancer.
  * A backend IP pool where the frontend pool sends the load balanced network traffic.
  * A health probe that determines health of the backend VM instances.
  * A load balancer rule that defines how traffic is distributed to the VMs.

### Create the load balancer resource

Create a public load balancer with [az network lb create](https://docs.azure.cn/cli/network/lb#az_network_lb_create):

* Named **myLoadBalancer**.
* A frontend pool named **myFrontEnd**.
* A backend pool named **myBackEndPool**.
* Associated with the virtual network **myVNet**.
* Associated with the backend subnet **myBackendSubnet**.

```azurecli
  az network lb create \
    --resource-group CreateIntLBQS-rg \
    --name myLoadBalancer \
    --sku Standard \
    --vnet-name myVnet \
    --subnet myBackendSubnet \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool       
```

### Create the health probe

A health probe checks all virtual machine instances to ensure they can send network traffic. 

A virtual machine with a failed probe check is removed from the load balancer. The virtual machine is added back into the load balancer when the failure is resolved.

Create a health probe with [az network lb probe create](https://docs.azure.cn/cli/network/lb/probe#az_network_lb_probe_create):

* Monitors the health of the virtual machines.
* Named **myHealthProbe**.
* Protocol **TCP**.
* Monitoring **Port 80**.

```azurecli
  az network lb probe create \
    --resource-group CreateIntLBQS-rg \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80   
```

### Create the load balancer rule

A load balancer rule defines:

* Frontend IP configuration for the incoming traffic.
* The backend IP pool to receive the traffic.
* The required source and destination port. 

Create a load balancer rule with [az network lb rule create](https://docs.azure.cn/cli/network/lb/rule#az_network_lb_rule_create):

* Named **myHTTPRule**
* Listening on **Port 80** in the frontend pool **myFrontEnd**.
* Sending load-balanced network traffic to the backend address pool **myBackEndPool** using **Port 80**. 
* Using health probe **myHealthProbe**.
* Protocol **TCP**.
* Idle timeout of **15 minutes**.
* Enable TCP reset.

```azurecli
  az network lb rule create \
    --resource-group CreateIntLBQS-rg \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe \
    --disable-outbound-snat true \
    --idle-timeout 15 \
    --enable-tcp-reset true
```
>[!NOTE]
>The virtual machines in the backend pool will not have outbound internet connectivity with this configuration. <br /> For more information on providing outbound connectivity, see: <br /> **[Outbound connections in Azure](load-balancer-outbound-connections.md)**<br /> Options for providing connectivity: <br /> **[Outbound-only load balancer configuration](egress-only.md)** <br /> **[What is Virtual Network NAT?](../virtual-network/nat-overview.md)**

### Add virtual machines to load balancer backend pool

Add the virtual machines to the backend pool with [az network nic ip-config address-pool add](https://docs.azure.cn/cli/network/nic/ip-config/address-pool#az_network_nic_ip_config_address_pool_add):


#### VM1
* In backend address pool **myBackEndPool**.
* In resource group **CreateIntLBQS-rg**.
* Associated with network interface **myNicVM1** and **ipconfig1**.
* Associated with load balancer **myLoadBalancer**.

```azurecli
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM1 \
   --resource-group CreateIntLBQS-rg \
   --lb-name myLoadBalancer
```

#### VM2
* In backend address pool **myBackEndPool**.
* In resource group **CreateIntLBQS-rg**.
* Associated with network interface **myNicVM2** and **ipconfig1**.
* Associated with load balancer **myLoadBalancer**.

```azurecli
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM2 \
   --resource-group CreateIntLBQS-rg \
   --lb-name myLoadBalancer
```

# [**Basic SKU**](#tab/option-1-create-load-balancer-basic)

>[!NOTE]
>Standard SKU load balancer is recommended for production workloads. For more information about SKUS, see **[Azure Load Balancer SKUs](skus.md)**.

## Configure virtual network

Before you deploy VMs and deploy your load balancer, create the supporting virtual network resources.

### Create a virtual network

Create a virtual network using [az network vnet create](https://docs.azure.cn/cli/network/vnet#az_network_vnet_createt):

* Named **myVNet**.
* Address prefix of **10.1.0.0/16**.
* Subnet named **myBackendSubnet**.
* Subnet prefix of **10.1.0.0/24**.
* In the **CreateIntLBQS-rg** resource group.
* Location of **chinaeast**.

```azurecli
  az network vnet create \
    --resource-group CreateIntLBQS-rg \
    --location chinaeast \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name myBackendSubnet \
    --subnet-prefixes 10.1.0.0/24
```
### Create a network security group

For a standard load balancer, the VMs in the backend address for are required to have network interfaces that belong to a network security group. 

Create a network security group using [az network nsg create](https://docs.azure.cn/cli/network/nsg#az_network_nsg_create):

* Named **myNSG**.
* In resource group **CreateIntLBQS-rg**.

```azurecli
  az network nsg create \
    --resource-group CreateIntLBQS-rg \
    --name myNSG
```

### Create a network security group rule

Create a network security group rule using [az network nsg rule create](https://docs.azure.cn/cli/network/nsg/rule#az_network_nsg_rule_create):

* Named **myNSGRuleHTTP**.
* In the network security group you created in the previous step, **myNSG**.
* In resource group **CreateIntLBQS-rg**.
* Protocol **(*)**.
* Direction **Inbound**.
* Source **(*)**.
* Destination **(*)**.
* Destination port **Port 80**.
* Access **Allow**.
* Priority **200**.

```azurecli
  az network nsg rule create \
    --resource-group CreateIntLBQS-rg \
    --nsg-name myNSG \
    --name myNSGRuleHTTP \
    --protocol '*' \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 200
```

### Create network interfaces for the virtual machines

Create two network interfaces with [az network nic create](https://docs.azure.cn/cli/network/nic#az_network_nic_create):

#### VM1

* Named **myNicVM1**.
* In resource group **CreateIntLBQS-rg**.
* In virtual network **myVNet**.
* In subnet **myBackendSubnet**.
* In network security group **myNSG**.

```azurecli

  az network nic create \
    --resource-group CreateIntLBQS-rg \
    --name myNicVM1 \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
#### VM2

* Named **myNicVM2**.
* In resource group **CreateIntLBQS-rg**.
* In virtual network **myVNet**.
* In subnet **myBackendSubnet**.

```azurecli
  az network nic create \
    --resource-group CreateIntLBQS-rg \
    --name myNicVM2 \
    --vnet-name myVnet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```

## Create backend servers

In this section, you create:

* A cloud configuration file named **cloud-init.txt** for the server configuration. 
* Availability set for the virtual machines
* Two virtual machines to be used as backend servers for the load balancer.

To verify that the load balancer was successfully created, you install NGINX on the virtual machines.

### Create cloud-init configuration file

Use a cloud-init configuration file to install NGINX and run a 'Hello World' Node.js app on a Linux virtual machine. 

In your current shell, create a file named cloud-init.txt. Copy and paste the following configuration into the shell. Ensure that you copy the whole cloud-init file correctly, especially the first line:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### Create availability set for virtual machines

Create the availability set with [az vm availability-set create](https://docs.azure.cn/cli/vm/availability-set#az_vm_availability_set_create):

* Named **myAvSet**.
* In resource group **CreateIntLBQS-rg**.
* Location **chinaeast**.

```azurecli
  az vm availability-set create \
    --name myAvSet \
    --resource-group CreateIntLBQS-rg \
    --location chinaeast 
    
```

### Create virtual machines

Create the virtual machines with [az vm create](https://docs.azure.cn/cli/vm#az_vm_create):

#### VM1
* Named **myVM1**.
* In resource group **CreateIntLBQS-rg**.
* Attached to network interface **myNicVM1**.
* Virtual machine image **UbuntuLTS**.
* Configuration file **cloud-init.txt** you created in step above.
* In availability set **myAvSet**.

```azurecli
  az vm create \
    --resource-group CreateIntLBQS-rg \
    --name myVM1 \
    --nics myNicVM1 \
    --image UbuntuLTS \
    --admin-user azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --availability-set myAvSet \
    --no-wait
    
```
#### VM2
* Named **myVM2**.
* In resource group **CreateIntLBQS-rg**.
* Attached to network interface **myNicVM2**.
* Virtual machine image **UbuntuLTS**.
* Configuration file **cloud-init.txt** you created in step above.
* In **Zone 2**.

```azurecli
  az vm create \
    --resource-group CreateIntLBQS-rg \
    --name myVM2 \
    --nics myNicVM2 \
    --image UbuntuLTS \
    --admin-user azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --availability-set myAvSet  \
    --no-wait
```

It may take a few minutes for the VMs to deploy.

## Create basic load balancer

This section details how you can create and configure the following components of the load balancer:

  * A frontend IP pool that receives the incoming network traffic on the load balancer.
  * A backend IP pool where the frontend pool sends the load balanced network traffic.
  * A health probe that determines health of the backend VM instances.
  * A load balancer rule that defines how traffic is distributed to the VMs.

### Create the load balancer resource

Create a public load balancer with [az network lb create](https://docs.azure.cn/cli/network/lb#az_network_lb_create):

* Named **myLoadBalancer**.
* A frontend pool named **myFrontEnd**.
* A backend pool named **myBackEndPool**.
* Associated with the virtual network **myVNet**.
* Associated with the backend subnet **myBackendSubnet**.

```azurecli
  az network lb create \
    --resource-group CreateIntLBQS-rg \
    --name myLoadBalancer \
    --sku Basic \
    --vnet-name myVNet \
    --subnet myBackendSubnet \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool       
```

### Create the health probe

A health probe checks all virtual machine instances to ensure they can send network traffic. 

A virtual machine with a failed probe check is removed from the load balancer. The virtual machine is added back into the load balancer when the failure is resolved.

Create a health probe with [az network lb probe create](https://docs.azure.cn/cli/network/lb/probe#az_network_lb_probe_create):

* Monitors the health of the virtual machines.
* Named **myHealthProbe**.
* Protocol **TCP**.
* Monitoring **Port 80**.

```azurecli
  az network lb probe create \
    --resource-group CreateIntLBQS-rg \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80   
```

### Create the load balancer rule

A load balancer rule defines:

* Frontend IP configuration for the incoming traffic.
* The backend IP pool to receive the traffic.
* The required source and destination port. 

Create a load balancer rule with [az network lb rule create](https://docs.azure.cn/cli/network/lb/rule#az_network_lb_rule_create):

* Named **myHTTPRule**
* Listening on **Port 80** in the frontend pool **myFrontEnd**.
* Sending load-balanced network traffic to the backend address pool **myBackEndPool** using **Port 80**. 
* Using health probe **myHealthProbe**.
* Protocol **TCP**.
* Idle timeout of **15 minutes**.

```azurecli
  az network lb rule create \
    --resource-group CreateIntLBQS-rg \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe \
    --idle-timeout 15 
```
### Add virtual machines to load balancer backend pool

Add the virtual machines to the backend pool with [az network nic ip-config address-pool add](https://docs.azure.cn/cli/network/nic/ip-config/address-pool#az_network_nic_ip_config_address_pool_add):


#### VM1
* In backend address pool **myBackEndPool**.
* In resource group **CreateIntLBQS-rg**.
* Associated with network interface **myNicVM1** and **ipconfig1**.
* Associated with load balancer **myLoadBalancer**.

```azurecli
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM1 \
   --resource-group CreateIntLBQS-rg \
   --lb-name myLoadBalancer
```

#### VM2
* In backend address pool **myBackEndPool**.
* In resource group **CreateIntLBQS-rg**.
* Associated with network interface **myNicVM2** and **ipconfig1**.
* Associated with load balancer **myLoadBalancer**.

```azurecli
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM2 \
   --resource-group CreateIntLBQS-rg \
   --lb-name myLoadBalancer
```

---

## Test the load balancer

### Create Azure Bastion public IP

Use [az network public-ip create](https://docs.azure.cn/cli/network/public-ip#az_network_public_ip_create) to create a public ip address for the bastion host:

* Create a standard zone redundant public IP address named **myBastionIP**.
* In **CreateIntLBQS-rg**.

```azurecli
  az network public-ip create \
    --resource-group CreateIntLBQS-rg \
    --name myBastionIP \
    --sku Standard
```

### Create Azure Bastion subnet

Use [az network vnet subnet create](https://docs.azure.cn/cli/network/vnet/subnet#az_network_vnet_subnet_create) to create a subnet:

* Named **AzureBastionSubnet**.
* Address prefix of **10.1.1.0/24**.
* In virtual network **myVNet**.
* In resource group **CreateIntLBQS-rg**.

```azurecli
  az network vnet subnet create \
    --resource-group CreateIntLBQS-rg \
    --name AzureBastionSubnet \
    --vnet-name myVNet \
    --address-prefixes 10.1.1.0/24
```

### Create Azure Bastion host
Use [az network bastion create](https://docs.microsoft.com/cli/azure/network/bastion#az_network_bastion_create) to create a bastion host:

* Named **myBastionHost**
* In **CreateIntLBQS-rg**
* Associated with public IP **myBastionIP**.
* Associated with virtual network **myVNet**.
* In **chinaeast** location.

```azurecli
  az network bastion create \
    --resource-group CreateIntLBQS-rg \
    --name myBastionHost \
    --public-ip-address myBastionIP \
    --vnet-name myVNet \
    --location chinaeast
```
It will take a few minutes for the bastion host to deploy.

### Create test virtual machine

Create the network interface with [az network nic create](https://docs.azure.cn/cli/network/nic#az_network_nic_create):

* Named **myNicTestVM**.
* In resource group **CreateIntLBQS-rg**.
* In virtual network **myVNet**.
* In subnet **myBackendSubnet**.
* In network security group **myNSG**.

```azurecli
  az network nic create \
    --resource-group CreateIntLBQS-rg \
    --name myNicTestVM \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
Create the virtual machine with [az vm create](https://docs.azure.cn/cli/vm#az_vm_create):

* Named **myTestVM**.
* In resource group **CreateIntLBQS-rg**.
* Attached to network interface **myNicTestVM**.
* Virtual machine image **Win2019Datacenter**.
* Choose values for **\<adminpass>** and **\<adminuser>**.
  

```azurecli
  az vm create \
    --resource-group CreateIntLBQS-rg \
    --name myTestVM \
    --nics myNicTestVM \
    --image Win2019Datacenter \
    --admin-username <adminuser> \
    --admin-password <adminpass> \
    --no-wait
```
Can take a few minutes for the virtual machine to deploy.

### Test

1. [Sign in](https://portal.azure.cn) to the Azure portal.

1. Find the private IP address for the load balancer on the **Overview** screen. Select **All services** in the left-hand menu, select **All resources**, and then select **myLoadBalancer**.

2. Make note or copy the address next to **Private IP Address** in the **Overview** of **myLoadBalancer**.

3. Select **All services** in the left-hand menu, select **All resources**, and then from the resources list, select **myTestVM** that is located in the **CreateIntLBQS-rg** resource group.

4. On the **Overview** page, select **Connect**, then **Bastion**.

6. Enter the username and password entered during VM creation.

7. Open **Internet Explorer** on **myTestVM**.

8. Enter the IP address from the previous step into the address bar of the browser. The default page of IIS Web server is displayed on the browser.

    :::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/load-balancer-test.png" alt-text="Create a standard internal load balancer" border="true":::
   
To see the load balancer distribute traffic across all three VMs, you can customize the default page of each VM's IIS Web server and then force-refresh your web browser from the client machine.

## Clean up resources

When no longer needed, use the [az group delete](https://docs.azure.cn/cli/group#az_group_delete) command to remove the resource group, load balancer, and all related resources.

```azurecli
  az group delete \
    --name CreateIntLBQS-rg
```

## Next steps
In this quickstart

* You created a standard or public load balancer
* Attached virtual machines. 
* Configured the load balancer traffic rule and health probe.
* Tested the load balancer.

To learn more about Azure Load Balancer, continue to 
> [!div class="nextstepaction"]
> [What is Azure Load Balancer?](load-balancer-overview.md)


<!-- Update_Description: new article about quickstart load balancer standard internal cli -->
<!--NEW.date: 12/21/2020-->