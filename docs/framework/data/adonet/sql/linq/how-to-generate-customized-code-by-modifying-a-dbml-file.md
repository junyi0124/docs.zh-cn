---
title: 如何：通过修改 DBML 文件生成自定义代码
ms.date: 03/30/2017
ms.assetid: 50ad597a-8598-42d3-82dd-fc7d702ebc37
ms.openlocfilehash: ab49f76a0d5e7338a93e21ae9a8d1d9d74a21e82
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173433"
---
# <a name="how-to-generate-customized-code-by-modifying-a-dbml-file"></a>如何：通过修改 DBML 文件生成自定义代码

您可以 ( .dbml) 元数据文件生成 Visual Basic 或 c # 源代码。 此方法提供了一个在生成应用程序映射代码前自定义默认 .dbml 文件的机会。 这是一项高级功能。  
  
 此过程中的步骤如下：  
  
1. 生成 .dbml 文件。  
  
2. 使用编辑器修改此 .dbml 文件。 请注意，此 .dbml 文件必须通过 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] .dbml 文件的架构定义 (.xsd) 文件的验证。 有关详细信息，请参阅 [LINQ to SQL 中的代码生成](code-generation-in-linq-to-sql.md)。  
  
3. 生成 Visual Basic 或 c # 源代码。  
  
 下面的示例使用 SQLMetal 命令行工具。 有关详细信息，请参阅 [SqlMetal.exe（代码生成工具）](../../../../tools/sqlmetal-exe-code-generation-tool.md)。  
  
## <a name="example"></a>示例  

 下面的代码从 Northwind 示例数据库生成 .dbml 文件。 您可以使用数据库的名称或 .mdf 文件的名称作为数据库元数据的源。  
  
```console  
sqlmetal /server:myserver /database:northwind /dbml:mymeta.dbml  
sqlmetal /dbml:mymeta.dbml mydbfile.mdf  
```  
  
## <a name="example"></a>示例  

 下面的代码从 .dbml 文件生成 Visual Basic 或 c # 源代码文件。  
  
```console
sqlmetal /namespace:nwind /code:nwind.vb /language:vb DBMLFile.dbml  
sqlmetal /namespace:nwind /code:nwind.cs /language:csharp DBMLFile.dbml  
```  
  
## <a name="see-also"></a>请参阅

- [LINQ to SQL 中的代码生成](code-generation-in-linq-to-sql.md)
- [SqlMetal.exe（代码生成工具）](../../../../tools/sqlmetal-exe-code-generation-tool.md)
- [创建对象模型](creating-the-object-model.md)
