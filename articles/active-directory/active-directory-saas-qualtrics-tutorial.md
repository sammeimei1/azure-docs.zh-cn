---
title: "教程：Azure Active Directory 与 Qualtrics 集成 | Microsoft 文档"
description: "了解如何使用 Qualtrics 与 Azure Active Directory 来启用单一登录、自动化预配和其他功能！"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 2fcde595a40dafda7549f5bccb582b57585b314e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a>教程：Azure Active Directory 与 Qualtrics 集成
本教程的目的是说明 Azure 与 Qualtrics 的集成。  

在本教程中概述的方案假定已有以下各项：

* 一个有效的 Azure 订阅
* 已启用 Qualtrics 单一登录 (SSO) 的订阅

完成本教程后，分配到 Qualtrics 的 Azure AD 用户能够在 Qualtrics 公司站点（服务提供商启动的登录）或使用[访问面板简介](active-directory-saas-access-panel-introduction.md)单一登录到应用程序。

在本教程中概述的方案由以下构建基块组成：

1. 为 Qualtrics 启用应用程序集成
2. 配置单一登录 (SSO)
3. 配置用户设置
4. 分配用户

![方案](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "方案")

## <a name="enabling-the-application-integration-for-qualtrics"></a>为 Qualtrics 启用应用程序集成
本部分的目的是概述如何为 Qualtrics 启用应用程序集成。

**若要为 Qualtrics 启用应用程序集成，请执行以下步骤：**

1. 在 Azure 经典门户的左侧导航窗格中，单击“Active Directory”。
   
   ![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")
2. 在“目录”列表中，选择要启用目录集成的目录。
3. 若要打开应用程序视图，请在目录视图的顶部菜单中，单击“应用程序”。
   
   ![应用程序](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "应用程序")
4. 在页面底部单击“添加”。
   
   ![添加应用程序](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "添加应用程序")
5. 在“要执行什么操作”对话框中，单击“从库中添加应用程序”。
   
   ![从库添加应用程序](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "从库添加应用程序")
6. 在搜索框中，键入“Qualtrics”。
   
   ![应用程序库](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "应用程序库")
7. 在结果窗格中，选择“Qualtrics”，并单击“完成”以添加该应用程序。
   
   ![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")
   
## <a name="configure-single-sign-on"></a>配置单一登录

本部分的目的是概述如何让用户使用基于 SAML 协议的联合身份验证通过他们在 Azure AD 中的帐户向 Qualtrics 进行身份验证。

**若要配置单一登录，请执行以下步骤：**

1. 在 Azure 经典门户的“Qualtrics”应用程序集成页上，单击“配置单一登录”，打开“配置单一登录”对话框。
   
   ![配置单一登录](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "配置单一登录")
2. 在“你希望用户如何登录 Qualtrics”页上，选择“Microsoft Azure AD 单一登录”，并单击“下一步”。
   
   ![配置单一登录](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "配置单一登录")
3. 在“配置应用 URL”页上的“Qualtrics 登录 URL”文本框中，键入 URL（例如：“*https://ssotest2ut1.qualtrics.com*”），并单击“下一步”。
   
   ![配置应用 URL](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "配置应用 URL")
4. 在“配置 Qualtrics 的单一登录”页上，单击“下载元数据”，然后将元数据文件保存在计算机上。
   
   ![配置单一登录](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "配置单一登录")
5. 将元数据文件发送给 Qualtrics 支持团队。
   
   >[!NOTE]
   >SSO 配置必须由 Qualtrics 支持团队执行。 配置完成后会立即收到通知。
   > 
   > 
6. 在 Azure 经典门户中，选择“单一登录配置确认”，并单击“完成”，关闭“配置单一登录”对话框。
   
   ![配置单一登录](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "配置单一登录")
   
## <a name="configure-user-provisioning"></a>配置用户设置

没有操作项可用于配置预配到 Qualtrics 的用户。 当分配的用户尝试使用访问面板登录到 Qualtrics 时，Qualtrics 会检查该用户是否存在。  

如果尚无用户帐户可用，Qualtrics 会自动创建该帐户。

## <a name="assign-users"></a>分配用户
若要测试配置，需要通过分配权限的方式向希望其使用应用程序的 Azure AD 用户授予该配置的访问权限。

**要将用户分配到 Qualtrics，请执行以下步骤：**

1. 在 Azure 经典门户中，创建测试帐户。
2. 在“Qualtrics”应用程序集成页上，单击“分配用户”。
   
   ![分配用户](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "分配用户")
3. 选择测试用户，单击“分配”，并单击“是”确认分配。
   
   ![是](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "是")

如果要测试 SSO 设置，请打开访问面板。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md)（访问面板简介）。

