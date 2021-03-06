---
title: "使用 Azure CLI 1.0 创建 Linux VM 的副本 | Microsoft 文档"
description: "了解如何使用 Azure CLI 1.0 在 Resource Manager 部署模型中创建 Azure Linux 虚拟机的副本"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 62ae54f3596c9383cbf3b401fcfdb42ecfdee63c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure-with-the-azure-cli-10"></a>使用 Azure CLI 1.0 创建在 Azure 上运行的 Linux 虚拟机副本
本文说明如何使用 Resource Manager 部署模型创建运行 Linux 的 Azure 虚拟机 (VM) 副本。 首先，通过操作系统和数据磁盘复制到新容器，并设置网络资源并创建新虚拟机。

还可以[上传自定义磁盘映像并从中创建 VM](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。

## <a name="cli-versions-to-complete-the-task"></a>用于完成任务的 CLI 版本
可以使用以下 CLI 版本之一完成任务：

- Azure CLI 1.0 – 用于经典部署模型和资源管理部署模型（本文）的 CLI
- [Azure CLI 2.0](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - 适用于资源管理部署模型的下一代 CLI

## <a name="before-you-begin"></a>开始之前
在开始执行相关步骤前，请先确保符合以下先决条件：

* 已在计算机上下载并安装 [Azure CLI](../../cli-install-nodejs.md)。 
* 还需要准备好有关现有 Azure Linux VM 的一些信息：

| 源 VM 信息 | 从何处获取 |
| --- | --- |
| VM 名称 |`azure vm list` |
| 资源组名称 |`azure vm list` |
| 位置 |`azure vm list` |
| 存储帐户名称 |`azure storage account list -g <resourceGroup>` |
| 容器名称 |`azure storage container list -a <sourcestorageaccountname>` |
| 源 VM VHD 文件名 |`azure storage blob list --container <containerName>` |

* 需要对新 VM 做出一些选择：   <br> -容器名称    <br> -VM 名称    <br> -VM 大小    <br> -vNet 名称    <br> -子网名称    <br> -IP 名称    <br> -NIC 名称

## <a name="login-and-set-your-subscription"></a>登录并设置订阅
1. 登录到 CLI。

    ```azurecli
    azure login
    ```
2. 请确保处于 Resource Manager 模式。

    ```azurecli
    azure config mode arm
    ```
3. 设置正确的订阅。 可以使用“azure account list”查看所有订阅。

    ```azurecli
    azure account set mySubscriptionID
    ```

## <a name="stop-the-vm"></a>停止 VM
停止源 VM 并将其解除分配。 可以使用“azure vm list”获取订阅中所有 VM 及其资源组名称的列表。

```azurecli
azure vm stop myResourceGroup myVM
azure vm deallocate myResourceGroup MyVM
```


## <a name="copy-the-vhd"></a>复制 VHD
可以使用 `azure storage blob copy start` 将 VHD 从源存储复制到目标。 在本示例中，我们将 VHD 复制到相同的存储帐户但不同的容器中。

要将 VHD 复制到同一存储帐户中的另一个容器，请键入：

```azurecli
azure storage blob copy start \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd \
        myNewContainerName
```

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>为新 VM 设置虚拟网络
为新 VM 设置虚拟网络和 NIC。 

```azurecli
azure network vnet create myResourceGroup myVnet -l myLocation

azure network vnet subnet create -a <address.prefix.in.CIDR/format> myResourceGroup myVnet mySubnet

azure network public-ip create myResourceGroup myPublicIP -l myLocation

azure network nic create myResourceGroup myNic -k mySubnet -m myVnet -p myPublicIP -l myLocation
```


## <a name="create-the-new-vm"></a>创建新 VM
现在[可以使用 Resource Manager 模板](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd)从已上传的虚拟磁盘创建 VM，或者通过在 CLI 中输入以下命令指定所复制磁盘的 URI 来创建 VM：

```azurecli
azure vm create -n myVM -l myLocation -g myResourceGroup -f myNic \
        -z Standard_DS1_v2 -y Linux \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd 
```



## <a name="next-steps"></a>后续步骤
若要了解如何使用 Azure CLI 管理新虚拟机，请参阅 [Azure CLI commands for the Azure Resource Manager](../azure-cli-arm-commands.md)（Azure Resource Manager 的 Azure CLI 命令）。

