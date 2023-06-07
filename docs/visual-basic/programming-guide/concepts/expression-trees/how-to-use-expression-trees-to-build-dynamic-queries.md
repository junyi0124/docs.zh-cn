---
title: 如何：使用表达式树生成动态查询
ms.date: 07/20/2015
ms.assetid: 16278787-7532-4b65-98b2-7a412406c4ee
ms.openlocfilehash: 1f686c37b5d04263ac5ae0dd6883aa8a519ed19e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410974"
---
# <a name="how-to-use-expression-trees-to-build-dynamic-queries-visual-basic"></a>如何：使用表达式树生成动态查询（Visual Basic）

在 LINQ 中，表达式树用于表示针对数据源的结构化查询，这些数据源可实现 <xref:System.Linq.IQueryable%601>。 例如，LINQ 提供程序可实现 <xref:System.Linq.IQueryable%601> 接口，用于查询关系数据存储。 Visual Basic 编译器将针对此类数据源的查询编译为在运行时生成表达式树的代码。 然后，查询提供程序可以遍历表达式树数据结构，并将其转换为适合于数据源的查询语言。

表达式树还可以用在 LINQ 中，用于表示分配给类型为 <xref:System.Linq.Expressions.Expression%601> 的变量的 lambda 表达式。

本主题描述如何使用表达式树来创建动态 LINQ 查询。 如果在编译时不知道查询的细节，动态查询将十分有用。 例如，应用程序可能会提供一个用户界面，最终用户可以使用该用户界面指定一个或多个谓词来筛选数据。 为了使用 LINQ 进行查询，这种应用程序必须使用表达式树在运行时创建 LINQ 查询。

## <a name="example"></a>示例

下面的示例演示如何使用表达式树依据 `IQueryable` 数据源构造一个查询，然后执行该查询。 代码将生成一个表达式树来表示以下查询：

`companies.Where(Function(company) company.ToLower() = "coho winery" OrElse company.Length > 16).OrderBy(Function(company) company)`

<xref:System.Linq.Expressions> 命名空间中的工厂方法用于创建表达式树，这些表达式树表示构成总体查询的表达式。 表示标准查询运算符方法调用的表达式将引用这些方法的 <xref:System.Linq.Queryable> 实现。 最终的表达式树将传递给 `IQueryable` 数据源的提供程序的 <xref:System.Linq.IQueryProvider.CreateQuery%60%601%28System.Linq.Expressions.Expression%29> 实现，以创建 `IQueryable` 类型的可执行查询。 通过枚举该查询变量获得结果。

```vb
' Add an Imports statement for System.Linq.Expressions.

Dim companies =
    {"Consolidated Messenger", "Alpine Ski House", "Southridge Video", "City Power & Light",
     "Coho Winery", "Wide World Importers", "Graphic Design Institute", "Adventure Works",
     "Humongous Insurance", "Woodgrove Bank", "Margie's Travel", "Northwind Traders",
     "Blue Yonder Airlines", "Trey Research", "The Phone Company",
     "Wingtip Toys", "Lucerne Publishing", "Fourth Coffee"}

' The IQueryable data to query.
Dim queryableData As IQueryable(Of String) = companies.AsQueryable()

' Compose the expression tree that represents the parameter to the predicate.
Dim pe As ParameterExpression = Expression.Parameter(GetType(String), "company")

' ***** Where(Function(company) company.ToLower() = "coho winery" OrElse company.Length > 16) *****
' Create an expression tree that represents the expression: company.ToLower() = "coho winery".
Dim left As Expression = Expression.Call(pe, GetType(String).GetMethod("ToLower", System.Type.EmptyTypes))
Dim right As Expression = Expression.Constant("coho winery")
Dim e1 As Expression = Expression.Equal(left, right)

' Create an expression tree that represents the expression: company.Length > 16.
left = Expression.Property(pe, GetType(String).GetProperty("Length"))
right = Expression.Constant(16, GetType(Integer))
Dim e2 As Expression = Expression.GreaterThan(left, right)

' Combine the expressions to create an expression tree that represents the
' expression: company.ToLower() = "coho winery" OrElse company.Length > 16).
Dim predicateBody As Expression = Expression.OrElse(e1, e2)

' Create an expression tree that represents the expression:
' queryableData.Where(Function(company) company.ToLower() = "coho winery" OrElse company.Length > 16)
Dim whereCallExpression As MethodCallExpression = Expression.Call(
        GetType(Queryable),
        "Where",
        New Type() {queryableData.ElementType},
        queryableData.Expression,
        Expression.Lambda(Of Func(Of String, Boolean))(predicateBody, New ParameterExpression() {pe}))
' ***** End Where *****

' ***** OrderBy(Function(company) company) *****
' Create an expression tree that represents the expression:
' whereCallExpression.OrderBy(Function(company) company)
Dim orderByCallExpression As MethodCallExpression = Expression.Call(
        GetType(Queryable),
        "OrderBy",
        New Type() {queryableData.ElementType, queryableData.ElementType},
        whereCallExpression,
        Expression.Lambda(Of Func(Of String, String))(pe, New ParameterExpression() {pe}))
' ***** End OrderBy *****

' Create an executable query from the expression tree.
Dim results As IQueryable(Of String) = queryableData.Provider.CreateQuery(Of String)(orderByCallExpression)

' Enumerate the results.
For Each company As String In results
    Console.WriteLine(company)
Next

' This code produces the following output:
'
' Blue Yonder Airlines
' City Power & Light
' Coho Winery
' Consolidated Messenger
' Graphic Design Institute
' Humongous Insurance
' Lucerne Publishing
' Northwind Traders
' The Phone Company
' Wide World Importers
```

此代码在传递到 `Queryable.Where` 方法的谓词中使用固定数量的表达式。 但是，可以编写一个视用户输入而定来合并可变数量谓词表达式的应用程序。 视用户输入而定，也可以更改在查询中调用的标准查询运算符。

## <a name="compile-the-code"></a>编译代码

- 创建新的**控制台应用程序**项目。

- 包括 System.Linq.Expressions 命名空间。

- 复制该示例中的代码并将其粘贴到 `Main` `Sub` 过程中。

## <a name="see-also"></a>另请参阅

- [表达式树 (Visual Basic)](index.md)
- [如何：执行表达式树 (Visual Basic)](how-to-execute-expression-trees.md)
