---
title: 对象和集合初始值设定项 - C# 编程指南
description: 在调用构造函数后，C# 中的对象初始值设定项在创建时将值分配给对象的可访问字段或属性。
ms.date: 12/19/2018
helpviewer_keywords:
- object initializers [C#]
- collection initializers [C#]
ms.assetid: c58f3db5-d7d4-4651-bd2d-5a3a97357f61
ms.openlocfilehash: 81deed8a21bff10364524c3e0784c562d4e727e6
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864769"
---
# <a name="object-and-collection-initializers-c-programming-guide"></a>对象和集合初始值设定项（C# 编程指南）

使用 C# 可以在单条语句中实例化对象或集合并执行成员分配。

## <a name="object-initializers"></a>对象初始值设定项

使用对象初始值设定项，你可以在创建对象时向对象的任何可访问字段或属性分配值，而无需调用后跟赋值语句行的构造函数。 利用对象初始值设定项语法，你可为构造函数指定参数或忽略参数（以及括号语法）。  以下示例演示如何使用具有命名类型 `Cat` 的对象初始值设定项以及如何调用无参数构造函数。 请注意，自动实现的属性在 `Cat` 类中的用法。 有关详细信息，请参阅[自动实现的属性](auto-implemented-properties.md)。  
  
[!code-csharp[ObjectInitializer1](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#CatDeclaration)]  
[!code-csharp[ObjectInitializer1a](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#ObjectPropertyInitialization)]  

对象初始值设定项语法允许你创建一个实例，然后将具有其分配属性的新建对象指定给赋值中的变量。

从 C# 6 开始，除了分配字段和属性外，对象初始值设定项还可以设置索引器。 请思考这个基本的 `Matrix` 类：

[!code-csharp[ObjectInitializer2](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#MatrixDeclaration)]  

可以使用以下代码初始化标识矩阵：

[!code-csharp[ObjectInitializer2a](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#MatrixInitialization)]  

包含可访问资源库的任何可访问索引器都可以用作对象初始值设定项中的表达式之一，这与参数的数量或类型无关。 索引参数构成左侧赋值，而表达式右侧是值。  例如，如果 `IndexersExample` 具有适当的索引器，则这些都是有效的：

```csharp
var thing = new IndexersExample {
    name = "object one",
    [1] = '1',
    [2] = '4',
    [3] = '9',
    Size = Math.PI,
    ['C',4] = "Middle C"
}
```

对于要进行编译的前面的代码，`IndexersExample` 类型必须具有以下成员：

```csharp
public string name;
public double Size { set { ... }; }
public char this[int i] { set { ... }; }
public string this[char c, int i] {  set { ... }; }
```

## <a name="object-initializers-with-anonymous-types"></a>具有匿名类型的对象初始值设定项

尽管对象初始值设定项可用于任何上下文中，但它们在 LINQ 查询表达式中特别有用。 查询表达式常使用只能通过使用对象初始值设定项进行初始化的[匿名类型](./anonymous-types.md)，如下面的声明所示。  

```csharp
var pet = new { Age = 10, Name = "Fluffy" };  
```

利用匿名类型，LINQ 查询表达式中的 `select` 子句可以将原始序列的对象转换为其值和形状可能不同于原始序列的对象。 如果你只想存储某个序列中每个对象的部分信息，则这很有用。 在下面的示例中，假定产品对象 (`p`) 包含很多字段和方法，而你只想创建包含产品名和单价的对象序列。  
  
[!code-csharp[ObjectInitializer3](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#AnonymousUse)]  

执行此查询时，`productInfos` 变量将包含一系列对象，这些对象可以在 `foreach` 语句中进行访问，如下面的示例所示：  

```csharp
foreach(var p in productInfos){...}  
```

新的匿名类型中的每个对象都具有两个公共属性，这两个属性接收与原始对象中的属性或字段相同的名称。 你还可在创建匿名类型时重命名字段；下面的示例将 `UnitPrice` 字段重命名为 `Price`。  

```csharp
select new {p.ProductName, Price = p.UnitPrice};  
```

## <a name="collection-initializers"></a>集合初始值设定项

在初始化实现 <xref:System.Collections.IEnumerable> 的集合类型和初始化使用适当的签名作为实例方法或扩展方法的 `Add` 时，集合初始值设定项允许指定一个或多个元素初始值设定项。 元素初始值设定项可以是简单的值、表达式或对象初始值设定项。 通过使用集合初始值设定项，无需指定多个调用；编译器将自动添加这些调用。  
  
下面的示例演示了两个简单的集合初始值设定项：  

```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };  
List<int> digits2 = new List<int> { 0 + 1, 12 % 3, MakeInt() };  
```

下面的集合初始值设定项使用对象初始值设定项来初始化上一个示例中定义的 `Cat` 类的对象。 请注意，各个对象初始值设定项分别括在大括号中且用逗号隔开。  
  
[!code-csharp[ListInitializer](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#ListInitializer)]  
  
如果集合的 `Add` 方法允许，则可以将 [null](../../language-reference/keywords/null.md) 指定为集合初始值设定项中的一个元素。  
  
[!code-csharp[ListInitializerNull](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#ListInitialerWithNull)]  
  
 如果集合支持读取/写入索引，可以指定索引元素。
  
[!code-csharp[DictionaryInitializer](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#DictionaryIndexerInitializer)]  

前面的示例生成调用 <xref:System.Collections.Generic.Dictionary%602.Item(%600)> 以设置值的代码。 在 C# 6 之前，可以使用以下语法初始化字典和其他关联容器。 请注意，它使用具有多个值的对象，而不是带括号和赋值的索引器语法：

[!code-csharp[DictionaryAddInitializer](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#DictionaryAddInitializer)]  

此初始值设定项示例调用 <xref:System.Collections.Generic.Dictionary%602.Add(%600,%601)>，将这三个项添加到字典中。 由于编译器生成的方法调用不同，这两种初始化关联集合的不同方法的行为略有不同。 这两种变量都适用于 `Dictionary` 类。 其他类型根据它们的公共 API 可能只支持两者中的一种。

## <a name="examples"></a>示例

下例结合了对象和集合初始值设定项的概念。

[!code-csharp[InitializerExample](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#FullExample)]  

下面的示例展示了实现 <xref:System.Collections.IEnumerable> 且包含具有多个参数的 `Add` 方法的一个对象，它使用在列表中每项具有多个元素的集合初始值设定项，这些元素对应于 `Add` 方法的签名。

[!code-csharp[InitializerListExample](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#FullListExample)]  

`Add` 方法可使用 `params` 关键字来获取可变数量的自变量，如下例中所示。 此示例还演示了索引器的自定义实现，以使用索引初始化集合。

[!code-csharp[InitializerListExample](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#FullDictionaryInitializer)]  

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [C# 中的 LINQ](../../linq/index.md)
- [匿名类型](anonymous-types.md)
