---
title: 构造类型 (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 41fa7bde-8d20-4a3f-a3d2-fb791e128010
ms.openlocfilehash: 82c8e3f2bac0d13da4870e90878e0de6fc9ec063
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177516"
---
# <a name="constructing-types-entity-sql"></a>构造类型 (Entity SQL)

[!INCLUDE[esql](../../../../../../includes/esql-md.md)] 提供了三种构造函数：行构造函数、命名类型构造函数和集合构造函数。  
  
## <a name="row-constructors"></a>行构造函数  

 使用 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 中的行构造函数可以从一个或多个值构造结构上类型化的匿名记录。 行构造函数的结果类型为行类型，其字段类型对应于用于构造该行的值的类型。 例如，下面的表达式构造一个类型为的值 `Record(a int, b string, c int)` ：  
  
 `ROW(1 AS a, "abc" AS b, a + 34 AS c)`  
  
 如果没有为行构造函数中的表达式提供别名，则实体框架会尝试生成一个别名。 有关详细信息，请参阅 [标识符](identifiers-entity-sql.md)中的 "别名规则" 一节。  
  
 以下规则适用于在行构造函数中指定表达式别名：  
  
- 行构造函数中的表达式不能引用同一构造函数中的其他别名。  
  
- 同一行构造函数中的两个表达式不能具有相同别名。  
  
 有关行构造函数的详细信息，请参阅 [row](row-entity-sql.md)。  
  
## <a name="collection-constructors"></a>集合构造函数  

 使用 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 中的集合构造函数可以从值列表创建多重集实例。 该构造函数中的所有值都必须为互兼容的 `T` 类型，并且该构造函数生成 `Multiset<T>` 类型的集合。 例如，下面的表达式创建整数集合：  
  
 `Multiset(1, 2, 3)`  
  
 `{1, 2, 3}`  
  
 因为无法确定元素类型，所以不能使用空的多重集构造函数。 下面的表达式无效：  
  
 `multiset() {}`  
  
 有关详细信息，请参阅 [多重集](multiset-entity-sql.md)。  
  
## <a name="named-type-constructors-namedtype-initializers"></a>命名类型构造函数（NamedType 初始值设定项）  

 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 允许类型构造函数（初始值设定项）创建命名复杂类型和命名实体类型的实例。 例如，下面的表达式创建 `Person` 类型的实例。  
  
 `Person("abc", 12)`  
  
 下面的表达式创建复杂类型的实例。  
  
 `MyModel.ZipCode(‘98118’, ‘4567’)`  
  
 下面的表达式创建嵌套复杂类型的实例。  
  
 `MyModel.AddressInfo('My street address', 'Seattle', 'WA', MyModel.ZipCode('98118', '4567'))`  
  
 下面的表达式创建具有嵌套复杂类型的实体的实例。  
  
 `MyModel.Person("Bill", MyModel.AddressInfo('My street address', 'Seattle', 'WA', MyModel.ZipCode('98118', '4567')))`  
  
 下面的示例演示如何将复杂类型的属性初始化为 null。 `MyModel.ZipCode(‘98118’, null)`  
  
 假定该构造函数的参数的顺序与该类型的属性的声明顺序相同。  
  
 有关详细信息，请参阅 [命名类型构造函数](named-type-constructor-entity-sql.md)。  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [Entity SQL 概述](entity-sql-overview.md)
- [类型系统](type-system-entity-sql.md)
