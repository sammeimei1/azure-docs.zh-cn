---
title: "从模板创建工作流 - Azure 逻辑应用 | Microsoft Docs"
description: "入门 - 使用 Azure 逻辑应用模板快速创建工作流，以连接应用并集成数据。"
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 3656acfb-eefd-4e75-b5d2-73da56c424c9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 89272869f7dfaa34cbd2ad32d67dca0955e6158b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-workflow-using-a-pre-built-template-or-pattern-to-get-started-quickly"></a>使用预建模板或模式配置快速入门的工作流

## <a name="what-are-logic-app-templates"></a>什么是逻辑应用模板
逻辑应用模板是预建的逻辑应用，可用于快速开始创建自己的工作流。 

这些模板是发现可使用逻辑应用构建的各种模式的好办法。 可以照原样使用这些模板或对其进行修改以适应具体的方案。

## <a name="overview-of-available-templates"></a>可用模板概述
逻辑应用平台当前发布了许多可用的模板。 下面列出了部分示例类别，以及其中使用的连接器类型。

### <a name="enterprise-cloud-templates"></a>企业云模板
通过集成 Dynamics CRM、Salesforce、Box、Azure Blob 以及其他连接器来满足企业云需求的模板。 可以使用这些模板执行的操作的一些示例包括：组织潜在客户和备份企业文件数据。

### <a name="enterprise-integration-pack-templates"></a>Enterprise Integration Pack 模板
配置 VETER 管道（验证、提取、转换、扩展、路由），通过 AS2 接收 X12 EDI 文档并将其转换为 XML，以及处理 X12 和 AS2 消息。

### <a name="protocol-pattern-templates"></a>协议模式模板
这些模板由包含协议模式（如通过 HTTP 的请求响应以及跨 FTP 和 SFTP 集成）的逻辑应用组成。 照原样使用，或作为基础，创建更复杂的协议模式。  

### <a name="personal-productivity-templates"></a>个人工作效率模板
有助于提高个人工作效率的模式包括具有如下功能的模板：设置日常提醒，将重要的工作项目添加为待办事项，以及通过自动化将耗时较长的任务缩减为单个用户同意的步骤。

### <a name="consumer-cloud-templates"></a>消费者云模板
与社交媒体服务（例如 Twitter、Slack 和电子邮件）集成的简单模板，最终都能够强化社交媒体营销方案。 这些模板还包括云复制等模板，通过节省花费在传统重复性任务上的时间提高工作效率。 

## <a name="how-to-create-a-logic-app-using-a-template"></a>如何使用模板创建逻辑应用
若要开始使用逻辑应用模板，请转到逻辑应用设计器。 如果通过打开现有逻辑应用进入设计器，设计器视图会自动加载该逻辑应用。 但是，如果要创建新的逻辑应用，会看到以下屏幕。  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

可以从该屏幕选择使用空白逻辑应用开始，或者使用预建的模板开始。 如果选择其中一种模板，会提供其他信息。 在此示例中，我们使用“在 Dropbox 中创建新文件时，将其复制到 OneDrive”模板。  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

如果选择使用该模板，只需选择“使用此模板”按钮。 根据该模板使用的连接器，将需要登录到帐户。 或者，如果以前已与该连接器建立了连接，可以选择“继续”，如下所示。  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

建立连接并选择“继续”后，逻辑应用会在设计器视图中打开。  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

在上述示例中，与使用多个模板情况一样，某些必填属性字段可以在连接器内进行填写，而某些字段可能还需先输入值才能正常部署逻辑应用。 如果尝试在不输入某些缺少字段的情况下部署，会收到错误消息通知。

如果想要返回模板查看器，请选择导航栏顶部的“模板”按钮。 如果切换回模板查看器，会丢失未保存的所有进度。 在切换回模板查看器前，将收到警告消息，通知你此情况。  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a>如何部署根据模板创建的逻辑应用
加载模板并进行任何所需更改后，选择左上角的“保存”按钮。 这会保存并发布逻辑应用。  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

若想了解如何向现有逻辑应用模板添加更多步骤，或进行常规编辑的详细信息，请参阅[创建逻辑应用](../logic-apps/logic-apps-create-a-logic-app.md)。

