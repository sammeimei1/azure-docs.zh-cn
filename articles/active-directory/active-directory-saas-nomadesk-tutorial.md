---
title: "教程： 与 Nomadesk Azure Active Directory 集成 |Microsoft 文档"
description: "了解如何配置单一登录 Azure Active Directory 和 Nomadesk 之间。"
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d261b776-b48e-45f0-9722-0297adefabb8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 1d652d562f4c5caffded18d928e2395e537f59f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-nomadesk"></a>与 Nomadesk 的教程： Azure Active Directory 集成

在本教程中，你将学习如何与 Azure Active Directory (Azure AD) 集成 Nomadesk。

与 Azure AD 集成 Nomadesk 提供了以下好处：

- 你可以控制有权访问 Nomadesk Azure AD 中
- 你可以使用户可以自动获取登录到 Nomadesk （上单一登录） 使用其 Azure AD 帐户
- 你可以管理你的帐户在一个中心位置-Azure 门户

如果你想要了解有关与 Azure AD 的 SaaS 应用程序集成的详细信息，请参阅[应用程序访问与单一登录与 Azure Active Directory 是什么](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>必备条件

若要使用 Nomadesk 配置 Azure AD 集成，你需要以下各项：

- Azure AD 订阅
- Nomadesk 上单一登录启用的订阅

> [!NOTE]
> 若要测试步骤在本教程中，我们不建议使用生产环境。

若要测试本教程中的步骤，你应遵循这些建议：

- 除非必要，否则，不要使用你的生产环境。
- 如果你没有 Azure AD 试用环境，你可以获取一个月试用版[此处](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案说明
在本教程中，你测试 Azure AD 单一登录在测试环境。 在本教程中概述的方案由两个主要构建基块组成：

1. 从库添加 Nomadesk
2. 配置和测试 Azure AD 单一登录

## <a name="adding-nomadesk-from-the-gallery"></a>从库添加 Nomadesk
若要配置 Nomadesk 到 Azure AD 的集成，你需要将 Nomadesk 从库添加到你的托管的 SaaS 应用的列表。

**若要从库添加 Nomadesk，执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)**，在左的导航面板中，单击**Azure Active Directory**图标。 

    ![Active Directory][1]

2. 导航到**企业应用程序**。 然后转到**所有应用程序**。

    ![应用程序][2]
    
3. 若要添加新应用程序，请单击**新的应用程序**对话框顶部的按钮。

    ![应用程序][3]

4. 在搜索框中，键入**Nomadesk**。

    ![创建一个 Azure AD 的测试用户](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_search.png)

5. 在结果面板中，选择**Nomadesk**，然后单击**添加**按钮以添加该应用程序。

    ![创建一个 Azure AD 的测试用户](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，可以配置和测试 Azure AD 单一登录与 Nomadesk 基于一个名为"Britta 人 Simon"的测试用户。

对于单一登录工作，Azure AD 需要知道什么对应项中的用户 Nomadesk 是向用户在 Azure AD 中。 换而言之，Azure AD 用户和 Nomadesk 中的相关的用户之间的链接关系需要建立。

在 Nomadesk，将分配的值**用户名**作为的值的 Azure AD 中**用户名**建立的链接关系。

若要配置和测试 Azure AD 单一登录与 Nomadesk，你需要完成以下构建基块：

1. **[Azure AD 单一登录配置](#configuring-azure-ad-single-sign-on)** -若要使用户可以使用此功能。
2. **[创建一个 Azure AD 的测试用户](#creating-an-azure-ad-test-user)** -若要测试 Azure AD 单一登录与 Britta 人 Simon。
3. **[创建 Nomadesk 测试用户](#creating-a-nomadesk-test-user)** -若要链接到用户的 Azure AD 表示的 Nomadesk 中具有 Britta 人 Simon 的副本。
4. **[Azure AD 测试用户分配](#assigning-the-azure-ad-test-user)** -若要启用 Britta 人 Simon 用于 Azure AD 单一登录。
5. **[单一登录测试](#testing-single-sign-on)** -若要验证是否配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，可以启用 Azure AD 单一登录在 Azure 门户和 Nomadesk 应用程序中配置单一登录。

**若要使用 Nomadesk 配置 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中，在**Nomadesk**应用程序集成页上，单击**单一登录**。

    ![配置单一登录][4]

2. 上**单一登录**对话框中，选择**模式**作为**SAML 基于登录**若要启用单一登录。
 
    ![配置单一登录](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_samlbase.png)

3. 上**Nomadesk 域和 Url**部分中，执行以下步骤：

    ![配置单一登录](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_url.png)

    a. 在**登录 URL**文本框中，键入 URL 使用以下模式：`https://mynomadesk.com/logon/saml/<TENANTID>`

    b。 在**标识符**文本框中，键入 URL 使用以下模式：`https://secure.nomadesk.com/saml/<instancename>`

    > [!NOTE] 
    > 这些值不是实际的。 更新这些值与实际的登录 URL 和标识符。 联系人[Nomadesk 客户端支持团队](mailto:support@nomadesk.com)以获取这些值。 
 
4. 上**SAML 签名证书**部分中，单击**Certificate(Base64)**然后将保存您的计算机上的证书文件。

    ![配置单一登录](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_certificate.png) 

5. 单击**保存**按钮。

    ![配置单一登录](./media/active-directory-saas-nomadesk-tutorial/tutorial_general_400.png)

6. 上**Nomadesk 配置**部分中，单击**配置 Nomadesk**以打开**上配置登录**窗口。 复制**注销 URL、 SAML 实体 ID 和 SAML 单一登录服务 URL**从**快速参考部分。**

    ![配置单一登录](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_configure.png) 

7. 在上配置单一登录**Nomadesk**端，你需要发送下载**证书**，**注销 URL、 SAML 实体 ID 和 SAML 单一登录服务 URL**到[Nomadesk 支持团队](mailto:support@nomadesk.com)。 设置此设置以让 SAML SSO 连接两端正确设置它们。

> [!TIP]
> 你现在可以读取内部这些指令的简洁版本[Azure 门户](https://portal.azure.com)，而你要设置应用程序 ！  在添加此应用程序从后**Active Directory > 企业应用程序**部分中，只需单击**上单一登录**选项卡上和访问通过的嵌入的文档**配置**底部部分中的。 你可以阅读更多有关嵌入的文档的功能： [Azure AD 嵌入文档]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>创建一个 Azure AD 的测试用户
此部分的目的是在调用 Britta 人 Simon Azure 门户中创建一个测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建一个测试用户，请执行以下步骤：**

1. 在**Azure 门户**，左侧的导航窗格中，单击**Azure Active Directory**图标。

    ![创建一个 Azure AD 的测试用户](./media/active-directory-saas-nomadesk-tutorial/create_aaduser_01.png) 

2. 若要显示的用户的列表，请转到**用户和组**单击**所有用户**。
    
    ![创建一个 Azure AD 的测试用户](./media/active-directory-saas-nomadesk-tutorial/create_aaduser_02.png) 

3. 若要打开**用户**对话框中，单击**添加**对话框顶部。
 
    ![创建一个 Azure AD 的测试用户](./media/active-directory-saas-nomadesk-tutorial/create_aaduser_03.png) 

4. 上**用户**对话框页上，执行以下步骤：
 
    ![创建一个 Azure AD 的测试用户](./media/active-directory-saas-nomadesk-tutorial/create_aaduser_04.png) 

    a. 在**名称**文本框中，键入**BrittaSimon**。

    b。 在**用户名**文本框中，键入**电子邮件地址**BrittaSimon。

    c. 选择**显示密码**并记下的值**密码**。

    d. 单击“创建”。
 
### <a name="creating-a-nomadesk-test-user"></a>创建 Nomadesk 测试用户

此部分的目的是创建一个名为 Britta 人 Simon 中 Nomadesk 用户。 Nomadesk 支持在实时预配，这是默认启用。

为你在本部分中的没有操作项。 在尝试访问 Nomadesk，如果它尚不存在的过程创建新用户。

>[!NOTE]
>如果你需要手动创建用户，你需要联系[Nomadesk 支持团队](mailto:support@nomadesk.com)。 

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在此部分中，你将启用 Britta 人 Simon 用于通过到 Nomadesk 授予访问 Azure 单一登录。

![将用户分配][200] 

**若要将 Britta 人 Simon 分配到 Nomadesk，执行以下步骤：**

1. 在 Azure 门户中，打开应用程序视图，然后导航到的目录视图，并转到**企业应用程序**然后单击**所有应用程序**。

    ![将用户分配][201] 

2. 在应用程序列表中，选择**Nomadesk**。

    ![配置单一登录](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_app.png) 

3. 在左侧菜单中，单击**用户和组**。

    ![将用户分配][202] 

4. 单击**添加**按钮。 然后选择**用户和组**上**添加分配**对话框。

    ![将用户分配][203]

5. 上**用户和组**对话框中，选择**Britta 人 Simon**用户列表中。

6. 单击**选择**按钮上**用户和组**对话框。

7. 单击**分配**按钮上**添加分配**对话框。
    
### <a name="testing-single-sign-on"></a>测试单一登录

在此部分中，你将测试 Azure AD 单一登录配置使用访问面板。

当你单击访问面板中的 Nomadesk 磁贴时，你应该获取自动登录到你 Nomadesk 的应用程序。
有关访问面板的详细信息，请参阅[访问面板简介](active-directory-saas-access-panel-introduction.md)。

## <a name="additional-resources"></a>其他资源

* [有关如何与 Azure Active Directory 集成 SaaS 应用程序教程列表](active-directory-saas-tutorial-list.md)
* [应用程序访问与单一登录与 Azure Active Directory 是什么？](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_203.png

