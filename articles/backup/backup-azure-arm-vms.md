---
title: "将 Azure VM 备份到恢复服务保管库 | Microsoft Docs"
description: "使用本文中的 Azure 虚拟机备份过程来发现 Azure 虚拟机并将其注册和备份到恢复服务保管库。"
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: 
keywords: "虚拟机备份; 备份虚拟机; 备份和灾难恢复; arm vm 备份"
ms.assetid: 5c68481d-7be3-4e68-b87c-0961c267053e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2016
ms.author: trinadhk; jimpark; markgal;
translationtype: Human Translation
ms.sourcegitcommit: ac8df40db8ddcc84a0a6221dddd0f17fecbe6586
ms.openlocfilehash: e80d4fdb6f189bf46096422602508b0827f41a67


---
# <a name="back-up-azure-vms-to-a-recovery-services-vault"></a>将 Azure VM 备份到恢复服务保管库
> [!div class="op_single_selector"]
> * [将 VM 备份到恢复服务保管库](backup-azure-arm-vms.md)
> * [将 VM 备份到备份保管库](backup-azure-vms.md)
>
>

本文介绍了将 Azure VM（资源管理器部署和经典部署）备份到恢复服务保管库的过程。 备份 VM 的大部分工作是进行准备。 在进行备份或保护 VM 之前，必须完成[先决条件](backup-azure-arm-vms-prepare.md)中的步骤来准备好保护 VM 的环境。 完成先决条件后，可以启动备份操作来创建 VM 的快照。


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

有关详细信息，请参阅[在 Azure 中规划 VM 备份基础结构](backup-azure-vms-introduction.md)和 [Azure 虚拟机](https://azure.microsoft.com/documentation/services/virtual-machines/)。

## <a name="triggering-the-backup-job"></a>触发备份作业
与恢复服务保管库关联的备份策略定义备份操作的运行频率和时间。 默认情况下，第一个计划的备份是初始备份。 在执行初始备份之前，“**备份作业**”边栏选项卡上的“上次备份状态”显示为“**警告(等待初始备份)**”。

![等待中的备份](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

除非初始备份预计马上开始，否则建议运行“立即备份” 。 以下过程从保管库仪表板开始。 当完成所有先决条件后，此过程可用于运行初始备份作业。 如果已运行初始备份作业，此过程便不适用。 关联的备份策略将确定后续的备份作业。  

若要运行初始备份作业，请执行以下操作：

1. 在保管库仪表板上的“**备份**”磁贴中，单击“**Azure 虚拟机**”。 <br/>
    ![设置图标](./media/backup-azure-vms-first-look-arm/rs-vault-in-dashboard-backup-vms.png)

    “**备份项**”边栏选项卡随即打开。
2. 在“**备份项**”边栏选项卡中，右键单击要备份的保管库，然后单击“**立即备份**”。

    ![“设置”图标](./media/backup-azure-vms-first-look-arm/back-up-now.png)

    随即会触发备份作业。 <br/>

    ![已触发备份作业](./media/backup-azure-vms-first-look-arm/backup-triggered.png)
3. 若要查看初始备份是否已完成，在保管库仪表板上的“**备份作业**”磁贴中，单击“**Azure 虚拟机**”。

    ![“备份作业”磁贴](./media/backup-azure-vms-first-look-arm/open-backup-jobs.png)

    “备份作业”边栏选项卡随即打开。
4. 在“**备份作业**”边栏选项卡中，可以看到所有作业的状态。

    ![“备份作业”磁贴](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view.png)

   > [!NOTE]
   > 在备份操作期间，每个虚拟机中的备份扩展刷新所有写入并采用一致的快照。
   >
   >

    完成备份作业后，状态将变为“*已完成*”。

## <a name="troubleshooting-errors"></a>排查错误
如果在备份虚拟机时遇到问题，请参阅 [VM 故障排除文章](backup-azure-vms-troubleshoot.md)以获取帮助。

## <a name="next-steps"></a>后续步骤
现已保护 VM，请参阅以下文章了解 VM 管理任务以及如何还原 VM。

* [管理和监视你的虚拟机](backup-azure-manage-vms.md)
* [恢复虚拟机](backup-azure-arm-restore-vms.md)



<!--HONumber=Nov16_HO3-->

