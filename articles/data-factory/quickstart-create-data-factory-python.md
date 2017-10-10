---
title: "使用 Python 创建 Azure 数据工厂 | Microsoft Docs"
description: "创建 Azure 数据工厂来将数据从 Azure Blob 存储中的一个位置复制到同一 Blob 存储中的另一位置。"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 09/19/2017
ms.author: jingwang
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 9c6f7c8e052aed2ab5aba2ee54065bf40e81a022
ms.contentlocale: zh-cn
ms.lasthandoff: 09/25/2017

---

# <a name="create-a-data-factory-and-pipeline-using-python"></a>使用 Python 创建数据工厂和管道
Azure 数据工厂是基于云的数据集成服务，用于在云中创建数据驱动型工作流，以便协调和自动完成数据移动和数据转换。 使用 Azure 数据工厂，可以创建和计划数据驱动型工作流（称为管道），以便从不同的数据存储引入数据，通过各种计算服务（例如 Azure HDInsight Hadoop、Spark、Azure Data Lake Analytics 和 Azure 机器学习）处理/转换数据，将输出数据发布到数据存储（例如 Azure SQL 数据仓库），供商业智能 (BI) 应用程序使用。 

此快速入门教程介绍了如何使用 Python 创建一个 Azure 数据工厂。 该数据工厂中的管道将数据从 Azure blob 存储中的一个文件夹复制到另一个文件夹。

如果你还没有 Azure 订阅，可以在开始前创建一个[免费](https://azure.microsoft.com/free/)帐户。

## <a name="prerequisites"></a>先决条件

* **Azure 存储帐户**。 可以将 blob 存储用作**源**和**接收器**数据存储。 如果没有 Azure 存储帐户，请参阅[创建存储帐户](../storage/common/storage-create-storage-account.md#create-a-storage-account)一文来获取创建步骤。 
* 按照[此说明](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application)**在 Azure Active Directory 中创建应用程序**。 记下要在后面的步骤中使用的以下值：**应用程序 ID**、**身份验证密钥**和**租户 ID**。 根据同一文章中的以下说明将应用程序分配到“参与者”角色。 

### <a name="create-and-upload-an-input-file"></a>创建并上传输入文件

1. 启动记事本。 复制以下文本并将其在磁盘上另存为 **input.txt** 文件。
    
    ```
    John|Doe
    Jane|Doe
    ```
2.  使用 [Azure 存储资源管理器](http://storageexplorer.com/)等工具创建 **adfv2tutorial** 容器，并将 input.txt 文件上传到该容器。 

## <a name="install-the-python-package"></a>安装 Python 包
1. 使用管理员特权打开一个终端或命令提示符。  
2. 若要为数据工厂安装 Python 包，请运行以下命令：

    ```
    pip install azure-mgmt-datafactory
    ```

    [用于数据工厂的 Python SDK](https://github.com/Azure/azure-sdk-for-python) 支持 Python 2.7、3.3、3.4、3.5 和 3.6。
## <a name="create-a-data-factory-client"></a>创建数据工厂客户端

1. 创建一个名为 **datafactory.py** 的文件。 添加以下语句来添加对命名空间的引用。
    
    ```python
    from azure.common.credentials import ServicePrincipalCredentials
    from msrestazure.azure_cloud import Cloud, CloudEndpoints, CloudSuffixes
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.datafactory import DataFactoryManagementClient
    from azure.mgmt.datafactory.models import *
    import json
    import time    
    ```
2. 添加用于打印信息的以下函数。 

    ```python
    def print_item(group):
      """Print an Azure object instance."""
      print("\tName: {}".format(group.name))
      print("\tId: {}".format(group.id))
      if hasattr(group, 'location'):
          print("\tLocation: {}".format(group.location))
      if hasattr(group, 'tags'):
          print("\tTags: {}".format(group.tags))
      if hasattr(group, 'properties'):
          print_properties(group.properties)
    
    def print_properties(props):
      """Print a ResourceGroup properties instance."""
      if props and hasattr(props, 'provisioning_state') and props.provisioning_state:
          print("\tProperties:")
          print("\t\tProvisioning State: {}".format(props.provisioning_state))
      print("\n\n")    
    ```
3. 向 **Main** 方法中添加用于创建 DataFactoryManagementClient 类的实例的以下代码。 将使用此对象来创建数据工厂、链接服务、数据集和管道。 还将使用此对象来监视管道运行详细信息。

    ```python   
    subscription_id = '<your subscription ID where the factory resides>'
    credentials = ServicePrincipalCredentials(
            client_id='<yourClientId>',
            secret='<YourPassword>',
            tenant='<YourTenandId>'
    )
    resource_client = ResourceManagementClient(credentials, subscription_id)
    adf_client = DataFactoryManagementClient(credentials, subscription_id)
    
    rg_params = {'location':'eastus'}
    df_params = {'location':'eastus'}
    rg_name = '<Your Resource Group Name>'
    df_name = '<Your Data Factory Name>'
    ```

## <a name="create-a-data-factory"></a>创建数据工厂

向 **Main** 方法中添加用于创建**数据工厂**的以下代码。 

```python
#Create Resource Group
resource_client.resource_groups.create_or_update(rg_name, rg_params)

#Create Data Factory
df_resource = Factory(location='eastus')
df = adf_client.factories.create_or_update(rg_name, df_name, df_resource)
print_item(df)
while df.provisioning_state != 'Succeeded':
    df = adf_client.factories.get(rg_name, df_name)
    time.sleep(1)
print_item(adf_client.factories.get(rg_name, df_name))
```

## <a name="create-a-linked-service"></a>创建链接服务

向 **Main** 方法中添加用于创建 **Azure 存储链接服务**的以下代码。

可在数据工厂中创建链接服务，将数据存储和计算服务链接到数据工厂。 在此快速入门中，只需要创建一个同时用作复制源和接收器存储的 Azure 存储链接服务，在示例中名为“AzureStorageLinkedService”。

```python
#Create Storage Linked Service
ls_name = 'storageLinkedService'

#Replace Storage String with your credentials
storage_string = SecureString('DefaultEndpointsProtocol=https;AccountName=<replace>;AccountKey=<replace>')

ls_azure_storage = AzureStorageLinkedService(connection_string=storage_string)
ls = adf_client.linked_services.create_or_update(rg_name, df_name, ls_name, ls_azure_storage)
print_item(ls)
```
## <a name="create-datasets"></a>创建数据集
在本部分中，将创建两个数据集：一个用于源，另一个用于接收器。

### <a name="create-a-dataset-for-source-azure-blob"></a>为源 Azure Blob 创建数据集
向 Main 方法中添加用于创建 Azure blob 数据集的以下代码。 有关 Azure Blob 数据集的属性的信息，请参阅 [Azure Blob 连接器](connector-azure-blob-storage.md#dataset-properties)一文。 

你将在 Azure Blob 中定义表示源数据的数据集。 此 Blob 数据集引用在上一步中创建的 Azure 存储链接服务。

```python
#Create Dataset Input
ds_name = 'ds_in'
ds_ls = LinkedServiceReference(ls_name)
blob_path= 'adfv2tutorial/'
blob_filename = 'input.txt'
ds_azure_blob= AzureBlobDataset(ds_ls, folder_path=blob_path, file_name = blob_filename)
ds = adf_client.datasets.create_or_update(rg_name, df_name, ds_name, ds_azure_blob)
print_item(ds)
```

### <a name="create-a-dataset-for-sink-azure-blob"></a>为接收器 Azure Blob 创建数据集
向 Main 方法中添加用于创建 Azure blob 数据集的以下代码。 有关 Azure Blob 数据集的属性的信息，请参阅 [Azure Blob 连接器](connector-azure-blob-storage.md#dataset-properties)一文。 

你将在 Azure Blob 中定义表示源数据的数据集。 此 Blob 数据集引用在上一步中创建的 Azure 存储链接服务。

```python
#Create Dataset Output
dsOut_name = 'ds_out'
output_blobpath = 'output/'
dsOut_azure_blob = AzureBlobDataset(ds_ls, folder_path=output_blobpath)
dsOut = adf_client.datasets.create_or_update(rg_name, df_name, dsOut_name, dsOut_azure_blob)
print_item(dsOut)
```

## <a name="create-a-pipeline"></a>创建管道

向 **Main** 方法中添加用于创建**包含复制活动的管道**的以下方法。

```python
#Create 1st activity: Copy Activity
act_name =  'copyBlobtoBlob'
blob_source = BlobSource()
blob_sink = BlobSink()
dsin_ref = DatasetReference(ds_name)
dsOut_ref = DatasetReference(dsOut_name)
copy_activity = CopyActivity(act_name,inputs=[dsin_ref], outputs=[dsOut_ref], source=blob_source, sink=blob_sink)

#Create Pipeline
p_name =  'copyPipeline'
params_for_pipeline = {}
p_obj = PipelineResource(activities=[copy_activity], parameters=params_for_pipeline)
p = adf_client.pipelines.create_or_update(rg_name, df_name, p_name, p_obj)
print_item(p)
```


## <a name="create-a-pipeline-run"></a>创建管道运行

向 **Main** 方法中添加用于**触发管道运行**的以下代码。

```python
#Create Pipeline Run
print(adf_client.pipelines.create_run(rg_name, df_name, p_name,
    {
    }
))
```

## <a name="run-the-code"></a>运行代码
生成并启动应用程序，然后验证管道执行。

控制台会输出数据工厂、链接服务、数据集、管道和管道运行的创建进度。 请等待，直至看到包含数据读取/写入大小的复制活动运行详细信息。 然后，使用 [Azure 存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)等工具检查 blob 是否已根据你在变量中的指定从“inputBlobPath”复制到“outputBlobPath”。


## <a name="clean-up-resources"></a>清理资源
若要以编程方式删除数据工厂，请向程序中添加以下代码行： 

```csharp
adf_client.data_factories.delete(rg_name, df_name)
```

## <a name="next-steps"></a>后续步骤
此示例中的管道将数据从 Azure Blob 存储中的一个位置复制到另一个位置。 完成[教程](tutorial-copy-data-dot-net.md)来了解在更多方案中使用数据工厂。 