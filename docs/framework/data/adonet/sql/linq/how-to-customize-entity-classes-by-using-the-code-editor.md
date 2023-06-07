---
title: 如何：通过使用代码编辑器自定义实体类
ms.date: 03/30/2017
ms.assetid: ec28332f-9f3c-4e0a-baca-60f9141a68c0
ms.openlocfilehash: 5e61acc9de1ef2f00d5e81a3c3080a9dc46f074d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91147692"
---
# <a name="how-to-customize-entity-classes-by-using-the-code-editor"></a>如何：通过使用代码编辑器自定义实体类

使用 Visual Studio 的开发人员可以使用对象关系设计器来创建或自定义其实体类。  
  
 你还可以使用 Visual Studio 代码编辑器编写你自己的映射代码或自定义已生成的代码。 有关详细信息，请参阅 [基于属性的映射](attribute-based-mapping.md)。  
  
 本节中的主题说明如何自定义对象模型。  
  
 [如何：指定数据库名称](how-to-specify-database-names.md)  
 描述如何使用 <xref:System.Data.Linq.Mapping.DatabaseAttribute.Name%2A>。  
  
 [如何：将表表示为类](how-to-represent-tables-as-classes.md)  
 描述如何使用 <xref:System.Data.Linq.Mapping.TableAttribute>。  
  
 [如何：将列表示为类成员](how-to-represent-columns-as-class-members.md)  
 描述如何使用 <xref:System.Data.Linq.Mapping.ColumnAttribute>。  
  
 [如何：表示主键](how-to-represent-primary-keys.md)  
 描述如何使用 <xref:System.Data.Linq.Mapping.ColumnAttribute.IsPrimaryKey%2A>。  
  
 [如何：映射数据库关系](how-to-map-database-relationships.md)  
 提供使用 <xref:System.Data.Linq.Mapping.AssociationAttribute> 属性的示例。  
  
 [如何：将列表示为数据库生成的](how-to-represent-columns-as-database-generated.md)  
 描述如何使用 <xref:System.Data.Linq.Mapping.ColumnAttribute.IsDbGenerated%2A>。  
  
 [如何：将列表示为时间戳列或版本列](how-to-represent-columns-as-timestamp-or-version-columns.md)  
 描述如何使用 <xref:System.Data.Linq.Mapping.ColumnAttribute.IsVersion%2A>。  
  
 [如何：指定数据库数据类型](how-to-specify-database-data-types.md)  
 描述如何使用 <xref:System.Data.Linq.Mapping.ColumnAttribute.DbType%2A>。  
  
 [如何：表示计算所得的列](how-to-represent-computed-columns.md)  
 描述如何使用 <xref:System.Data.Linq.Mapping.ColumnAttribute.Expression%2A>。  
  
 [如何：指定专用存储字段](how-to-specify-private-storage-fields.md)  
 描述如何使用 <xref:System.Data.Linq.Mapping.DataAttribute.Storage%2A>。  
  
 [如何：将列表示为允许 Null 值](how-to-represent-columns-as-allowing-null-values.md)  
 描述如何使用 <xref:System.Data.Linq.Mapping.ColumnAttribute.CanBeNull%2A>。  
  
 [如何：映射继承层次结构](how-to-map-inheritance-hierarchies.md)  
 说明指定继承层次结构所需的映射。  
  
 [如何：指定并发冲突检查](how-to-specify-concurrency-conflict-checking.md)  
 描述如何使用 <xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A>。  
  
## <a name="see-also"></a>请参阅

- [SqlMetal.exe（代码生成工具）](../../../../tools/sqlmetal-exe-code-generation-tool.md)
