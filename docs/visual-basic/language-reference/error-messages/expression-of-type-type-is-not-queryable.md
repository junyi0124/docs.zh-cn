---
title: 类型 <type> 的表达式不可查询
ms.date: 07/20/2015
f1_keywords:
- bc36593
- vbc36593
helpviewer_keywords:
- BC36593
ms.assetid: 6f1f5860-bf97-4885-9ebb-bc87d028095c
ms.openlocfilehash: d605243c213166f20592fdc440a3362f957ebbf2
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163059"
---
# <a name="bc36593-expression-of-type-type-is-not-queryable"></a>BC36593：类型的表达式 \<type> 不可查询

类型的表达式 \<type> 不可查询。 请确保不缺少 LINQ 提供程序的程序集引用和/或命名空间导入。

 可查询类型在 <xref:System.Linq> 、 <xref:System.Data.Linq> 和 <xref:System.Xml.Linq> 命名空间中定义。 必须导入一个或多个此类命名空间才能执行 LINQ 查询。

 <xref:System.Linq>命名空间使你能够通过使用 LINQ 查询对象（如集合和数组）。

 <xref:System.Data.Linq>命名空间使你能够通过使用 LINQ 查询 ADO.NET 数据集和 SQL Server 数据库。

 <xref:System.Xml.Linq>命名空间使你能够使用 LINQ 查询 xml，并使用 Visual Basic 中的 xml 功能。

 **错误 ID：** BC36593

## <a name="to-correct-this-error"></a>更正此错误

1. 将 `Import` <xref:System.Linq> 、 <xref:System.Data.Linq> 或命名空间的语句添加 <xref:System.Xml.Linq> 到代码文件中。 你还可以使用 "项目设计器 **" ("项目设计**器" 的 "**引用**" 页) 导入项目的命名空间。

2. 确保已标识为查询源的类型是可查询类型。 即，实现或的类型 <xref:System.Collections.Generic.IEnumerable%601> <xref:System.Linq.IQueryable%601> 。

## <a name="see-also"></a>另请参阅

- <xref:System.Linq>
- <xref:System.Data.Linq>
- <xref:System.Xml.Linq>
- [Visual Basic 中的 LINQ 简介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [LINQ](../../programming-guide/language-features/linq/index.md)
- [XML](../../programming-guide/language-features/xml/index.md)
- [引用和 Imports 语句](../../programming-guide/program-structure/references-and-the-imports-statement.md)
- [Imports 语句（.NET 命名空间和类型）](../statements/imports-statement-net-namespace-and-type.md)
- [项目设计器 -&gt;“引用”页 (Visual Basic)](/visualstudio/ide/reference/references-page-project-designer-visual-basic)
