---
title: 使用数据定义语言
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: ec50083d-44f4-4093-9b23-5eacd601f96e
ms.openlocfilehash: c30cbbc1eae6d4cbcadb9bfe8d267fb428764971
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200883"
---
# <a name="working-with-data-definition-language"></a>使用数据定义语言

从 .NET Framework 版本4开始，实体框架支持 (DDL) 的数据定义语言。 这样，您将能够基于连接字符串和存储元数据 (SSDL) 模型创建或删除数据库实例。  
  
 <xref:System.Data.Objects.ObjectContext> 的以下方法使用连接字符串和 SSDL 内容来完成以下操作：创建或删除数据库，检查数据库是否存在，以及查看生成的 DDL 脚本：  
  
- <xref:System.Data.Objects.ObjectContext.CreateDatabase%2A>  
  
- <xref:System.Data.Objects.ObjectContext.DeleteDatabase%2A>  
  
- <xref:System.Data.Objects.ObjectContext.DatabaseExists%2A>  
  
- <xref:System.Data.Objects.ObjectContext.CreateDatabaseScript%2A>  
  
> [!NOTE]
> 假定有足够的权限可执行 DDL 命令。  
  
 以上列出的方法将大部分工作都委托给基础 ADO.NET 数据提供程序。 该提供程序负责确保用于生成数据库对象的命名约定与用于查询和更新的约定保持一致。  
  
 下面的示例演示如何基于现有模型生成数据库。 它还将新的实体对象添加到对象上下文中，然后将该对象保存到数据库中。  
  
## <a name="procedures"></a>过程  
  
### <a name="to-define-a-database-based-on-the-existing-model"></a>基于现有模型定义数据库  
  
1. 创建控制台应用程序。  
  
2. 向应用程序中添加现有模型。  
  
    1. 添加一个名为的空模型 `SchoolModel` 。 若要创建空模型，请参阅 [如何：创建新的 .Edmx 文件](/previous-versions/dotnet/netframework-4.0/cc716703(v=vs.100)) 主题。  
  
     将 SchoolModel.edmx 文件添加到您的项目中。  
  
    1. 从 [School 模型](/previous-versions/dotnet/netframework-4.0/bb896300(v=vs.100)) 主题复制 school 模型的概念、存储和映射内容。  
  
    2. 打开 SchoolModel.edmx 文件并将内容粘贴在 `edmx:Runtime` 标记中。  
  
3. 将以下代码添加到主函数中。 此类代码将连接字符串初始化为数据库服务器，查看 DDL 脚本，创建数据库，将新实体添加到上下文，并将更改保存到数据库。  
  
     [!code-csharp[DP ObjectServices Concepts#DDL](../../../../../samples/snippets/csharp/VS_Snippets_Data/DP ObjectServices Concepts/CS/Source.cs#ddl)]
     [!code-vb[DP ObjectServices Concepts#DDL](../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP ObjectServices Concepts/VB/Source.vb#ddl)]
