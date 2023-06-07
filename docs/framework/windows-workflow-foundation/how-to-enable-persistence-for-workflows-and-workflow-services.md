---
title: 如何：对工作流和工作流服务启用持久性
description: 了解如何配置 SQL 工作流实例存储以通过编程方式和使用配置文件为工作流和工作流服务启用持久性。
ms.date: 03/30/2017
ms.assetid: 2b1c8bf3-9866-45a4-b06d-ee562393e503
ms.openlocfilehash: 31fe6e3f06989e9a42254747565342cf97e4b9f1
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83421509"
---
# <a name="how-to-enable-persistence-for-workflows-and-workflow-services"></a>如何：对工作流和工作流服务启用持久性

本主题说明如何对工作流和工作流服务启用持久性。

## <a name="enable-persistence-for-workflows"></a>对工作流启用持久性

通过使用类的属性，可以将实例存储与**WorkflowApplication**关联 <xref:System.Activities.WorkflowApplication.InstanceStore%2A> <xref:System.Activities.WorkflowApplication> 。 <xref:System.Activities.WorkflowApplication.Persist%2A> 方法将工作流保存或持久保存到与应用程序关联的实例存储中。 <xref:System.Activities.WorkflowApplication.Unload%2A> 方法将工作流保存到实例存储中，然后从内存中卸载该实例。 **Load**方法使用实例持久性存储区中存储的工作流数据将工作流加载到内存中。

**持久性**方法执行以下步骤：

1. 暂停工作流计划程序并等待，直至工作流进入空闲状态。

2. 将工作流持久保存或保存到持久性存储。

3. 恢复工作流计划程序。

 **Unload**方法执行以下步骤：

1. 暂停工作流计划程序并等待，直至工作流进入空闲状态。

2. 将工作流持久保存或保存到持久性存储。

3. 在内存中释放工作流实例。

当工作流处于非持久性区域中时，始终会阻止**持久性**和**Unload**方法，直到工作流退出非持久性区域。 在完成非持久性区域之后，该方法将继续执行保存或卸载操作。 如果在完成非持久性区域之前发生超时，或者如果持久性进程的时间太长，则将引发 TimeoutException。

## <a name="enable-persistence-for-workflow-services-in-code"></a>在代码中实现工作流服务的持久性

类的**DurableInstancingOptions**成员 <xref:System.ServiceModel.WorkflowServiceHost> 具有一个名为**InstanceStore**的属性，可用于将实例存储与**WorkflowServiceHost**相关联。

```csharp
// wsh is an instance of WorkflowServiceHost class
wsh.DurableInstancingOptions.InstanceStore = new SqlWorkflowInstanceStore();
```

当**WorkflowServiceHost**打开时，如果**DurableInstancingOptions**不为 null，则会自动启用持久性。

通常，服务行为通过使用**InstanceStore**属性提供要与工作流服务主机一起使用的具体实例存储区。 例如，SqlWorkflowInstanceStoreBehavior 创建了一个**SqlWorkflowInstanceStore**实例，对其进行配置，并将其分配给**DurableInstancingOptions**。

## <a name="enable-persistence-for-workflow-services-using-an-application-configuration-file"></a>使用应用程序配置文件实现工作流服务的持久性

可以通过将以下代码添加到您 app.config 或 web.config 文件中，使用应用程序配置文件来实现持久性：

```xml
<configuration>
  <system.serviceModel>
    <behaviors>
      <serviceBehaviors>
        <behavior name="myBehavior">
          <sqlWorkflowInstanceStore connectionString="Data Source=myDatabaseServer;Initial Catalog=myPersistenceDatabase" />
        </behavior>
      </serviceBehaviors>
    </behaviors>
  </system.serviceModel>
</configuration>
```
