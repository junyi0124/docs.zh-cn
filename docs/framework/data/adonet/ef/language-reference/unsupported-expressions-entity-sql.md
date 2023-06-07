---
title: 不支持的表达式 (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 5e79da7e-e78a-413c-8fb0-f3f9cd84f579
dev_langs:
- sql
ms.openlocfilehash: 956fe117eb0c59392c3999046bc70deaed268ac6
ms.sourcegitcommit: 4e2d355baba82814fa53efd6b8bbb45bfe054d11
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2019
ms.locfileid: "70248784"
---
# <a name="unsupported-expressions"></a>不支持的表达式

本主题介绍中[!INCLUDE[esql](../../../../../../includes/esql-md.md)]不支持的 transact-sql 表达式。 有关详细信息，请参阅[实体 SQL 与 transact-sql 的不同之处](how-entity-sql-differs-from-transact-sql.md)。

## <a name="quantified-predicates"></a>量化谓词

Transact-sql 允许构造以下形式：

```sql
sal > all (select salary from employees)
sal > any (select salary from employees)
```

但 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 不支持此这样的构造。 在 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 中的等效表达式可以写为如下形式：

```sql
not exists(select 0 from employees as e where sal <= e.salary)
exists(select 0 from employees as e where sal > e.salary)
```

## <a name="-operator"></a>* 运算符

Transact-sql 支持在 SELECT 子句中使用 * 运算符，以指示应提取所有列。在 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 中不支持使用此运算符。

## <a name="see-also"></a>请参阅

- [实体 SQL 概述](entity-sql-overview.md)
- [实体 SQL 与 Transact-SQL 的区别](how-entity-sql-differs-from-transact-sql.md)
