---
title: 如何使用表达式树来生成动态查询 (C#)
description: 了解如何使用表达式树来创建动态 LINQ 查询。 如果在编译时不知道查询的细节，这些查询将十分有用。
ms.date: 07/20/2015
ms.assetid: 52cd44dd-a3ec-441e-b93a-4eca388119c7
ms.openlocfilehash: 284e7fa4534d1648c8e2bd6f4feaa62796ca60d8
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202586"
---
# <a name="how-to-use-expression-trees-to-build-dynamic-queries-c"></a>如何使用表达式树来生成动态查询 (C#)

在 LINQ 中，表达式树用于表示针对数据源的结构化查询，这些数据源可实现 <xref:System.Linq.IQueryable%601>。 例如，LINQ 提供程序可实现 <xref:System.Linq.IQueryable%601> 接口，用于查询关系数据存储。 C# 编译器将针对此类数据源的查询编译为代码，该代码在运行时会生成一个表达式树。 然后，查询提供程序可以遍历表达式树数据结构，并将其转换为适合于数据源的查询语言。  
  
 表达式树还可以用在 LINQ 中，用于表示分配给类型为 <xref:System.Linq.Expressions.Expression%601> 的变量的 lambda 表达式。  
  
 本主题描述如何使用表达式树来创建动态 LINQ 查询。 如果在编译时不知道查询的细节，动态查询将十分有用。 例如，应用程序可能会提供一个用户界面，最终用户可以使用该用户界面指定一个或多个谓词来筛选数据。 为了使用 LINQ 进行查询，这种应用程序必须使用表达式树在运行时创建 LINQ 查询。  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用表达式树依据 `IQueryable` 数据源构造一个查询，然后执行该查询。 代码将生成一个表达式树来表示以下查询：  
  
 ```csharp
 companies.Where(company => (company.ToLower() == "coho winery" || company.Length > 16))
          .OrderBy(company => company)
 ```
  
 <xref:System.Linq.Expressions> 命名空间中的工厂方法用于创建表达式树，这些表达式树表示构成总体查询的表达式。 表示标准查询运算符方法调用的表达式将引用这些方法的 <xref:System.Linq.Queryable> 实现。 最终的表达式树将传递给 `IQueryable` 数据源的提供程序的 <xref:System.Linq.IQueryProvider.CreateQuery%60%601%28System.Linq.Expressions.Expression%29> 实现，以创建 `IQueryable` 类型的可执行查询。 通过枚举该查询变量获得结果。  
  
```csharp  
// Add a using directive for System.Linq.Expressions.  
  
string[] companies = { "Consolidated Messenger", "Alpine Ski House", "Southridge Video", "City Power & Light",  
                   "Coho Winery", "Wide World Importers", "Graphic Design Institute", "Adventure Works",  
                   "Humongous Insurance", "Woodgrove Bank", "Margie's Travel", "Northwind Traders",  
                   "Blue Yonder Airlines", "Trey Research", "The Phone Company",  
                   "Wingtip Toys", "Lucerne Publishing", "Fourth Coffee" };  
  
// The IQueryable data to query.  
IQueryable<String> queryableData = companies.AsQueryable<string>();  
  
// Compose the expression tree that represents the parameter to the predicate.  
ParameterExpression pe = Expression.Parameter(typeof(string), "company");  
  
// ***** Where(company => (company.ToLower() == "coho winery" || company.Length > 16)) *****  
// Create an expression tree that represents the expression 'company.ToLower() == "coho winery"'.  
Expression left = Expression.Call(pe, typeof(string).GetMethod("ToLower", System.Type.EmptyTypes));  
Expression right = Expression.Constant("coho winery");  
Expression e1 = Expression.Equal(left, right);  
  
// Create an expression tree that represents the expression 'company.Length > 16'.  
left = Expression.Property(pe, typeof(string).GetProperty("Length"));  
right = Expression.Constant(16, typeof(int));  
Expression e2 = Expression.GreaterThan(left, right);  
  
// Combine the expression trees to create an expression tree that represents the  
// expression '(company.ToLower() == "coho winery" || company.Length > 16)'.  
Expression predicateBody = Expression.OrElse(e1, e2);  
  
// Create an expression tree that represents the expression  
// 'queryableData.Where(company => (company.ToLower() == "coho winery" || company.Length > 16))'  
MethodCallExpression whereCallExpression = Expression.Call(  
    typeof(Queryable),  
    "Where",  
    new Type[] { queryableData.ElementType },  
    queryableData.Expression,  
    Expression.Lambda<Func<string, bool>>(predicateBody, new ParameterExpression[] { pe }));  
// ***** End Where *****  
  
// ***** OrderBy(company => company) *****  
// Create an expression tree that represents the expression  
// 'whereCallExpression.OrderBy(company => company)'  
MethodCallExpression orderByCallExpression = Expression.Call(  
    typeof(Queryable),  
    "OrderBy",  
    new Type[] { queryableData.ElementType, queryableData.ElementType },  
    whereCallExpression,  
    Expression.Lambda<Func<string, string>>(pe, new ParameterExpression[] { pe }));  
// ***** End OrderBy *****  
  
// Create an executable query from the expression tree.  
IQueryable<string> results = queryableData.Provider.CreateQuery<string>(orderByCallExpression);  
  
// Enumerate the results.  
foreach (string company in results)  
    Console.WriteLine(company);  
  
/*  This code produces the following output:  
  
    Blue Yonder Airlines  
    City Power & Light  
    Coho Winery  
    Consolidated Messenger  
    Graphic Design Institute  
    Humongous Insurance  
    Lucerne Publishing  
    Northwind Traders  
    The Phone Company  
    Wide World Importers  
*/  
```  
  
 此代码在传递到 `Queryable.Where` 方法的谓词中使用固定数量的表达式。 但是，可以编写一个视用户输入而定来合并可变数量谓词表达式的应用程序。 视用户输入而定，也可以更改在查询中调用的标准查询运算符。  
  
## <a name="compiling-the-code"></a>编译代码  
  
- 包括 System.Linq.Expressions 命名空间。  
  
## <a name="see-also"></a>请参阅

- [表达式树 (C#)](./index.md)
- [如何执行表达式树 (C#)](./how-to-execute-expression-trees.md)
- [在运行时动态指定谓词筛选器](../../../linq/dynamically-specify-predicate-filters-at-runtime.md)
