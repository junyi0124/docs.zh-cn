---
title: 如何：定义连接字符串
ms.date: 03/30/2017
ms.assetid: 6027335d-4e26-420d-9151-6523289b1989
ms.openlocfilehash: 9b029644e0d4e4c7467fbe1e1144579e6edb3478
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90536205"
---
# <a name="how-to-define-the-connection-string"></a>如何：定义连接字符串

本主题介绍如何定义在连接到概念模型时使用的连接字符串。 本主题基于 [AdventureWorks 销售](/previous-versions/dotnet/netframework-4.0/bb387147(v=vs.100)) 概念模型。 AdventureWorks 销售模型在实体框架文档中的与任务相关的主题中使用。 本主题假定您已经配置了实体框架并定义了 AdventureWorks 销售模型。 有关详细信息，请参阅 [如何：手动定义模型和映射文件](/previous-versions/dotnet/netframework-4.0/bb399785(v=vs.100))。 本主题中的过程也包括在 [如何：手动配置实体框架项目](/previous-versions/dotnet/netframework-4.0/bb738546(v=vs.100))中。

> [!NOTE]
> 如果你在 Visual Studio 项目中使用实体数据模型向导，它会自动生成 .edmx 文件并将该项目配置为使用实体框架。 有关详细信息，请参阅 [如何：使用实体数据模型向导](/previous-versions/dotnet/netframework-4.0/bb738677(v=vs.100))。

## <a name="to-define-the-entity-framework-connection-string"></a>定义实体框架连接字符串

- 打开项目的应用程序配置文件 (app.config) 并添加以下连接字符串：

```xml
<connectionStrings>
    <add name="AdventureWorksEntities"
         connectionString="metadata=.\AdventureWorks.csdl|.\AdventureWorks.ssdl|.\AdventureWorks.msl;
         provider=System.Data.SqlClient;provider connection string='Data Source=localhost;
         Initial Catalog=AdventureWorks;Integrated Security=True;Connection Timeout=60;
         multipleactiveresultsets=true'" providerName="System.Data.EntityClient" />
</connectionStrings>
```

如果你的项目没有应用程序配置文件，则可以通过从 "**项目**" 菜单中选择 "**添加新项**"，选择 "**常规**" 类别，选择 "**应用程序配置文件**"，然后单击 "**添加**" 来添加一个。

## <a name="see-also"></a>请参阅

- [快速入门](/previous-versions/dotnet/netframework-4.0/bb399182(v=vs.100))
- [如何：创建新的 .edmx 文件](/previous-versions/dotnet/netframework-4.0/cc716703(v=vs.100))
- [ADO.NET 实体数据模型工具](/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100))
