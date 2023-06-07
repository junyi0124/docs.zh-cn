---
title: 如何：使用 EntityCommand 执行参数化存储过程
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 4f5639bf-bb7f-4982-bb1d-c7caa4348888
ms.openlocfilehash: ec1ff7cdbdc83bc409b191f0aefe2b50cbad9225
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91192160"
---
# <a name="how-to-execute-a-parameterized-stored-procedure-using-entitycommand"></a>如何：使用 EntityCommand 执行参数化存储过程

本主题说明如何使用 <xref:System.Data.EntityClient.EntityCommand> 类执行参数化存储过程。  
  
### <a name="to-run-the-code-in-this-example"></a>运行本示例中的代码  
  
1. 将 [School 模型](/previous-versions/dotnet/netframework-4.0/bb896300(v=vs.100)) 添加到项目中，并将项目配置为使用实体框架。 有关详细信息，请参阅 [如何：使用实体数据模型向导](/previous-versions/dotnet/netframework-4.0/bb738677(v=vs.100))。  
  
2. 在应用程序的代码页中，添加以下 `using` 语句（在 Visual Basic 中为 `Imports`）：  
  
     [!code-csharp[DP EntityServices Concepts#Namespaces](../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts/cs/source.cs#namespaces)]
     [!code-vb[DP EntityServices Concepts#Namespaces](../../../../../samples/snippets/visualbasic/VS_Snippets_Data/dp entityservices concepts/vb/source.vb#namespaces)]  
  
3. 导入 `GetStudentGrades` 存储过程并将 `CourseGrade` 实体指定为返回类型。 有关如何导入存储过程的信息，请参阅 [如何：导入存储过程](/previous-versions/dotnet/netframework-4.0/bb896231(v=vs.100))。  
  
## <a name="example"></a>示例  

 下面的代码执行 `GetStudentGrades` 存储过程，其中，`StudentId` 为必需的参数。 然后由 <xref:System.Data.EntityClient.EntityDataReader> 读取结果。  
  
 [!code-csharp[DP EntityServices Concepts#StoredProcWithEntityCommand](../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts/cs/source.cs#storedprocwithentitycommand)]
 [!code-vb[DP EntityServices Concepts#StoredProcWithEntityCommand](../../../../../samples/snippets/visualbasic/VS_Snippets_Data/dp entityservices concepts/vb/source.vb#storedprocwithentitycommand)]  
  
## <a name="see-also"></a>请参阅

- [用于实体框架的 EntityClient 提供程序](entityclient-provider-for-the-entity-framework.md)
