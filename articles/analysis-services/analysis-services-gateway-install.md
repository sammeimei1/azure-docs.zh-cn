---
title: "安装本地数据网关 | Microsoft 文档"
description: "了解如何安装并配置本地数据网关。"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/20/2017
ms.author: owend
ms.translationtype: HT
ms.sourcegitcommit: a29f1e7b39b7f35073aa5aa6c6bd964ffaa6ffd0
ms.openlocfilehash: d614352a62dc7aca012e9b144473604ae9829af9
ms.contentlocale: zh-cn
ms.lasthandoff: 09/21/2017

---
# <a name="install-and-configure-an-on-premises-data-gateway"></a>安装并配置本地数据网关
同一区域中的一个或多个 Azure Analysis Services 服务器连接到本地数据源时，需要具备本地数据网关。 要了解有关网关的更多信息，请参阅[本地数据网关](analysis-services-gateway.md)。

## <a name="prerequisites"></a>先决条件
**最低要求：**

* .NET 4.5 Framework
* 64 位版本的 Windows 7/Windows Server 2008 R2（或更高版本）

**推荐：**

* 8 核 CPU
* 8 GB 内存
* 64 位版本的 Windows 2012 R2（或更高版本）

**重要注意事项：**

* 安装过程中，向 Azure 注册你的网关时，会选中你订阅的默认区域。 你也可以选择其他区域。 如果你在多个区域有服务器，则必须为每个区域都安装一个网关。 
* 不能在域控制器上安装网关。
* 一台计算机上只能安装一个网关。
* 在计算机处于开启但未处于休眠状态下安装网关。
* 不要在使用无线网络连接的计算机上安装网关。 否则，可能会降低性能。


## <a name="download"></a>下载
 [下载网关](https://aka.ms/azureasgateway)

## <a name="install"></a>安装

1. 运行安装程序。

2. 选择一个位置，接受条款，然后单击“安装”。

   ![安装位置和许可条款](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. 输入用于登录 Azure 的帐户。 该帐户必须位于你租户的 Azure Active Directory 中。 该帐户用于网关管理员。 

   ![输入用于登录 Azure 的帐户](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > 如果你使用域帐户登录，它将映射到你在 Azure AD 中的组织帐户。 你的组织帐户将用作网关管理员。

## <a name="register"></a>注册
若要在 Azure 中创建网关资源，则必须在网关云服务中注册你安装的本地实例。 

1.  选择“在此计算机上注册新网关”。

    ![注册](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. 为你的网关键入名称和恢复密钥。 默认情况下，网关使用你订阅的默认区域。 如果你需要选择其他区域，请选择“更改区域”。

   ![注册](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <a name="create-resource"></a>创建 Azure 网关资源
在安装并注册了网关后，需要在 Azure 订阅中创建网关资源。 使用注册网关时使用的帐户登录到 Azure。

1. 在 Azure 门户中，单击“创建新服务” > “企业集成” > “本地数据网关” > “创建”。

   ![创建网关资源](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. 在“创建连接网关”中，输入以下设置：

    * 名称：输入网关资源的名称。 

    * 订阅：选择要与网关资源关联的 Azure 订阅。 
    此订阅应与服务器所在的订阅相同。
   
      默认订阅取决于用来登录的 Azure 帐户。

    * **资源组**：创建资源组或选择现有资源组。

    * 位置： 选择网关的注册区域。

    * 安装名称：如果尚未选择网关安装，请选择注册的网关。 

    完成后，单击“创建”。

## <a name="connect-servers"></a>将服务器连接到网关资源

1. 在 Azure Analysis Services 服务器概述中，单击“本地数据网关”。

   ![将服务器连接到网关](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. 在“选取要连接的本地数据网关”，选择你的网关资源，然后单击“连接所选网关”。

   ![将服务器连接到网关资源](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > 如果列表中不显示你的网关，很可能是你的服务器与你注册网关时指定的区域不在同一个区域。 

就这么简单。 如果你需要打开端口或执行任何故障排除时，一定要签出[本地数据网关](analysis-services-gateway.md)。

## <a name="next-steps"></a>后续步骤
* [管理 Analysis Services](analysis-services-manage.md)   
* [从 Azure Analysis Services 获取数据](analysis-services-connect.md)

