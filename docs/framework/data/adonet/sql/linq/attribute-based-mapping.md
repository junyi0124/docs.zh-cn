---
title: 基于特性的映射
ms.date: 03/30/2017
ms.assetid: 6dd89999-f415-4d61-b8c8-237d23d7924e
ms.openlocfilehash: 986a5022ea9e70868689c898649067135eac944b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156103"
---
# <a name="attribute-based-mapping"></a>基于特性的映射

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)][!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]通过应用特性或使用外部映射文件，将 SQL Server 数据库映射到对象模型。 本主题概述了基于属性的方法。  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 最基本的映射形式是将数据库映射到 <xref:System.Data.Linq.DataContext>，将表映射到类，将列和关系映射到这些类的属性。 您也可以使用属性来将继承层次结构映射到对象模型中。 有关详细信息，请参阅 [如何：在 Visual Basic 或 c # 中生成对象模型](how-to-generate-the-object-model-in-visual-basic-or-csharp.md)。  
  
 使用 Visual Studio 的开发人员通常使用对象关系设计器执行基于属性的映射。 也可以使用 SQLMetal 命令行工具，或亲自手动对属性进行编码。 有关详细信息，请参阅 [如何：在 Visual Basic 或 c # 中生成对象模型](how-to-generate-the-object-model-in-visual-basic-or-csharp.md)。  
  
> [!NOTE]
> 您还可以通过使用外部 XML 文件进行映射。 有关详细信息，请参阅 [外部映射](external-mapping.md)。  
  
 以下各节更加详细地介绍了基于属性的映射。 有关更多信息，请参见 <xref:System.Data.Linq.Mapping> 命名空间。  
  
## <a name="databaseattribute-attribute"></a>DatabaseAttribute 属性  

 使用此属性可指定在连接未提供名称时数据库的默认名称。 此属性 (Attribute) 是可选的，但如果使用它，则必须按照下表中的说明应用 <xref:System.Data.Linq.Mapping.DatabaseAttribute.Name%2A> 属性 (Property)。  
  
|属性|类型|默认|说明|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.DatabaseAttribute.Name%2A>|字符串|请参见<xref:System.Data.Linq.Mapping.DatabaseAttribute.Name%2A>|与其 <xref:System.Data.Linq.Mapping.DatabaseAttribute.Name%2A> 属性一起使用，用于指定数据库的名称。|  
  
 有关详细信息，请参阅 <xref:System.Data.Linq.Mapping.DatabaseAttribute>。  
  
## <a name="tableattribute-attribute"></a>TableAttribute 属性  

 使用此属性可将类指定为与数据库表或视图关联的实体类。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 将具有此属性的类视为持久性类。 下表介绍了 <xref:System.Data.Linq.Mapping.TableAttribute.Name%2A> 属性。  
  
|属性|类型|默认|说明|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.TableAttribute.Name%2A>|字符串|与类名相同的字符串|将类指定为与数据库表关联的实体类。|  
  
 有关详细信息，请参阅 <xref:System.Data.Linq.Mapping.TableAttribute>。  
  
## <a name="columnattribute-attribute"></a>ColumnAttribute 属性  

 使用此属性可指定实体类的某个成员表示数据库表中的列。 您可以将此属性 (Attribute) 应用于任何字段或属性 (Property)。  
  
 当 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 保存对数据库所做的更改时，只会检索并持久保存您标识为列的那些成员。 不具有此属性的成员被假定为非持久的，且不会被提交以进行插入或更新。  
  
 下表介绍了此属性 (Attribute) 的属性 (Property)。  
  
|属性|类型|默认|描述|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.AutoSync%2A>|AutoSync|从不|指示公共语言运行库 (CLR) 在执行插入或更新操作后检索值。<br /><br /> 可供选择的值：Always、Never、OnUpdate、OnInsert。|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.CanBeNull%2A>|Boolean|`true`|指示列可以包含 null 值。|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.DbType%2A>|字符串|推断出的数据库列类型|使用数据库类型和修饰符来指定数据库列的类型。|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.Expression%2A>|字符串|空|定义数据库中计算所得的列。|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.IsDbGenerated%2A>|Boolean|`false`|指示列包含数据库自动生成的值。|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.IsDiscriminator%2A>|Boolean|`false`|指示列包含 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 继承层次结构的鉴别器值。|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.IsPrimaryKey%2A>|Boolean|`false`|指定此类成员表示作为表主键或表主键一部分的列。|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.IsVersion%2A>|Boolean|`false`|将成员的列类型标识为数据库时间戳或版本号。|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A>|UpdateCheck|除非对某个成员而言 `Always` 为 <xref:System.Data.Linq.Mapping.ColumnAttribute.IsVersion%2A>，否则为 `true`|指定 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 如何实现开放式并发冲突的检测。|  
  
 有关详细信息，请参阅 <xref:System.Data.Linq.Mapping.ColumnAttribute>。  
  
> [!NOTE]
> AssociationAttribute 和 ColumnAttribute Storage 属性值区分大小写。 例如，请确保 AssociationAttribute.Storage 属性 (Property) 的属性 (Attribute) 中使用的值与代码中其他位置使用的相应属性 (Property) 名称值的大小写相匹配。 这适用于所有 .NET 编程语言，甚至不是通常区分大小写的语言，包括 Visual Basic。 有关 Storage 属性的更多信息，请参见 <xref:System.Data.Linq.Mapping.DataAttribute.Storage%2A?displayProperty=nameWithType>。  
  
## <a name="associationattribute-attribute"></a>AssociationAttribute 属性  

 使用此属性 (Attribute) 可指定属性 (Property) 表示数据库中的关联，如外键对主键关系。 有关关系的详细信息，请参阅 [如何：映射数据库关系](how-to-map-database-relationships.md)。  
  
 下表介绍了此属性 (Attribute) 的属性 (Property)。  
  
|属性|类型|默认|描述|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.AssociationAttribute.DeleteOnNull%2A>|Boolean|`false`|当放置在其外键成员均不可以为 null 的关联上时，如果该关联设置为 null，则删除对象。|  
|<xref:System.Data.Linq.Mapping.AssociationAttribute.DeleteRule%2A>|字符串|无|向关联添加删除行为。|  
|<xref:System.Data.Linq.Mapping.AssociationAttribute.IsForeignKey%2A>|Boolean|`false`|如果为 true，则将成员指定为表示数据库关系的关联中的外键。|  
|<xref:System.Data.Linq.Mapping.AssociationAttribute.IsUnique%2A>|Boolean|`false`|如果为 true，则指示对外键的唯一性约束。|  
|<xref:System.Data.Linq.Mapping.AssociationAttribute.OtherKey%2A>|字符串|相关类的 ID|将目标实体类的一个或多个成员指定为关联的另一端上的键值。|  
|<xref:System.Data.Linq.Mapping.AssociationAttribute.ThisKey%2A>|字符串|包含类的 ID|指定此实体类的成员表示关联的此端上的键值。|  
  
 有关详细信息，请参阅 <xref:System.Data.Linq.Mapping.AssociationAttribute>。  
  
> [!NOTE]
> AssociationAttribute 和 ColumnAttribute Storage 属性值区分大小写。 例如，请确保 AssociationAttribute.Storage 属性 (Property) 的属性 (Attribute) 中使用的值与代码中其他位置使用的相应属性 (Property) 名称值的大小写相匹配。 这适用于所有 .NET 编程语言，甚至不是通常区分大小写的语言，包括 Visual Basic。 有关 Storage 属性的更多信息，请参见 <xref:System.Data.Linq.Mapping.DataAttribute.Storage%2A?displayProperty=nameWithType>。  
  
## <a name="inheritancemappingattribute-attribute"></a>InheritanceMappingAttribute 属性  

 使用此属性可映射继承层次结构。  
  
 下表介绍了此属性 (Attribute) 的属性 (Property)。  
  
|属性|类型|默认|说明|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.InheritanceMappingAttribute.Code%2A>|字符串|无。 必须提供值。|指定鉴别器的代码值。|  
|<xref:System.Data.Linq.Mapping.InheritanceMappingAttribute.IsDefault%2A>|Boolean|`false`|如果为 true，则在存储区中没有与指定值中的任何一个值匹配的鉴别器值时实例化此类型的对象。|  
|<xref:System.Data.Linq.Mapping.InheritanceMappingAttribute.Type%2A>|类型|无。 必须提供值。|指定层次结构中的类的类型。|  
  
 有关详细信息，请参阅 <xref:System.Data.Linq.Mapping.InheritanceMappingAttribute>。  
  
## <a name="functionattribute-attribute"></a>FunctionAttribute 属性  

 使用此属性可指定方法表示数据库中的存储过程或用户定义函数。  
  
 下表介绍了此属性 (Attribute) 的属性 (Property)。  
  
|属性|类型|默认|描述|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.FunctionAttribute.IsComposable%2A>|Boolean|`false`|如果为 false，则指示映射到存储过程。 如果为 true，则指示映射到用户定义的函数。|  
|<xref:System.Data.Linq.Mapping.FunctionAttribute.Name%2A>|字符串|与数据库中的名称相同的字符串|指定存储过程或用户定义函数的名称。|  
  
 有关详细信息，请参阅 <xref:System.Data.Linq.Mapping.FunctionAttribute>。  
  
## <a name="parameterattribute-attribute"></a>ParameterAttribute 属性  

 使用此属性可映射存储过程方法中的输入参数。  
  
 下表介绍了此属性 (Attribute) 的属性 (Property)。  
  
|属性|类型|默认|说明|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.ParameterAttribute.DbType%2A>|字符串|无|指定数据库类型。|  
|<xref:System.Data.Linq.Mapping.ParameterAttribute.Name%2A>|字符串|与数据库中的参数名相同的字符串|指定参数的名称。|  
  
 有关详细信息，请参阅 <xref:System.Data.Linq.Mapping.ParameterAttribute>。  
  
## <a name="resulttypeattribute-attribute"></a>ResultTypeAttribute 属性  

 使用此属性可指定结果类型。  
  
 下表介绍了此属性 (Attribute) 的属性 (Property)。  
  
|属性|类型|默认|描述|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.ResultTypeAttribute.Type%2A>|类型|（无）|用于映射到返回 <xref:System.Data.Linq.IMultipleResults> 的存储过程的方法。 为存储过程声明有效的或预期的类型映射。|  
  
 有关详细信息，请参阅 <xref:System.Data.Linq.Mapping.ResultTypeAttribute>。  
  
## <a name="dataattribute-attribute"></a>DataAttribute 属性  

 使用此属性可指定名称和私有存储字段。  
  
 下表介绍了此属性 (Attribute) 的属性 (Property)。  
  
|属性|类型|默认|说明|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.DataAttribute.Name%2A>|字符串|与数据库中的名称相同|指定表、列等的名称。|  
|<xref:System.Data.Linq.Mapping.DataAttribute.Storage%2A>|字符串|公共访问器|指定基础存储字段的名称。|  
  
 有关详细信息，请参阅 <xref:System.Data.Linq.Mapping.DataAttribute>。  
  
## <a name="see-also"></a>请参阅

- [引用](reference.md)
