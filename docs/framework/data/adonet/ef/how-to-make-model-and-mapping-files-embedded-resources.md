---
title: 如何：创建模型及映射文件嵌入资源
ms.date: 03/30/2017
ms.assetid: 20dfae4d-e95a-4264-9540-f5ad23b462d3
ms.openlocfilehash: 8496dcad5422d1a45af52e58325efd360768da34
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91198283"
---
# <a name="how-to-make-model-and-mapping-files-embedded-resources"></a>如何：创建模型及映射文件嵌入资源

使用实体框架可以将模型和映射文件部署为应用程序的嵌入资源。 包含嵌入模型和映射文件的程序集必须加载到实体连接所在的应用程序域中。 有关详细信息，请参阅 [连接字符串](connection-strings.md)。 默认情况下，实体数据模型工具嵌入模型和映射文件。 手动定义模型和映射文件时，请使用此过程来确保将文件与实体框架的应用程序一起部署到嵌入的资源。  
  
> [!NOTE]
> 若要维护嵌入资源，每次修改模型和映射文件时都必须重复此过程。  
  
### <a name="to-embed-model-and-mapping-files"></a>嵌入模型和映射文件  
  
1. 在 **解决方案资源管理器**中，选择概念 ( ") 文件。  
  
2. 在 " **属性** " 窗格中，将 " **生成操作** " 设置为 " **嵌入资源**"。  
  
3. 对存储文件 (.ssdl) 和映射文件 (.msl) 重复步骤 1 和步骤 2。  
  
4. 在 **解决方案资源管理器**中，双击 App.config 文件，然后根据 `Metadata` `connectionString` 以下格式之一修改属性中的参数：  
  
    - `Metadata=` `res://<assemblyFullName>/<resourceName>;`  
  
    - `Metadata=` `res://*/<resourceName>;`  
  
    - `Metadata=res://*;`  
  
     有关详细信息，请参阅 [连接字符串](connection-strings.md)。  
  
## <a name="example"></a>示例  

 下面的连接字符串引用 [AdventureWorks 销售模型](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)的嵌入模型和映射文件。 该连接字符串存储在项目的 App.config 文件中。  

## <a name="see-also"></a>请参阅

- [建模和映射](modeling-and-mapping.md)
- [如何：定义连接字符串](how-to-define-the-connection-string.md)
- [如何：生成 EntityConnection 连接字符串](how-to-build-an-entityconnection-connection-string.md)
- [ADO.NET 实体数据模型工具](/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100))
