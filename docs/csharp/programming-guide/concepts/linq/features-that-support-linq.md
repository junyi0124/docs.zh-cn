---
title: 支持 LINQ 的 C# 功能
description: 了解要用于 LINQ 查询和其他上下文中的 C# 功能。 C# 3.0 中引入了这些语言构造。
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ [C#], features supporting LINQ
ms.assetid: 524b0078-ebfd-45a7-b390-f2ceb9d84797
ms.openlocfilehash: 13254c69193e04ffcf11e3e23f1deb460063a6c1
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063687"
---
# <a name="c-features-that-support-linq"></a>支持 LINQ 的 C# 功能

下一节介绍 C# 3.0 中引入的新语言构造。 虽然这些新功能在一定程度上都用于 LINQ 查询，但并不限于 LINQ，如果认为有用，在任何情况下都可以使用这些新功能。

## <a name="query-expressions"></a>查询表达式

查询表达式使用类似于 SQL 或 XQuery 的声明性语法来查询 IEnumerable 集合。 在编译时，查询语法转换为对 LINQ 提供程序的标准查询运算符扩展方法实现的方法调用。 应用程序通过使用 `using` 指令指定适当的命名空间来控制范围内的标准查询运算符。 下面的查询表达式获取一个字符串数组，按字符串中的第一个字符对字符串进行分组，然后对各组进行排序。

```csharp
var query = from str in stringArray
            group str by str[0] into stringGroup
            orderby stringGroup.Key
            select stringGroup;
```

有关详细信息，请参阅 [LINQ 查询表达式](../../../linq/index.md)。

## <a name="implicitly-typed-variables-var"></a>隐式类型化变量 (var)

可以使用 [var](../../../language-reference/keywords/var.md) 修饰符来指示编译器推断并分配类型，而不必在声明并初始化变量时显式指定类型，如下所示：

```csharp
var number = 5;
var name = "Virginia";
var query = from str in stringArray
            where str[0] == 'm'
            select str;
```

声明为 `var` 的变量与显式指定其类型的变量一样都是强类型。 通过使用 `var`，可以创建匿名类型，但它只能用于本地变量。 也可以使用隐式类型声明数组。

有关详细信息，请参阅[隐式类型局部变量](../../classes-and-structs/implicitly-typed-local-variables.md)。

## <a name="object-and-collection-initializers"></a>对象和集合初始值设定项

通过对象和集合初始值设定项，初始化对象时无需为对象显式调用构造函数。 初始值设定项通常用在将源数据投影到新数据类型的查询表达式中。 假定一个类名为 `Customer`，具有公共 `Name` 和 `Phone` 属性，可以按下列代码中所示使用对象初始值设定项：

```csharp
var cust = new Customer { Name = "Mike", Phone = "555-1212" };
```

继续我们的 `Customer` 类，假设有一个名为 `IncomingOrders` 的数据源，并且每个订单具有一个较大的 `OrderSize`，我们希望基于该订单创建新的 `Customer`。 可以在此数据源上执行 LINQ 查询，并使用对象初始化来填充集合：

```csharp
var newLargeOrderCustomers = from o in IncomingOrders
                            where o.OrderSize > 5
                            select new Customer { Name = o.Name, Phone = o.Phone };
```

数据源可能具有比 `Customer` 类更多的属性，例如 `OrderSize`，但执行对象初始化后，从查询返回的数据被定型为所需的数据类型；我们选择与我们的类相关的数据。 因此，我们现在有填充了我们想要的多个新 `Customer` 的 `IEnumerable`。 上述代码也可以使用 LINQ 的方法语法编写：

```csharp
var newLargeOrderCustomers = IncomingOrders.Where(x => x.OrderSize > 5).Select(y => new Customer { Name = y.Name, Phone = y.Phone });
```

有关详情，请参阅：

- [对象和集合初始值设定项](../../classes-and-structs/object-and-collection-initializers.md)

- [标准查询运算符的查询表达式语法](./query-expression-syntax-for-standard-query-operators.md)

## <a name="anonymous-types"></a>匿名类型

匿名类型由编译器构造，且类型名称只可用于编译器。 匿名类型提供一种在查询结果中对一组属性临时分组的简便方法，无需定义单独的命名类型。 使用新的表达式和对象初始值设定项初始化匿名类型，如下所示：

```csharp
select new {name = cust.Name, phone = cust.Phone};
```

有关详细信息，请参阅[匿名类型](../../classes-and-structs/anonymous-types.md)。

## <a name="extension-methods"></a>扩展方法

扩展方法是一种可与类型关联的静态方法，因此可以像实例方法那样对类型调用它。 实际上，利用此功能，可以将新方法“添加”到现有类型，而不会实际修改它们。 标准查询运算符是一组扩展方法，它们为实现 <xref:System.Collections.Generic.IEnumerable%601> 的任何类型提供 LINQ 查询功能。

有关详细信息，请参阅[扩展方法](../../classes-and-structs/extension-methods.md)。

## <a name="lambda-expressions"></a>Lambda 表达式

Lambda 表达式是一种内联函数，该函数使用 => 运算符将输入参数与函数体分离，并且可以在编译时转换为委托或表达式树。 在 LINQ 编程中，在对标准查询运算符进行直接方法调用时，会遇到 lambda 表达式。

有关详细信息，请参见:

- [匿名函数](../../statements-expressions-operators/anonymous-functions.md)

- [Lambda 表达式](../../../language-reference/operators/lambda-expressions.md)

- [表达式树 (C#)](../expression-trees/index.md)

## <a name="see-also"></a>请参阅

- [语言集成查询 (LINQ) (C#)](./index.md)
