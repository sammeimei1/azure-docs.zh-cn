---
title: "创建 Azure 面向 Internet 的负载均衡器 - PowerShell | Microsoft 文档"
description: "了解如何使用 PowerShell 在 Resource Manager 中创建面向 Internet 的负载均衡器"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
ms.assetid: 8257f548-7019-417f-b15f-d004a1eec826
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 0347e90fa97a083865e8b783c96034d30a859031
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2017
---
# <a name="get-started"></a>在资源管理器中，透过使用 PowerShell 来创建网络面向的负载均衡器

> [!div class="op_single_selector"]
> * [门户](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [模板](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

本文介绍 Resource Manager 部署模型。 还可以[了解如何使用经典部署模型创建面向 Internet 的负载均衡器](load-balancer-get-started-internet-classic-cli.md)。

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>使用 Azure PowerShell 部署解决方案

以下过程说明如何使用 Azure Resource Manager 和 PowerShell 创建面向 Internet 的负载均衡器。 借助 Azure Resource Manager，可单独创建和配置每个资源，然后将其组合在一起来创建负载均衡器。

需要创建和配置以下对象才能部署负载均衡器：

* 前端 IP 配置：包含传入网络流量的公共 IP (PIP) 地址。
* 后端地址池：包含要从负载均衡器接收网络流量的虚拟机网络接口 (NIC)。
* 负载均衡规则：所含规则可将负载均衡器上的公共端口映射到后端地址池的端口上。
* 入站 NAT 规则：所含规则可将负载均衡器上的公共端口映射到后端地址池中特定虚拟机的端口。
* 探测器：包含用于检查后端地址池中虚拟机实例的可用性的运行状况探测器。

有关详细信息，请参阅 [Azure Resource Manager 对负载均衡器的支持](load-balancer-arm.md)。

## <a name="set-up-powershell-to-use-resource-manager"></a>将 PowerShell 设置为使用 Resource Manager

确保具备 Azure Resource Manager 模块的最新生产版本才可用于 PowerShell：

1. 登录 Azure。

    ```powershell
    Login-AzureRmAccount
    ```

    在系统提示时输入凭据。

2. 检查该帐户的订阅。

    ```powershell
    Get-AzureRmSubscription
    ```

3. 选择要使用的 Azure 订阅。

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. 创建资源组。 （若要使用现有资源组，请跳过此步骤。）

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>为前端 IP 池创建虚拟网络和公共 IP 地址

1. 创建子网和虚拟网络。

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. 使用 DNS 名称 **loadbalancernrp.westus.cloudapp.azure.com** 创建要由前端 IP 池使用的名为 **PublicIP** 的 Azure 公用 IP 地址资源。以下命令使用静态分配类型。

    ```powershell
    $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -DomainNameLabel loadbalancernrp
    ```

   > [!IMPORTANT]
   > 负载均衡器将公共 IP 的域标签用作 FQDN 的前缀。 这不同于经典部署模型，后者将云服务用作负载均衡器 FQDN。
   > 在本示例中，FQDN 是 **loadbalancernrp.westus.cloudapp.azure.com**。

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>创建前端 IP 池和后端地址池

1. 创建使用 **PublicIp** 资源且名为 **LB-Frontend** 的前端 IP 池。

    ```powershell
    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP
    ```

2. 创建名为 **LB-backend** 的后端地址池。

    ```powershell
    $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend
    ```

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>创建 NAT 规则、负载均衡器规则、探测器和负载均衡器

下例会创建以下项：

* 用于将端口 3441 上的所有传入流量转换到端口 3389 的 NAT 规则
* 用于将端口 3442 上的所有传入流量转换到端口 3389 的 NAT 规则
* 用于检查名为 **HealthProbe.aspx** 的页面上运行状况状态的探测规则
* 用于将端口 80 上的所有传入流量平衡到后端池中的地址端口 80 的负载均衡器规则
* 使用上述所有对象的负载均衡器

执行以下步骤：

1. 创建 NAT 规则。

    ```powershell
    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389
    ```

2. 创建运行状况探测器。 有两种方法可以配置探测器：

    HTTP 探测器

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    TCP 探测器

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

3. 创建负载均衡器规则。

    ```powershell
    $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

4. 使用之前创建的对象创建负载均衡器。

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

## <a name="create-nics"></a>创建 NIC

创建网络接口（或修改现有接口），并将其关联到 NAT 规则、负载均衡器规则和探测器：

1. 获取需创建 NIC 的虚拟网络和虚拟网络子网。

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. 创建名为 **lb-nic1-be** 的 NIC，并将其与第一个 NAT 规则和第一个（且仅有的）后端地址池相关联。

    ```powershell
    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
    ```

3. 创建名为 **lb-nic2-be** 的 NIC，并将其与第二个 NAT 规则和第一个（且仅有的）后端地址池相关联。

    ```powershell
    $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
    ```

4. 检查 NIC。

        $backendnic1

    预期输出：

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. 使用 `Add-AzureRmVMNetworkInterface` cmdlet 将 NIC 分配给不同 VM。

## <a name="create-a-virtual-machine"></a>创建虚拟机

有关创建虚拟机和分配 NIC 的指南，请参阅[使用 PowerShell 创建 Azure VM](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)。

## <a name="add-the-network-interface-to-the-load-balancer"></a>向负载均衡器添加网络接口

1. 检索 Azure 中的负载均衡器。

    将负载均衡器资源加载到变量中（如果还没有这样做）。 该变量称为 **$lb**。使用与先前创建的负载均衡器资源相同的名称。

    ```powershell
    $lb= get-azurermloadbalancer -name NRP-LB -resourcegroupname NRP-RG
    ```

2. 将后端配置加载到变量。

    ```powershell
    $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name LB-backend -LoadBalancer $lb
    ```

3. 将创建好的网络接口加载到变量中。 变量名为 **$nic**。 网络接口名称与上例中的相同。

    ```powershell
    $nic =get-azurermnetworkinterface -name lb-nic1-be -resourcegroupname NRP-RG
    ```

4. 更改网络接口上的后端配置。

    ```powershell
    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
    ```

5. 保存网络接口对象。

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```

    将网络接口添加到负载均衡器后端池后，它会根据该负载均衡器资源的负载均衡规则开始接收网络流量。

## <a name="update-an-existing-load-balancer"></a>更新现有的负载均衡器

1. 使用上例中的负载均衡器，通过 `Get-AzureLoadBalancer` 将负载均衡器对象分配给变量 **$slb**。

    ```powershell
    $slb = get-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
    ```

2. 在下例中，使用前端池中的端口 81 和后端池中的端口 8181 将入站 NAT 规则添加到现有负载均衡器中。

    ```powershell
    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP
    ```

3. 通过 `Set-AzureLoadBalancer` 保存新配置。

    ```powershell
    $slb | Set-AzureRmLoadBalancer
    ```

## <a name="remove-a-load-balancer"></a>删除负载均衡器

使用命令 `Remove-AzureLoadBalancer` 删除之前在 **NRP-RG** 资源组中创建的名为 **NRP-LB** 的负载均衡器。

```powershell
Remove-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
```

> [!NOTE]
> 可以使用可选开关 **-Force**，避免显示删除提示。

## <a name="next-steps"></a>后续步骤

[开始配置内部负载均衡器](load-balancer-get-started-ilb-arm-ps.md)

[配置负载均衡器分发模式](load-balancer-distribution-mode.md)

[配置负载均衡器的空闲 TCP 超时设置](load-balancer-tcp-idle-timeout.md)
