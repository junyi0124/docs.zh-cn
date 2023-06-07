---
title: 排序规则
ms.date: 12/13/2019
description: 了解如何创建自定义排序序列。
ms.openlocfilehash: 9879846cc191a62c4cb47a0fbaa47c59153ba61c
ms.sourcegitcommit: 7980a91f90ae5eca859db7e6bfa03e23e76a1a50
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "81242967"
---
# <a name="collation"></a>排序规则

SQLite 在比较 TEXT 值时使用排序序列确定顺序和相等性。 可以指定在 SQL 查询中创建列时或是对每个操作要使用的排序规则。 SQLite 在默认情况下包含三种排序序列。

| 排序规则 | 描述                               |
| --------- | ----------------------------------------- |
| RTRIM     | 忽略尾随空格               |
| NOCASE    | 对于 ASCII 字符 A-Z 不区分大小写 |
| BINARY    | 区分大小写。 直接比较字节   |

## <a name="custom-collation"></a>自定义排序规则

还可以使用 <xref:Microsoft.Data.Sqlite.SqliteConnection.CreateCollation%2A> 定义自己的排序序列或替代内置排序序列。 下面的示例演示如何替代 NOCASE 排序规则以支持 Unicode 字符。 GitHub 上提供了[完整示例代码](https://github.com/dotnet/docs/blob/master/samples/snippets/standard/data/sqlite/CollationSample/Program.cs)。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/CollationSample/Program.cs?name=snippet_Collation)]

## <a name="like-operator"></a>Like 运算符

SQLite 中的 LIKE 运算符不遵循排序规则。 默认实现的语义与 NOCASE 排序规则相同。 只对 ASCII 字符 A 到 Z 不区分大小写。

可以使用以下 pragma 语句轻松地使 LIKE 运算符区分大小写：

```sql
PRAGMA case_sensitive_like = 1
```

有关替代 LIKE 运算符的实现的详细信息，请参阅[用户定义的函数](user-defined-functions.md)。

## <a name="see-also"></a>请参阅

* [用户定义的函数](user-defined-functions.md)
* [SQL 语法](https://www.sqlite.org/lang.html)
