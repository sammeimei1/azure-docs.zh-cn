---
title: "快速入门：Azure AD SSPR | Microsoft Docs"
description: "快速部署 Azure AD 自助密码重置"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/28/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 8497c6c6d7cfc6c4457073783d20f48a722ea18e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2017
---
# <a name="quickstart-azure-ad-self-service-password-reset"></a>快速入门：Azure AD 自助密码重置

> [!IMPORTANT]
> 你是否因登录时遇到问题而浏览至此？ 如果是这样，[可按以下方式更改和重置密码](active-directory-passwords-update-your-own-password.md)。

## <a name="rapidly-deploy-self-service-password-reset"></a>快速部署自助密码重置

IT 管理员可以让用户通过自助密码重置 (SSPR) 这种简单的方法来重置或解锁其密码或帐户。 系统提供详细的报告，用于跟踪用户使用系统的时间，同时还提供通知，提醒用户存在误用或滥用情况。

本指南假定已经有了一个可以运行的试用版或许可版 Azure AD 租户。 如需 Azure AD 设置帮助，请参阅 [Azure AD 入门](https://azure.microsoft.com/trial/get-started-active-directory/)一文。

1. 在现有的 Azure AD 租户中选择“密码重置”

2. 在“属性”屏幕的“已启用自助密码重置”选项下，选择以下选项之一
    * 无人 - 没有人可以使用 SSPR 功能
    * 一个组 - 仅你所选择的特定 Azure AD 组的成员可以使用 SSPR 功能
    * 所有人 - 在 Azure AD 租户中有帐户的所有用户都可以使用 SSPR 功能

3. 在“身份验证方法”屏幕中，可选择以下选项
    * 进行重置所需的方法数 - 我们支持的最小数目为 1，最大数目为 2
    * 可供用户使用的方法 - 至少需要一种方法，也可提供额外的选项
        * **电子邮件**：向用户配置的身份验证电子邮件地址发送含代码的电子邮件
        * **手机**：允许用户选择是通过其配置的手机号码接电话，还是接收含代码的短信
        * **办公电话**：通过用户配置的办公电话号码呼叫用户，告知其代码
        * **安全问题**：要求选择
            * 注册所需的问题数 - 成功注册所需的最少问题数，即用户可以选择回答更多的问题，以便创建一个问题池，其中的问题可供抽取。 此选项可以设置为 3-5 个问题，该数目必须大于或等于重置所需的问题数。
                * 可以添加自定义问题，只需在选择安全问题时单击“自定义”按钮即可
            * 重置所需的问题数 - 可以设置为 3-5 个问题，用户必须正确回答这些问题才能重置或解锁密码。

4. 自定义（建议）：用于更改“联系管理员”链接，使之指向你所定义的页面或电子邮件地址

5. 注册（可选）：此屏幕为管理员提供以下选项：
    * 要求用户在登录时注册
    * 用户必须在几天后重新确认其身份验证信息

6. 可选：“通知”屏幕为管理员提供以下选项：
    * 重置密码时通知用户
    * 当其他管理员重置其密码时通知所有管理员

你目前已为 Azure AD 租户配置 SSPR。 可以就此打住，也可以继续配置将密码同步到本地 AD 域这一功能。

> [!NOTE]
> 请使用用户而不是管理员来测试 SSPR，因为 Microsoft 对 Azure 管理员类型帐户强制实施强身份验证要求。 有关管理员密码策略的详细信息，请参阅[密码策略文章](active-directory-passwords-policy.md#administrator-password-policy-differences)。

## <a name="configure-synchronization-to-existing-identity-source"></a>配置到现有标识源的同步

若要启用本地标识到 Azure AD 的同步，需在组织的服务器上安装并配置 [Azure AD Connect](./connect/active-directory-aadconnect.md)。 该应用程序负责将用户和组从现有的标识源同步到 Azure AD 租户。

* [从 DirSync 或 Azure AD Sync 升级到 Azure AD Connect](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [通过快速设置开始使用 Azure AD Connect](./connect/active-directory-aadconnect-get-started-express.md)
* [配置密码写回](active-directory-passwords-writeback.md#configuring-password-writeback)，将密码从 Azure AD 写回到本地目录。

## <a name="disabling-self-service-password-reset"></a>禁用自助密码重置

禁用自助密码重置很简单：打开 Azure AD 租户，转到“密码重置”>“属性”> 在“已启用自助密码重置”下选择“无”

### <a name="learn-more"></a>了解详细信息
以下链接提供了有关使用 Azure AD 进行密码重置的其他信息

* [**授权**](active-directory-passwords-licensing.md) - 配置 Azure AD 授权
* [**数据**](active-directory-passwords-data.md) - 了解所需的数据以及如何使用它进行密码管理
* [**推出**](active-directory-passwords-best-practices.md) - 使用此处提供的指南计划 SSPR 并将其部署到用户
* [**自定义**](active-directory-passwords-customize.md) - 自定义公司的 SSPR 体验的外观。
* [**策略**](active-directory-passwords-policy.md) - 了解并设置 Azure AD 密码策略
* [**报告**](active-directory-passwords-reporting.md) - 了解用户是否访问 SSPR 功能，以及在何时何处进行访问
* [深入技术探究](active-directory-passwords-how-it-works.md) - 了解幕后的工作原理
* [**常见问题**](active-directory-passwords-faq.md) - 如何？ 为什么？ 什么？ 何处？ 谁？ 何时？ - 常见问题的答案
* [**故障排除**](active-directory-passwords-troubleshoot.md) - 了解如何解决使用 SSPR 时遇到的常见问题

## <a name="next-steps"></a>后续步骤

本快速入门介绍了如何为用户配置自助密码重置。 若要继续使用 Azure 门户来完成这些步骤，请单击下面的门户链接。

> [!div class="nextstepaction"]
> [启用自助服务密码重置](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

