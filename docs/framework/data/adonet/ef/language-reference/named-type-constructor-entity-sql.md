---
title: 命名类型构造函数 (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 549dea04-d93d-4c87-a292-f81b1598dbfd
ms.openlocfilehash: c673b58ee5811e3d3b74b3744d3f5291888e2253
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91197776"
---
# <a name="named-type-constructor-entity-sql"></a>命名类型构造函数 (Entity SQL)

用于创建概念模型名义类型（如实体或复杂类型）的实例。  
  
## <a name="syntax"></a>语法  
  
```sql  
[{identifier. }] identifier( [expression [{, expression }]] )  
```  
  
## <a name="arguments"></a>参数  

 `identifier`  
 作为简单标识符或带引号的标识符的值。 有关详细信息，请参阅 [标识符](identifiers-entity-sql.md)  
  
 `expression`  
 类型的属性，假设这些属性的顺序与它们在类型声明中的顺序相同。  
  
## <a name="return-value"></a>返回值  

 命名复杂类型和实体类型的实例。  
  
## <a name="remarks"></a>备注  

 下面的示例演示如何构造名义类型和复杂类型：  
  
 下面的表达式创建 `Person` 类型的实例：  
  
 `Person("abc", 12)`  
  
 下面的表达式创建复杂类型的实例：  
  
 `MyModel.ZipCode(‘98118’, ‘4567’)`  
  
 下面的表达式创建嵌套复杂类型的实例：  
  
 `MyModel.AddressInfo('My street address', 'Seattle', 'WA', MyModel.ZipCode('98118', '4567'))`  
  
 下面的表达式创建具有嵌套复杂类型的实体的实例：  
  
 `MyModel.Person("Bill", MyModel.AddressInfo('My street address', 'Seattle', 'WA', MyModel.ZipCode('98118', '4567')))`  
  
 下面的示例演示如何将复杂类型的属性初始化为 null：`MyModel.ZipCode(‘98118’, null)`  
  
## <a name="example"></a>示例  

 下面的 Entity SQL 查询使用命名类型构造函数创建概念模型类型的实例。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#NAMED_TYPE_CONSTRUCTOR](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#named_type_constructor)]  
  
## <a name="see-also"></a>请参阅

- [构造类型](constructing-types-entity-sql.md)
- [实体 SQL 引用](entity-sql-reference.md)
