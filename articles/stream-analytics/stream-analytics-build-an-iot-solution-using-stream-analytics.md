---
title: "使用流分析构建 IoT 解决方案 | Microsoft Docs"
description: "使用收费站方案了解流分析 IoT 解决方案的入门教程"
keywords: "iot 解决方案, 开窗函数"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: a93693ef7d40025fa96846594a8eb525a50b6885
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="build-an-iot-solution-by-using-stream-analytics"></a>使用流分析构建 IoT 解决方案
## <a name="introduction"></a>介绍
在本教程中，将学习如何使用 Azure 流分析来实时了解数据。 开发人员可以轻松将数据流（例如点击流、日志和设备生成的时间）与历史记录或引用数据结合起来，获取业务信息。 由 Microsoft Azure 托管的 Azure 流分析是可完全管理的实时流计算服务，它提供内置冗余、低延迟及伸缩性，可让用户在几分钟之内就立刻上手。

完成本教程之后，能够：

* 熟悉 Azure 流分析门户。
* 配置和部署流式处理作业。
* 使用流分析查询语言来表达真实世界的问题并解决这些问题。
* 自信地使用流分析为客户开发流解决方案。
* 使用监视和日志记录体验来排解问题。

## <a name="prerequisites"></a>先决条件
若要完成本教程，需要以下必备条件：

* 最新版本的 [Azure PowerShell](/powershell/azure/overview)
* Visual Studio 2017、2015 或免费版 [Visual Studio Community](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
* [Azure 订阅](https://azure.microsoft.com/pricing/free-trial/)
* 计算机上的管理权限
* 从 Microsoft 下载中心下载 [TollApp.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TollApp/TollApp.zip)
* 可选：[GitHub](https://aka.ms/azure-stream-analytics-toll-source) 中 TollApp 事件生成器的源代码

## <a name="scenario-introduction-hello-toll"></a>方案简介：“你好，收费站！”
收费站是常见景象。 在世界各地的高速公路、桥梁和隧道旁边都会看到它们。 每个收费站有多个收费亭。 在手动收费亭前，要停下来向办事员支付通行费。 在自动收费亭前，穿过收费亭时，每个收费亭上的传感器将扫描安装在汽车挡风玻璃上的 RFID 卡。 我们可以轻松地将车辆通过这些收费站的情况想象成能够执行许多有趣操作的事件流。

![收费亭前汽车的图片](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>传入的数据
本教程适用于两个数据流。 安装在收费站入口和出口的传感器产生第一个流。 第二个流是包含汽车注册数据的静态查找数据集。

### <a name="entry-data-stream"></a>入口数据流
入口数据流包含汽车进入收费站时的相关信息。

| TollID | EntryTime | LicensePlate | 状态 | 制造商 | 模型 | VehicleType | VehicleWeight | 收费站 | 标记 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 |2014-09-10 12:01:00.000 |JNB 7001 |NY |Honda |CRV |1 |0 |7 | |
| 1 |2014-09-10 12:02:00.000 |YXZ 1001 |NY |Toyota |Camry |1 |0 |4 |123456789 |
| 3 |2014-09-10 12:02:00.000 |ABC 1004 |CT |Ford |Taurus |1 |0 |5 |456789123 |
| 2 |2014-09-10 12:03:00.000 |XYZ 1003 |CT |Toyota |Corolla |1 |0 |4 | |
| 1 |2014-09-10 12:03:00.000 |BNJ 1007 |NY |Honda |CRV |1 |0 |5 |789123456 |
| 2 |2014-09-10 12:05:00.000 |CDE 1007 |NJ |Toyota |4x4 |1 |0 |6 |321987654 |

下面是每个列的简短说明：

| 列 | 说明 |
| --- | --- |
| TollID |唯一标识收费亭的收费亭 ID |
| EntryTime |汽车进入收费亭的日期和时间（世界协调时） |
| LicensePlate |汽车的牌照号码 |
| 状态 |美国的某个州 |
| 制造商 |汽车制造商 |
| 模型 |汽车型号 |
| VehicleType |1（客车）或 2（商用车） |
| WeightType |汽车的重量，单位为吨；0 代表客车 |
| 收费站 |通行费，单位为美元 |
| 标记 |汽车上可用于自动付费的 e-Tag，空白代表手动付款 |

### <a name="exit-data-stream"></a>出口数据流
出口数据流包含汽车离开收费站的相关信息。

| **TollId** | **ExitTime** | **LicensePlate** |
| --- | --- | --- |
| 1 |2014-09-10T12:03:00.0000000Z |JNB 7001 |
| 1 |2014-09-10T12:03:00.0000000Z |YXZ 1001 |
| 3 |2014-09-10T12:04:00.0000000Z |ABC 1004 |
| 2 |2014-09-10T12:07:00.0000000Z |XYZ 1003 |
| 1 |2014-09-10T12:08:00.0000000Z |BNJ 1007 |
| 2 |2014-09-10T12:07:00.0000000Z |CDE 1007 |

下面是每个列的简短说明：

| 列 | 说明 |
| --- | --- |
| TollID |唯一标识收费亭的收费亭 ID |
| ExitTime |汽车离开收费亭的日期和时间（世界协调时） |
| LicensePlate |汽车的牌照号码 |

### <a name="commercial-vehicle-registration-data"></a>商用车注册数据
本教程使用商用车注册数据库的静态快照。

| LicensePlate | RegistrationId | Expired |
| --- | --- | --- |
| SVT 6023 |285429838 |1 |
| XLZ 3463 |362715656 |0 |
| BAC 1005 |876133137 |1 |
| RIV 8632 |992711956 |0 |
| SNY 7188 |592133890 |0 |
| ELH 9896 |678427724 |1 |

下面是每个列的简短说明：

| 列 | 说明 |
| --- | --- |
| LicensePlate |汽车的牌照号码 |
| RegistrationId |汽车的注册 ID |
| Expired |汽车的注册状态：0 代表汽车注册仍有效，1 代表汽车注册已过期 |

## <a name="set-up-the-environment-for-azure-stream-analytics"></a>设置 Azure 流分析的环境
若要完成本教程，需要一个 Microsoft Azure 订阅。 Microsoft 提供免费试用版的 Microsoft Azure 服务。

如果没有 Azure 帐户，可以[请求免费试用版](http://azure.microsoft.com/pricing/free-trial/)。

> [!NOTE]
> 若要注册免费试用版，必须有可接收短信的移动设备和有效的信用卡。
> 
> 

请务必按照本文末尾的“清理 Azure 帐户”部分中的步骤操作，以便充分利用 Azure 信用额度。

## <a name="provision-azure-resources-required-for-the-tutorial"></a>预配本教程所需的 Azure 资源
本教程需要两个事件中心来接收“入口”和“出口”数据流。 Azure SQL 数据库输出流分析作业的结果。 Azure 存储存储有关汽车注册的引用数据。

可以使用 GitHub 上 TollApp 文件夹中的 Setup.ps1 脚本创建所有必要的资源。 为了节省时间，我们建议运行此脚本。 如果想要详细了解如何在 Azure 门户中配置这些资源，请参阅附录“在 Azure 门户中配置教程资源”。

下载并保存 [TollApp](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TollApp/TollApp.zip) 支持文件夹和文件。

*以管理员身份*打开“Microsoft Azure PowerShell”窗口。 如果还没有安装 Azure PowerShell，请根据[安装和配置 Azure PowerShell](/powershell/azure/overview)中的说明安装 Azure PowerShell。

因为 Windows 会自动阻止 .ps1、.dll 和 .exe 文件，所以在运行脚本前需要设置执行策略。 确保*以管理员身份*运行 Azure PowerShell 窗口。 运行“Set-ExecutionPolicy unrestricted”。 出现提示时键入“Y”。

![“Set-ExecutionPolicy unrestricted”在 Azure PowerShell 窗口中运行的屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

运行 **Get-ExecutionPolicy**，确保命令正常工作。

![“Get-ExecutionPolicy”在 Azure PowerShell 窗口中运行的屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

转到包含脚本和生成器应用程序的目录。

![“cd .\TollApp\TollApp”在 Azure PowerShell 窗口中运行的屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

键入“.\\Setup.ps1”，设置 Azure 帐户，创建并配置所有所需资源，然后开始生成事件。 脚本将随机挑选一个区域来创建资源。 若要显式指定某个区域，可以传递 **-location** 参数，如以下示例所示：

**.\\Setup.ps1 -location “Central US”**

![Azure 登录页的屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

脚本打开 Microsoft Azure 的“登录”页。 输入帐户凭据。

> [!NOTE]
> 如果帐户有权访问多个订阅，系统将要求输入用于本教程的订阅名称。
> 
> 

脚本可能需要几分钟的时间来运行。 完成后，输出应如下屏幕截图所示。

![Azure PowerShell 窗口中脚本输出的屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

还将看到类似于以下屏幕截图的其他窗口。 此应用程序将事件发送到 Azure 事件中心，因为运行本教程需要这样做。 因此，在完成本教程之前不要停止此应用程序，或者关闭此窗口。

![“发送事件中心数据”屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

现在，Azure 门户中应会显示资源。 转到 <https://portal.azure.com>，并使用帐户凭据登录。 请注意，当前某些功能会使用经典门户。 相关步骤将清晰标出。

### <a name="azure-event-hubs"></a>Azure 事件中心
在 Azure 门户的左侧管理窗格底部，单击“更多服务”。 在提供的字段中键入“事件中心”，并单击“事件中心”。 这会启动新的浏览器窗口，显示**经典门户**的**服务总线**区域。 可在此处查看由 Setup.ps1 脚本创建的事件中心。

![服务总线](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image8.png)

单击以 *tolldata* 开头的项。 单击“事件中心”选项卡。会看到在此命名空间中创建的两个事件中心，分别名为 *entry* 和 *exit*。

![经典门户中的“事件中心”选项卡](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image9.png)

### <a name="azure-storage-container"></a>Azure 存储容器
1. 返回到浏览器中打开指向 Azure 门户的选项卡。 单击 Azure 门户左侧的“存储”，查看在教程中使用的 Azure 存储容器。
   
    ![存储菜单项](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image11.png)
2. 单击以 *tolldata* 开头的项。 单击“容器”选项卡，查看创建的容器。
   
    ![Azure 门户中的“容器”选项卡](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image10.png)
3. 单击 **tolldata** 容器，查看已上传的、包含汽车注册数据的 JSON 文件。
   
    ![容器中 registration.json 文件的屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image12.png)

### <a name="azure-sql-database"></a>Azure SQL 数据库
1. 返回在浏览器中打开的第一个选项卡上的 Azure 门户。 单击 Azure 门户左侧的“SQL 数据库”，查看要在本教程中使用的 SQL 数据库，并单击“tolldatadb”。
   
    ![创建的 SQL 数据库屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)
2. 复制服务器名称，但不要复制端口号（例如 *服务器名称*.database.windows.net）。
    ![已创建的 SQL 数据库 db 的屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15a.png)

## <a name="connect-to-the-database-from-visual-studio"></a>从 Visual Studio 连接到数据库
使用 Visual Studio 访问输出数据库中的查询结果。

从 Visual Studio 连接到 SQL 数据库（目标）：

1. 打开 Visual Studio，依次单击“工具” > “连接到数据库”。
2. 出现提示时，单击“Microsoft SQL Server”作为数据源。
   
    ![更改“数据源”对话框](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)
3. 在“服务器名称”字段中，粘贴从 Azure 门户的之前部分中复制的名称（即*服务器名称*.database.windows.net）。
4. 单击“使用 SQL Server 身份验证”。
5. 在“用户名”字段中输入“tolladmin”，并在“密码”字段中输入“123toll!” 。
6. 单击“选择或输入数据库名称”，并选择“TollDataDB”作为数据库。
   
    ![“添加连接”对话框](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)
7. 单击 **“确定”**。
8. 打开 Server Explorer。
   
    ![服务器资源管理器](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)
9. 在 TollDataDB 数据库中查看四个表。
   
    ![TollDataDB 数据库中的表](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>事件生成器：TollApp 示例项目
PowerShell 脚本使用 TollApp 示例应用程序自动发送事件。 不需要执行任何附加步骤。

但是，如果对实现的细节有兴趣，可以在 GitHub [samples/TollApp](https://aka.ms/azure-stream-analytics-toll-source) 中找到 TollApp 应用程序的源代码。

![在 Visual Studio 中显示的示例代码的屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>创建流分析作业
1. 在 Azure 门户中，单击页面左上角的绿色加号，创建新的流分析作业。 选择“智能+分析”，并单击“流分析作业”。
   
    ![新建按钮](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)
2. 请提供作业名称，验证订阅是否正确，并在事件中心存储所在的区域（此脚本默认为“美国中南部”）创建新的资源组。
3. 在页面底部单击“固定到仪表板”，并单击“创建”。
   
    ![创建流分析作业选项](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>定义输入源
1. 将创建作业并打开作业页面。 还可在门户仪表板上单击已创建的分析作业。

2. 单击“输入”选项卡，定义源数据。
   
    ![“输入”选项卡](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.png)
3. 单击“添加输入”。
   
    ![“添加输入”选项](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)
4. 输入“EntryStream”作为“输入别名”。
5. 源类型是**数据流**
6. 源是**事件中心**。
7. **服务总线命名空间**应是下拉菜单中的 TollData。
8. **事件中心名称**应设置为**条目**。
9. 事件中心策略名称*为 RootManageSharedAccessKey（默认值）。
10. 选择“JSON”作为“事件序列化格式”，选择“UTF8”作为“编码”。
   
    设置看起来类似于：
   
    ![事件中心设置](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

10. 单击页面底部的“创建”完成向导。
    
    现在，已创建入口流，可执行相同的步骤来创建出口流。 请确保输入如以下屏幕截图所示的值。
    
    ![出口流的设置](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)
    
    定义了两种输入流：
    
    ![在 Azure 门户中定义的输入流](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.png)
    
    接下来，为包含汽车注册数据的 blob 文件添加引用数据输入。
11. 单击“添加”，执行与流输入相同的过程，但不同的是要选择“引用数据”而不是“数据流”，并且“输入别名”是“注册”。

12. 存储帐户以 **tolldata** 开头。 容器名称应为 **tolldata**，**路径模式**应为 **registration.json**。 此文件名区分大小写，且应该全为**小写**。
    
    ![博客存储设置](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)
13. 单击“创建”完成向导。

现在，已定义所有输入。

## <a name="define-output"></a>定义输出
1. 在“流分析作业概述”窗格中，选择“输出”。
   
    ![“输出”选项卡和“添加输出”选项](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.png)
2. 单击 **“添加”**。
3. 将“输出别名”设置为“output”，并将“接收器”设置为“SQL 数据库”。
3. 选择在文章“从 Visual Studio 连接到数据库”部分中使用的服务器名称。 数据库名称为“TollDataDB”。
4. 在“用户名”字段中输入“tolladmin”、在“密码”字段中输入“123toll!” ，并在“表”字段中输入“TollDataRefJoin”。
   
    ![SQL 数据库设置](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.png)
5. 单击“创建” 。

## <a name="azure-stream-analytics-query"></a>Azure 流分析查询
“查询”选项卡包含转换传入数据的 SQL 查询。

![添加到“查询”选项卡的查询](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.png)

本教程尝试回答几个与通行费数据相关的业务问题，并构造可在 Azure 流分析中使用的流分析查询以提供相关的答案。

在开始第一个流分析作业前，我们先了解几种方案和查询语法。

## <a name="introduction-to-azure-stream-analytics-query-language"></a>Azure 流分析查询语言简介
- - -
假设需要统计进入某个收费亭的汽车数目。 由于这是连续的事件流，所以必须定义一个“时段”。 我们将问题修改为“每三分钟有多少汽车进入收费亭？”。 这通常称为轮转计数。

我们看看回答此问题的 Azure 流分析查询：

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

如你所见，Azure 流分析使用类似 SQL 的查询语言，并且添加了一些扩展以指定查询与时间相关的方面。

有关详细信息，请参阅 MSDN 中的[时间管理](https://msdn.microsoft.com/library/azure/mt582045.aspx)和查询中所用的[窗口化](https://msdn.microsoft.com/library/azure/dn835019.aspx)构造。

## <a name="testing-azure-stream-analytics-queries"></a>测试 Azure 流分析查询
既然已经编写了第一个 Azure 流分析查询，现在就可以使用位于 TollApp 文件夹中以下路径的示例数据文件来测试该查询：

**..\\TollApp\\TollApp\\Data**

此文件夹包含以下文件：

* Entry.json
* Exit.json
* Registration.json

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>问题 1：进入某个收费亭的汽车数量
1. 打开 Azure 门户，转到创建的 Azure 流分析作业。 单击“查询”选项卡，并粘贴前一部分中的查询。

2. 要根据示例数据验证此查询，请单击 ... 符号，再选择“上传文件中的示例数据”，将数据上传到 EntryStream 输入。

    ![Entry.json 文件的屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)
3. 在显示的窗格中，选择本地计算机上的文件 (Entry.json)，并单击“确定”。 “测试”图标现将变亮且可单击。
   
    ![Entry.json 文件的屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.png)
3. 验证查询的输出是否与以下预期结果相同：
   
    ![测试结果](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.png)

## <a name="question-2-report-total-time-for-each-car-to-pass-through-the-toll-booth"></a>问题 2：报告每辆车通过收费亭的总时间
了解汽车通过收费站所需的平均时间有助于评估进度效率和客户体验。

若要查找总时间，需要联接 EntryTime 流和 ExitTime 流。 需要联接 TollId 和 LicencePlate 列中的流。 **JOIN** 运算符要求指定一个临时余量，以描述联接事件之间可接受的时间差。 使用 **DATEDIFF** 函数来指定事件之间的时间差不能超过 15 分钟。 我们还将 **DATEDIFF** 函数应用到出口及入口时间，以计算汽车经过收费站的实际时间。 请注意相比 **JOIN** 条件，在 **SELECT** 语句中使用 **DATEDIFF** 的差异。

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. 若要测试此查询，请在作业的**查询**上更新该查询。 按上文输入 **EntryStream** 的方式为 **ExitStream** 添加测试文件。
   
2. 单击“测试”。

3. 选择复选框，测试查询并查看输出：
   
    ![测试输出](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>问题 3：报告注册已过期的所有商用车
Azure 流分析可以使用静态数据快照来与临时数据流联接。 为了演示此功能，请使用以下示例问题。

如果某辆商用车已向收费公司注册，则可以直接通过收费亭，而不用停车接受检查。 使用商用车登记查找表来识别登记已过期的所有商用车。

```
SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
FROM EntryStream TIMESTAMP BY EntryTime
JOIN Registration
ON EntryStream.LicensePlate = Registration.LicensePlate
WHERE Registration.Expired = '1'
```

若要使用引用数据测试查询，需要为引用数据定义输入源，而此操作已经完成。

要测试此查询，请将查询粘贴到“查询”选项卡，单击“测试”并指定两个输入源和注册示例数据，并单击“测试”。  
   
![测试输出](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

## <a name="start-the-stream-analytics-job"></a>启动流分析作业
现在即可结束配置，并启动作业。 保存问题 3 中的查询，这会生成与输出表 **TollDataRefJoin** 架构匹配的输出。

转到作业“仪表板”并单击“启动”。

![作业仪表板中“开始”按钮的屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.png)

在打开的对话框中，将“开始输出”时间更改为“自定义时间”。 将时间更改为早于当前时间一个小时。 此更改确保了从在教程开头生成事件时，事件中心中的所有事件都得到处理。 现单击“开始”按钮以开始作业。

![选择自定义事件](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

启动作业可能需要几分钟时间。 可以在流分析的顶级页中查看状态。

![作业状态的屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.png)

## <a name="check-results-in-visual-studio"></a>在 Visual Studio 中检查结果
1. 打开 Visual Studio 服务器资源管理器，并右键单击“TollDataRefJoin”表。
2. 单击“显示表数据”，查看作业的输出。
   
    ![选择服务器资源管理器中的“显示表数据”](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>扩大 Azure 流分析作业
Azure 流分析能够弹性缩放，因而能够处理大量数据。 Azure 流分析查询可以使用 **PARTITION BY** 子句来告诉系统此步骤会扩大。**PartitionId** 是系统加入以与输入（事件中心）的分区 ID 匹配的特殊列。

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. 停止当前作业，更新“查询”选项卡中的查询，并打开作业仪表板中的“设置”齿轮。 单击“缩放”。
   
    **流式处理单位**定义了作业能够接收的计算能力大小。
2. 将下拉菜单中的 1 更改为 6。
   
    ![选择 6 个流式处理单位的屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.png)
3. 转到“输出”选项卡，并将 SQL 表名称更改为“TollDataTumblingCountPartitioned”。

如果此时启动作业，Azure 流分析可将工作分散到更多计算资源上，并实现更高的吞吐量。 请注意，TollApp 应用程序还会发送已按 TollId 分区的事件。

## <a name="monitor"></a>监视
“监视器”区域包含正在运行的作业的相关统计信息。 首次需要配置，才能使用同一区域中的存储帐户（按本文档其余部分命名收费站）。   

![监视器屏幕截图](media/stream-analytics-build-an-iot-solution-using-stream-analytics/monitoring.png)

还可通过作业仪表板的“设置”区域访问“活动日志”。


## <a name="conclusion"></a>结束语
本教程介绍 Azure 流分析服务。 它演示了如何为流分析作业配置输入和输出。 该教程还使用收费站数据方案来解释在数据空间不断变化时所引发的常见问题类型，以及如何在 Azure 流分析中使用类似于 SQL 的简单查询来解决这些问题。 本教程介绍了用于处理临时数据的 SQL 扩展构造。 它演示了如何联接不同的数据流、如何使用静态引用数据来扩充数据流以及如何扩大查询以达成更高的吞吐量。

尽管本教程进行了详细的介绍，但它不可能面面俱到。 可通过在[常用流分析使用模式的查询示例](stream-analytics-stream-analytics-query-patterns.md)中使用 SAQL 语言，发现更多查询模式。
若要了解有关 Azure 流分析的详细信息，请参阅[联机文档](https://azure.microsoft.com/documentation/services/stream-analytics/)。

## <a name="clean-up-your-azure-account"></a>清理 Azure 帐户
1. 请在 Azure 门户中停止流分析作业。
   
    Setup.ps1 脚本将创建两个事件中心和一个 SQL 数据库。 以下说明可帮助你在本教程结束时清理资源。
2. 在 PowerShell 窗口中键入“.\\Cleanup.ps1”，启动用于删除本教程所用资源的脚本。
   
   > [!NOTE]
   > 资源按名称标识。 在确认删除之前，请确保仔细检查每个项。
   > 
   > 


