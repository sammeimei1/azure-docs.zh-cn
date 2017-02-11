---
title: "使用 PowerShell cmdlet 创建虚拟机规模集 | Microsoft Docs"
description: "开始使用 Azure PowerShell cmdlet 来创建和管理你的第一个 Azure 虚拟机缩放集"
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 430d9d64-1f35-48f0-a4fd-9b69910ffa59
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: danielsollondon
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 14f83c6753ce37639b1b2f78a4c632f1d69f585d


---
# <a name="creating-virtual-machine-scale-sets-using-powershell-cmdlets"></a>使用 PowerShell cmdlet 创建虚拟机缩放集
本示例演示如何创建虚拟机缩放集 (VMSS)，其中将会创建具有 3 个节点的 VMSS，并包含所有关联的网络和存储。

## <a name="first-steps"></a>前几个步骤
确保已安装最新的 Azure PowerShell 模块，其中包含维护和创建 VMSS 所需的 PowerShell cmdlet。
转到[此处](http://aka.ms/webpi-azps)的命令行工具，获取最新的可用 Azure 模块。

若要查找 VMSS 相关的 cmdlet，请使用搜索字符串 \*VMSS\*。

## <a name="creating-a-vmss"></a>创建 VMSS
##### <a name="create-resource-group"></a>创建资源组
```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

##### <a name="create-storage-account"></a>创建存储帐户
设置存储帐户类型/名称。

```
$stoname = 'sto' + $rgname;
$stotype = 'Standard_LRS';
 New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $stoname -Location $loc -SkuName $stotype -Kind "Storage";

$stoaccount = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $stoname;
```

#### <a name="create-networking-vnet--subnet"></a>创建网络（VNET/子网）
##### <a name="subnet-specification"></a>子网规范
```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

##### <a name="vnet-specification"></a>VNET 规范
```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

#In this case below we assume the new subnet is the only one, note difference if you have one already or have adjusted this code to more than one subnet.
$subnetId = $vnet.Subnets[0].Id;
```

##### <a name="create-public-ip-resource-to-allow-external-access"></a>创建允许外部访问的公共 IP 资源
这将绑定到负载平衡器。

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

##### <a name="create-and-configure-load-balancer"></a>创建并配置负载平衡器
```
$frontendName = 'fe' + $rgname
$backendAddressPoolName = 'bepool' + $rgname
$probeName = 'vmssprobe' + $rgname
$inboundNatPoolName = 'innatpool' + $rgname
$lbruleName = 'lbrule' + $rgname
$lbName = 'vmsslb' + $rgname

#Bind Public IP to Load Balancer
$frontend = New-AzureRmLoadBalancerFrontendIpConfig -Name $frontendName -PublicIpAddress $pubip
```

##### <a name="configure-load-balancer"></a>配置负载平衡器
创建后端地址池配置，这将由 VMSS 中 VM 的 NIC 共享。

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

设置负载平衡的探测端口，并根据应用程序适当地更改设置。

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

创建 NAT 规则以通过负载平衡器直接路由（而不平衡负载）连接到 VMSS 中的 VM，请注意这只是演示 RDP 的用法，实际上应使用内部 VNET 方法通过 RDP 访问这些服务器。

```
$frontendpoolrangestart = 3360
$frontendpoolrangeend = 3370
$backendvmport = 3389
$inboundNatPool = New-AzureRmLoadBalancerInboundNatPoolConfig -Name $inboundNatPoolName -FrontendIPConfigurationId `
$frontend.Id -Protocol Tcp -FrontendPortRangeStart $frontendpoolrangestart -FrontendPortRangeEnd $frontendpoolrangeend -BackendPort $backendvmport;
```

创建负载平衡规则。本示例演示如何使用上述步骤中的设置来平衡端口 80 请求的负载。

```
$protocol = 'Tcp'
$feLBPort = 80
$beLBPort = 80

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name $lbruleName `
-FrontendIPConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -Protocol $protocol -FrontendPort $feLBPort -BackendPort $beLBPort `
-IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -Verbose;
```

使用配置创建负载平衡器。

```
$actualLb = New-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname -Location $loc `
-FrontendIpConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -LoadBalancingRule $lbrule -InboundNatPool $inboundNatPool -Verbose;
```

检查 LB 设置，检查负载均衡端口配置。请注意，在 VMSS 中创建 VM 之前，看不到入站 NAT 规则。

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### <a name="configure-and-create-vmss"></a>配置和创建 VMSS
请注意，此基础结构示例演示如何在 VMSS 中设置分发和缩放网络流量，但此处指定的 VM 映像并未安装任何 Web 服务。

```
#specify VMSS Name
$vmssName = 'vmss' + $rgname;

##specify VMSS specific details
$adminUsername = 'azadmin';
$adminPassword = "Password1234!";

$PublisherName = 'MicrosoftWindowsServer'
$Offer         = 'WindowsServer'
$Sku          = '2012-R2-Datacenter'
$Version       = 'latest'
$vmNamePrefix = 'winvmss'

$vhdContainer = "https://" + $stoname + ".blob.core.windows.net/" + $vmssName;

###add an extension
$extname = 'BGInfo';
$publisher = 'Microsoft.Compute';
$exttype = 'BGInfo';
$extver = '2.1';
```

将 NIC 绑定到负载平衡器和子网

```
$ipCfg = New-AzureRmVmssIPConfig -Name 'nic' `
-LoadBalancerInboundNatPoolsId $actualLb.InboundNatPools[0].Id `
-LoadBalancerBackendAddressPoolsId $actualLb.BackendAddressPools[0].Id `
-SubnetId $subnetId;
```

创建 VMSS 配置

```
#Specify number of nodes
$numberofnodes = 3

$vmss = New-AzureRmVmssConfig -Location $loc -SkuCapacity $numberofnodes -SkuName 'Standard_A2' -UpgradePolicyMode 'automatic' `
    | Add-AzureRmVmssNetworkInterfaceConfiguration -Name $subnetName -Primary $true -IPConfiguration $ipCfg `
    | Set-AzureRmVmssOSProfile -ComputerNamePrefix $vmNamePrefix -AdminUsername $adminUsername -AdminPassword $adminPassword `
    | Set-AzureRmVmssStorageProfile -Name 'test' -OsDiskCreateOption 'FromImage' -OsDiskCaching 'None' `
    -ImageReferenceOffer $Offer -ImageReferenceSku $Sku -ImageReferenceVersion $Version `
    -ImageReferencePublisher $PublisherName -VhdContainer $vhdContainer `
    | Add-AzureRmVmssExtension -Name $extname -Publisher $publisher -Type $exttype -TypeHandlerVersion $extver -AutoUpgradeMinorVersion $true
```

生成 VMSS 配置

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

现在你已创建 VMSS。 在本示例中，可以测试使用 RDP 连接到单个 VM：

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```



<!--HONumber=Nov16_HO3-->

