---
title: TREAT (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 5b77f156-55de-4cb4-8154-87f707d4c635
ms.openlocfilehash: bb41c0fed944ce4db11878b9213a62c6f851418e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201039"
---
# <a name="treat-entity-sql"></a>TREAT (Entity SQL)

将特定基类型的对象视为指定派生类型的对象。  
  
## <a name="syntax"></a>语法  
  
```sql  
TREAT ( expression as type)  
```  
  
## <a name="arguments"></a>参数  

 `expression`  
 任何返回实体的有效查询表达式。  
  
> [!NOTE]
> 指定表达式的类型必须为指定数据类型的子类型，或者该数据类型必须为表达式的类型的子类型。  
  
 `type`  
 一个实体类型。 该类型必须由命名空间进行限定。  
  
> [!NOTE]
> 指定表达式必须为指定数据类型的子类型，或者该数据类型必须为该表达式的子类型。  
  
## <a name="return-value"></a>返回值  

 一个具有指定数据类型的值。  
  
## <a name="remarks"></a>备注  

 TREAT 用于在相关类之间执行向上转换。 例如，如果 `Employee` 派生自 `Person` 且 p 的类型为 `Person`，则 `TREAT(p AS NamespaceName.Employee)` 会将泛型 `Person` 实例向上转换为 `Employee`；即，使您可以将 p 视为 `Employee`。  
  
 TREAT 用于可以执行类似于以下查询的继承方案：  
  
```sql  
SELECT TREAT(p AS NamespaceName.Employee)  
FROM ContainerName.Person AS p  
WHERE p IS OF (NamespaceName.Employee)
```  
  
 此查询将 `Person` 实体向上转换为 `Employee` 类型。 如果 p 的值的实际类型不是 `Employee`，则表达式会生成值 `null`。  
  
> [!NOTE]
> 指定的表达式 `Employee` 必须为指定数据类型的子类型 `Person` ，或者该数据类型必须为该表达式的子类型。 否则，表达式会导致编译时错误。  
  
 下表显示了 TREAT 在某些典型模式和非常见模式下的行为。 所有异常都在调用提供程序之前从客户端引发：  
  
|模式|行为|  
|-------------|--------------|  
|`TREAT (null AS EntityType)`|返回 `DbNull`。|  
|`TREAT (null AS ComplexType)`|引发异常。|  
|`TREAT (null AS RowType)`|引发异常。|  
|`TREAT (EntityType AS EntityType)`|返回 `EntityType` 或 `null`。|  
|`TREAT (ComplexType AS ComplexType)`|引发异常。|  
|`TREAT (RowType AS RowType)`|引发异常。|  
  
## <a name="example"></a>示例  

 下面的 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 查询使用 TREAT 运算符将类型为 Course 的对象转换为类型为 OnsiteCourse 的对象的集合。 该查询基于 [School 模型](/previous-versions/dotnet/netframework-4.0/bb896300(v=vs.100))。  
  
 [!code-sql[DP EntityServices Concepts#TREAT_ISOF](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#treat_isof)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [可以为 NULL 的结构化类型](nullable-structured-types-entity-sql.md)
