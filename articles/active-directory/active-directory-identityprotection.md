---
title: "Azure Active Directory Identity Protection | Microsoft 文档"
description: "了解如何使用 Azure AD Identity Protection 来限制攻击者利用已泄露标识或设备的能力，以及保护之前怀疑或已知受到威胁的标识或设备。"
services: active-directory
keywords: "Azure Active Directory Identity Protection, Cloud App Discovery, 管理应用程序, 安全, 风险, 风险级别, 漏洞, 安全策略"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 0c7a8d68c0df729441e3f7faa5cd06066db1261d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-identity-protection"></a>Azure Active Directory 标识保护

Azure Active Directory Identity Protection 是 Azure AD Premium P2 版本的功能，支持：

- 检测影响组织标识的潜在漏洞

- 配置自动响应，可检测与组织标识相关的可以操作  

- 调查可疑事件，并采取适当的措施解决   


## <a name="getting-started"></a>入门

Microsoft 对基于云的标识的保护期超过十年。 通过 Azure Active Directory Identity Protection，可以在环境中使用与 Microsoft 用于保护标识相同的保护系统。

大多数安全违规出现在当攻击者通过窃取用户的标识来获取环境的访问权限时。 近年来，攻击者越来越擅于使用第三方违规，以及使用复杂的网络钓鱼攻击。 攻击者获取较低权限的用户帐户的访问权限后，即可通过横向移动，轻松访问重要的公司资源。

因此需要：

- 保护任何权限级别的所有标识

- 主动防止滥用遭到入侵的标识

发现标识是否遭到入侵并不容易。 Azure Active Directory 使用自适应机器学习算法和试探法来检测异常和可疑事件，这些事件能指示可能遭到入侵的标识。 Identity Protection 使用此数据生成报告和警报，使你能够评估检测到的问题并采取相应的缓解措施或补救措施。

Azure Active Directory Identity Protection 不只是一个监视和报告工具。 若要保护组织的标识，可以配置基于风险的策略，该策略可在达到指定风险级别时自动响应检测到的问题。 除了 Azure Active Directory 与 EMS 提供的条件性访问控制以外，这些策略也可以自动阻止或启用自适应补救措施，包括密码重置和强制实施多重身份验证。


#### <a name="identity-protection-capabilities"></a>Identity Protection 功能

**检测漏洞和风险帐户：**  

* 提供自定义建议，通过突显漏洞来改善整体安全局势
* 计算登录风险级别
* 计算用户风险级别


**调查风险事件：**

* 针对风险事件发送通知
* 使用相关的区分上下文的信息调查风险事件
* 提供基本工作流来跟踪调查
* 轻松使用补救措施，例如密码重置

**基于风险的条件性访问策略：**

* 通过阻止登录或请求多重身份验证质询来缓解有风险登录的策略。
* 阻止或保护有风险用户帐户的策略
* 要求用户注册多重身份验证的策略



## <a name="identity-protection-roles"></a>标识保护角色

若要对围绕 Identity Protection 实现展开的管理活动进行负载均衡，可以分配多个角色。 Azure AD Identity Protection 支持 3 个目录角色：

| 角色                         | 有权执行的操作                          | 无权执行的操作
| :--                          | ---                                |  ---   |
| 全局管理员         | 对 Identity Protection 具有完全访问权限、载入 Identity Protection| |
| 安全管理员       | 对 Identity Protection 具有完全访问权限 | 载入 Identity Protection、重置用户密码 |
| 安全读者              | 对 Identity Protection 具有只读访问权限 | 载入 Identity Protection、修改用户、配置策略、重置密码 |




有关更多详细信息，请参阅[在 Azure Active Directory 中分配管理员角色](active-directory-assign-admin-roles-azure-portal.md)



## <a name="detection"></a>检测

### <a name="vulnerabilities"></a>漏洞

Azure Active Directory Identity Protection 分析配置，并检测影响用户标识的漏洞。 有关详细信息，请参阅 [Azure Active Directory Identity Protection 检测到的漏洞](active-directory-identityprotection-vulnerabilities.md)。

### <a name="risk-events"></a>风险事件

Azure Active Directory 使用自适应机器学习算法和试探法来检测与用户标识相关的可疑操作。 系统为检测到的每个可疑操作创建记录。 这些记录也称为风险事件。  
有关详细信息，请参阅 [Azure Active Directory 风险事件](active-directory-identity-protection-risk-events.md)。


## <a name="investigation"></a>调查
Identity Protection 的使用通常从 Identity Protection 仪表板开始。

![补救](./media/active-directory-identityprotection/1000.png "补救")

在仪表板中可以访问：

* “已标记为存在风险的用户”、“风险事件”和“漏洞”等报告
* 各种设置，例如“安全策略”、“通知”和“多重身份验证注册”的配置

这通常是调查的起点，在调查过程中查看风险事件相关活动、日志和其他相关信息可以确定是否需要采取补救或缓解措施，了解标识如何遭到入侵，以及遭到入侵的标识如何被利用。

可以将调查活动绑定到 Azure Active Directory Protection 发送的每封电子邮件[通知](active-directory-identityprotection-notifications.md)。

以下部分提供了更多详细信息以及与调查相关的步骤。  


## <a name="risky-sign-ins"></a>有风险的登录

Aure Active Directory 可实时和离线检测[风险事件类型](active-directory-reporting-risk-events.md#risk-event-types)。 用户登录时检测到的各项风险事件组成名为风险登录的逻辑概念。 风险登录是指可能不是由用户帐户合法拥有者进行的登录尝试。


### <a name="sign-in-risk-level"></a>登录风险级别

登录风险级别是指不是由用户帐户合法拥有者进行登录尝试的可能性（高、中、低）。

### <a name="mitigating-sign-in-risk-events"></a>缓解登录风险事件

缓解措施可限制攻击者利用遭到入侵的标识或设备的能力，且无需将标识或设备还原到安全状态。 缓解措施不能解决以前与标识或设备关联的登录风险事件。

若要自动解除风险登录，可配置登录风险安全策略。 使用这些策略时，请考虑根据用户或登录的风险级别阻止有风险的登录，或要求用户执行多重身份验证。 这些措施可防止攻击者利用遭窃的标识造成损害，并且可以取得一些时间来保护标识。

### <a name="sign-in-risk-security-policy"></a>登录风险安全策略
登录风险策略是条件性访问策略，可评估特定登录的风险，根据预先定义的条件和规则来应用缓解措施。

![登录风险策略](./media/active-directory-identityprotection/1014.png "登录风险策略")

Azure AD Identity Protection 允许执行以下操作，帮助管理有风险登录的缓解措施：

* 设置要应用策略的用户和组：

    ![登录风险策略](./media/active-directory-identityprotection/1015.png "登录风险策略")
* 设置可触发策略的登录风险级别阈值（低、中或高）：

    ![登录风险策略](./media/active-directory-identityprotection/1016.png "登录风险策略")
* 设置触发策略时要强制实施的控制：  

    ![登录风险策略](./media/active-directory-identityprotection/1017.png "登录风险策略")
* 切换策略的状态：

    ![MFA 注册](./media/active-directory-identityprotection/403.png "MFA 注册")
* 在激活更改之前查看和评估其影响：

    ![登录风险策略](./media/active-directory-identityprotection/1018.png "登录风险策略")

#### <a name="what-you-need-to-know"></a>需要了解的事项
可以将登录风险安全策略配置为要求多重身份验证：

![登录风险策略](./media/active-directory-identityprotection/1017.png "登录风险策略")

但是，出于安全原因，此设置仅适用于已注册多重身份验证的用户。 如果尚未注册多重身份验证的用户满足要求多重身份验证的条件，该用户会被阻止。

如果想要针对有风险的登录要求执行多重身份验证，最佳实践之一是：

1. 对受影响的用户启用[多重身份验证注册策略](#multi-factor-authentication-registration-policy)。
2. 要求受影响的用户在无风险会话中登录以执行 MFA 注册

完成这些步骤可确保对有风险的登录要求执行多重身份验证。

#### <a name="best-practices"></a>最佳实践
选择“高”阈值可减少触发策略的次数，最大程度地降低对用户的影响。  

但是，它会从策略中排除标记为“低”和“中”风险的登录，而无法阻止攻击者利用遭到入侵的标识。

设置策略时：

* 排除没有/无法注册多重身份验证的用户
* 排除无法启用策略（例如无法访问技术支持）的区域中的用户
* 排除可能生成大量误报的用户（开发人员、安全分析人员）
* 在首次实施策略期间，或者必须尽量减少向最终用户显示质询时，请使用“高”阈值。
* 如果组织需要更高的安全性，请使用“低”阈值。 选择“低”阈值会显示更多的用户登录质询，但可以提高安全性。

对于大多数组织而言，建议的默认值是设置“中”阈值的规则，以便在可用性与安全性之间取得均衡。

登录风险策略：

* 应用到所有使用新式身份验证的浏览器流量和登录。
* 不会应用到使用旧式安全协议并在联合 IDP 上禁用 WS-Trust 终结点的应用程序，例如 ADFS。

Identity Protection 控制台中的“风险事件”页列出了所有事件：

* 此策略已应用到
* 可以查看活动并确定操作是否适当

如需相关用户体验的概述，请参阅：

* [有风险的登录恢复](active-directory-identityprotection-flows.md#risky-sign-in-recovery)
* [有风险的登录已阻止](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  
* [Azure AD Identity Protection 中的登录体验](active-directory-identityprotection-flows.md)  

**若要打开相关的配置对话框**：

- 在“Azure AD Identity Protection”边栏选项卡上的“配置”部分中，单击“登录风险策略”。

    ![用户风险策略](./media/active-directory-identityprotection/1014.png "用户风险策略")



## <a name="users-flagged-for-risk"></a>已标记为存在风险的用户

Azure Active Directory 针对用户检测到的所有活动[风险事件](active-directory-identity-protection-risk-events.md)组成名为存在风险的用户的逻辑概念。 已标记为存在风险的用户是指可能已泄露的用户帐户。

![已标记为存在风险的用户](./media/active-directory-identityprotection/1200.png)


### <a name="user-risk-level"></a>用户风险级别

用户风险级别指示用户的标识遭到入侵的可能性（高、中或低）。 它根据与用户的标识关联的用户风险事件进行计算。

风险事件的状态为“活动”或“已关闭”。 用户风险级别计算只考虑“活动”的风险事件。

使用以下输入计算用户风险级别：

* 影响用户的活动风险事件
* 这些事件的风险级别
* 是否已采取任何补救措施

![用户风险](./media/active-directory-identityprotection/1031.png "用户风险")

可以使用用户风险级别来创建条件性访问策略，阻止有风险的用户进行登录，或强制他们使用安全的方式更改其密码。

### <a name="closing-risk-events-manually"></a>手动关闭风险事件

在大多数情况下，我们会采取补救措施（例如重置安全密码）来自动关闭风险事件。 但是，这种做法不一定总会凑效。  
例如，在以下情况下就是这样：

* 已删除具有活动风险事件的用户
* 调查显示合法的用户执行了报告的风险事件

由于用户风险计算考虑到“活动”的风险事件，因此，可能需要通过手动关闭风险事件来手动降低风险级别。  
在调查过程中，可以选择采取以下任何措施更改风险事件的状态：

![措施](./media/active-directory-identityprotection/34.png "措施")

* **解决** - 如果在调查风险事件之后在 Identity Protection 外部采取适当的补救措施，并且应该将风险事件视为已关闭，请将事件标记为“已解决”。 解决的事件会将风险事件的状态设置为“已关闭”，此风险事件不再算作用户风险。
* **标记为误报** - 在某些情况下，可以调查某个风险事件，查明该事件是否被错误地标记为有风险。 将风险事件标记为误报，可以减少发生这种情况。 这可以帮助机器学习算法将来改善类似事件的分类。 误报事件的状态为“已关闭”，不再算作用户风险。
* **忽略** - 如果尚未采取任何补救措施，但想要从活动列表中删除风险事件，可以忽略风险事件，在这种情况下，事件状态将变为“已关闭”。 忽略的事件不算作用户风险。 这种做法只能在非常情况下使用。
* **重新激活** - 可以重新激活已手动关闭的风险事件（通过选择“解决”、“误报”或“忽略”），并将事件状态设回“活动”。 重新激活的风险事件包括在用户风险级别计算中。 无法重新激活通过补救（例如重置安全密码）关闭的风险事件。

**若要打开相关的配置对话框**：

1. 在“Azure AD Identity Protection”边栏选项卡中的“调查”下面，单击“风险事件”。

    ![手动密码重置](./media/active-directory-identityprotection/1002.png "手动密码重置")
2. 在“风险事件”列表中，单击一种风险。

    ![手动密码重置](./media/active-directory-identityprotection/1003.png "手动密码重置")
3. 在“风险”边栏选项卡中，右键单击一个用户。

    ![手动密码重置](./media/active-directory-identityprotection/1004.png "手动密码重置")

### <a name="closing-all-risk-events-for-a-user-manually"></a>手动关闭用户的所有风险事件
在 Azure Active Directory Identity Protection中，只需单击一下鼠标即可关闭用户的所有风险事件，而无需逐个手动关闭。

![措施](./media/active-directory-identityprotection/2222.png "措施")

单击“清除所有事件”时，所有事件都会关闭，受影响的用户不再有风险。

### <a name="remediating-user-risk-events"></a>补救用户风险事件

补救是保护以前疑似或已知遭到入侵的标识或设备的措施。 补救措施可让标识或设备还原到安全状态，解决以前与标识或设备关联的风险事件。

若要补救用户风险事件，可以：

* 通过重置安全密码，手动补救用户风险事件
* 通过配置用户风险安全策略，自动缓解或补救用户风险事件
* 重新创建受感染设备的映像  

#### <a name="manual-secure-password-reset"></a>手动重置安全密码
重置安全密码是许多风险事件的有效补救措施，执行后，可以自动关闭这些风险事件并重新计算用户风险级别。 可以使用 Identity Protection 仪表板为有风险的用户启动密码重置。

相关对话框中提供了重置密码的两种不同方法：

**重置密码** - 如果用户已注册多重身份验证，则选择“要求用户重置其密码”即可让用户自行解决问题。 用户在下次登录时，必须成功解决多重身份验证质询，然后必须更改密码。 如果用户帐户尚未注册多重身份验证，则无法使用此选项。

**临时密码** - 选择“生成临时密码”可立即使现有密码失效，为用户创建新的临时密码。 将新的临时密码发送到用户的备用电子邮件地址或用户的经理。 由于该密码是暂时性的，因此系统会提示用户在登录时更改密码。

![策略](./media/active-directory-identityprotection/1005.png "策略")

**若要打开相关的配置对话框**：

1. 在“Azure AD Identity Protection”边栏选项卡中，单击“已标记为存在风险的用户”。

    ![手动密码重置](./media/active-directory-identityprotection/1006.png "手动密码重置")
2. 从用户列表中，选择至少有一个风险事件的用户。

    ![手动密码重置](./media/active-directory-identityprotection/1007.png "手动密码重置")
3. 在用户边栏选项卡中，单击“重置密码”。

    ![手动密码重置](./media/active-directory-identityprotection/1008.png "手动密码重置")

### <a name="user-risk-security-policy"></a>用户风险安全策略
用户风险安全策略是条件性访问策略，可评估特定用户的风险级别，根据预先定义的条件和规则来应用补救措施和缓解措施。

![用户风险策略](./media/active-directory-identityprotection/1009.png "用户风险策略")

Azure AD Identity Protection 允许执行以下操作，帮助管理标记为有风险的用户的缓解与补救措施：

* 设置要应用策略的用户和组：

    ![用户风险策略](./media/active-directory-identityprotection/1010.png "用户风险策略")
* 设置可触发策略的用户风险级别阈值（低、中或高）：

    ![用户风险策略](./media/active-directory-identityprotection/1011.png "用户风险策略")
* 设置触发策略时要强制实施的控制：

    ![用户风险策略](./media/active-directory-identityprotection/1012.png "用户风险策略")
* 切换策略的状态：

    ![用户风险策略](./media/active-directory-identityprotection/403.png "MFA 注册")
* 在激活更改之前查看和评估其影响：

    ![用户风险策略](./media/active-directory-identityprotection/1013.png "用户风险策略")

选择“高”阈值可减少触发策略的次数，最大程度地降低对用户的影响。
但是，这会从策略中排除标记为“低”和“中”风险的用户，因而无法保护以前疑似或已知遭到入侵的标识或设备。

设置策略时：

* 排除可能生成大量误报的用户（开发人员、安全分析人员）
* 排除无法启用策略（例如无法访问技术支持）的区域中的用户
* 在首次实施策略期间，或者必须尽量减少向最终用户显示质询时，请使用“高”阈值。
* 如果组织需要更高的安全性，请使用“低”阈值。 选择“低”阈值会显示更多的用户登录质询，但可以提高安全性。

对于大多数组织而言，建议的默认值是设置“中”阈值的规则，以便在可用性与安全性之间取得均衡。

如需相关用户体验的概述，请参阅：

* [遭到入侵的帐户恢复流程](active-directory-identityprotection-flows.md#compromised-account-recovery)。  
* [遭到入侵的帐户阻止流程](active-directory-identityprotection-flows.md#compromised-account-blocked)。  

**若要打开相关的配置对话框**：

- 在“Azure AD Identity Protection”边栏选项卡上的“配置”部分中，单击“用户风险策略”。

    ![用户风险策略](./media/active-directory-identityprotection/1009.png "用户风险策略")

### <a name="mitigating-user-risk-events"></a>缓解用户风险事件
管理员可以设置用户风险安全策略，根据风险级别阻止用户登录。

阻止登录：

* 避免针对受影响用户生成新的用户风险事件
* 允许管理员手动补救影响用户标识的风险事件，并将它还原到安全状态



## <a name="multi-factor-authentication-registration-policy"></a>多重身份验证注册策略
Azure 多重身份验证是要求使用多种方式（而不仅仅是用户名和密码）对身份进行验证的一种方法。 它为用户登录和事务提供了附加的安全层。  
建议要求对用户登录执行 Azure 多重身份验证，因为这种身份验证方法可以：

* 提供强式身份验证和一系列简单的验证选项
* 在帮助组织保护帐户以及在帐户遭到入侵后进行恢复方面发挥关键的作用

![用户风险策略](./media/active-directory-identityprotection/1019.png "用户风险策略")

有关更多详细信息，请参阅[什么是 Azure 多重身份验证？](../multi-factor-authentication/multi-factor-authentication.md)

Azure AD Identity Protection 可以通过配置一个策略来帮助管理首次多重身份验证注册，在该策略中，可以：

* 设置要应用策略的用户和组：

    ![MFA 策略](./media/active-directory-identityprotection/1020.png "MFA 策略")
* 设置触发策略时要强制实施的控制：  

    ![MFA 策略](./media/active-directory-identityprotection/1021.png "MFA 策略")
* 切换策略的状态：

    ![MFA 策略](./media/active-directory-identityprotection/403.png "MFA 策略")
* 查看当前注册状态：

    ![MFA 策略](./media/active-directory-identityprotection/1022.png "MFA 策略")

如需相关用户体验的概述，请参阅：

* [多重身份验证注册流程](active-directory-identityprotection-flows.md#multi-factor-authentication-registration)。  
* [Azure AD Identity Protection 中的登录体验](active-directory-identityprotection-flows.md)。  

**若要打开相关的配置对话框**：

- 在“Azure AD Identity Protection”边栏选项卡上的“配置”部分中，单击“多重身份验证注册”。

    ![MFA 策略](./media/active-directory-identityprotection/1019.png "MFA 策略")

## <a name="next-steps"></a>后续步骤
* [第 9 频道：Azure AD 和标识展示：Identity Protection 预览](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [启用 Azure Active Directory Identity Protection](active-directory-identityprotection-enable.md)

* [Azure Active Directory Identity Protection 检测到的漏洞](active-directory-identityprotection-vulnerabilities.md)

* [Azure Active Directory 风险事件](active-directory-identity-protection-risk-events.md)

* [Azure Active Directory Identity Protection 通知](active-directory-identityprotection-notifications.md)

* [Azure Active Directory Identity Protection 演练手册](active-directory-identityprotection-playbook.md)

* [Azure Active Directory Identity Protection 术语表](active-directory-identityprotection-glossary.md)

* [Azure AD Identity Protection 中的登录体验](active-directory-identityprotection-flows.md)

* [Azure Active Directory Identity Protection - 如何取消阻止用户](active-directory-identityprotection-unblock-howto.md)

* [Azure Active Directory Identity Protection 和 Microsoft Graph 入门](active-directory-identityprotection-graph-getting-started.md)
