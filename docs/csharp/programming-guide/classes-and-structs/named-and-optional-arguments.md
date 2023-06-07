---
title: 命名参数和可选参数 - C# 编程指南
description: C# 中的命名参数按名称而不是位置指定参数。 可以省略可选元素。
ms.date: 09/25/2020
ms.custom: contperf-fy21q1
f1_keywords:
- namedParameter_CSharpKeyword
- cs_namedParameter
helpviewer_keywords:
- parameters [C#], named
- named arguments [C#]
- arguments [C#], named
- optional arguments [C#]
- arguments [C#], optional
- parameters [C#], optional
- named and optional arguments [C#]
ms.assetid: 839c960c-c2dc-4d05-af4d-ca5428e54008
ms.openlocfilehash: bb79d956124a610bac0de6825c1f42655789e98d
ms.sourcegitcommit: d0990c1c1ab2f81908360f47eafa8db9aa165137
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97513102"
---
# <a name="named-and-optional-arguments-c-programming-guide"></a>命名实参和可选实参（C# 编程指南）

C# 4 介绍命名实参和可选实参。 通过命名实参，你可以为形参指定实参，方法是将实参与该形参的名称匹配，而不是与形参在形参列表中的位置匹配。 通过 *可选参数*，你可以为某些形参省略实参。 这两种技术都可与方法、索引器、构造函数和委托一起使用。

使用命名参数和可选参数时，将按实参出现在实参列表（而不是形参列表）中的顺序计算这些实参。

通过命名形参和可选形参，你可以为所选形参提供实参。 此功能极大地方便了对 COM 接口（例如 Microsoft Office 自动化 API）的调用。

## <a name="named-arguments"></a>命名实参

有了命名实参，你将不再匹配形参在所调用方法的形参列表中的顺序。 每个实参的形参都可按形参名称进行指定。 例如，通过以函数定义的顺序按位置发送实参，可以调用打印订单详细信息（例如卖家姓名、订单号和产品名称）的函数。

```csharp
PrintOrderDetails("Gift Shop", 31, "Red Mug");
```

如果不记得形参的顺序，但却知道其名称，则可以按任意顺序发送实参。

```csharp
PrintOrderDetails(orderNum: 31, productName: "Red Mug", sellerName: "Gift Shop");
PrintOrderDetails(productName: "Red Mug", sellerName: "Gift Shop", orderNum: 31);
```

命名实参还可以标识每个实参所表示的含义，从而改进代码的可读性。 在下面的示例方法中，`sellerName` 不得为 NULL 或空白符。 由于 `sellerName` 和 `productName` 都是字符串类型，所以使用命名实参而不是按位置发送实参是有意义的，可以区分这两种类型并减少代码阅读者的困惑。
  
当命名实参与位置实参一起使用时，只要

- 没有后接任何位置实参或

    ```csharp
    PrintOrderDetails("Gift Shop", 31, productName: "Red Mug");
    ```

- 以 C# 7.2 开头，则它们就有效并用在正确位置。 在以下示例中，形参 `orderNum` 位于正确的位置，但未显式命名。

    ```csharp
    PrintOrderDetails(sellerName: "Gift Shop", 31, productName: "Red Mug");
    ```

遵循任何无序命名参数的位置参数无效。

```csharp
// This generates CS1738: Named argument specifications must appear after all fixed arguments have been specified.
PrintOrderDetails(productName: "Red Mug", 31, "Gift Shop");
```

## <a name="example"></a>示例

以下代码执行本节以及某些其他节中的示例。  

[!code-csharp[csProgGuideNamedAndOptional#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/program.cs#1)]

## <a name="optional-arguments"></a>可选实参

方法、构造函数、索引器或委托的定义可以指定其形参为必需还是可选。 任何调用都必须为所有必需的形参提供实参，但可以为可选的形参省略实参。

每个可选形参都有一个默认值作为其定义的一部分。 如果没有为该形参发送实参，则使用默认值。 默认值必须是以下类型的表达式之一：
  
- 常量表达式；  
- `new ValType()` 形式的表达式，其中 `ValType` 是值类型，例如 [enum](../../language-reference/builtin-types/enum.md) 或 [struct](../../language-reference/builtin-types/struct.md)；
- [default(ValType)](../../language-reference/operators/default.md) 形式的表达式，其中 `ValType` 是值类型。

可选参数定义于参数列表的末尾和必需参数之后。 如果调用方为一系列可选形参中的任意一个形参提供了实参，则它必须为前面的所有可选形参提供实参。 实参列表中不支持使用逗号分隔的间隔。 例如，在以下代码中，使用一个必选形参和两个可选形参定义实例方法 `ExampleMethod`。

[!code-csharp[csProgGuideNamedAndOptional#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/optional.cs#15)]

下面对 `ExampleMethod` 的调用会导致编译器错误，原因是为第三个形参而不是为第二个形参提供了实参。

```csharp
//anExample.ExampleMethod(3, ,4);
```

但是，如果知道第三个形参的名称，则可以使用命名实参来完成此任务。

```csharp
anExample.ExampleMethod(3, optionalint: 4);
```

IntelliSense 使用括号表示可选形参，如下图所示：

![显示 ExampleMethod 方法的 IntelliSense 快速信息的屏幕截图。](./media/named-and-optional-arguments/optional-examplemethod-parameters.png)

> [!NOTE]
> 此外，还可通过使用 .NET <xref:System.Runtime.InteropServices.OptionalAttribute> 类声明可选参数。 `OptionalAttribute` 形参不需要默认值。

## <a name="example"></a>示例

在以下示例中，`ExampleClass` 的构造函数具有一个可选形参。 实例方法 `ExampleMethod` 具有一个必选形参（`required`）和两个可选形参（`optionalstr` 和 `optionalint`）。 `Main` 中的代码演示了可用于调用构造函数和方法的不同方式。

[!code-csharp[csProgGuideNamedAndOptional#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/optional.cs#2)]

前面的代码演示了一些示例，其中不会正确应用可选形参。 第一个示例说明了必须为第一个形参提供实参，这是必需的。
  
## <a name="com-interfaces"></a>COM 接口

命名实参和可选实参，以及对动态对象的支持大大提高了与 COM API（例如 Office Automation API）的互操作性。

例如，Microsoft Office Excel 的 <xref:Microsoft.Office.Interop.Excel.Range> 接口中的 <xref:Microsoft.Office.Interop.Excel.Range.AutoFormat%2A> 方法有七个可选形参。 这些形参如下图所示：

![显示 AutoFormat 方法的 IntelliSense 快速信息的屏幕截图。](./media/named-and-optional-arguments/autoformat-method-parameters.png)

在 C# 3.0 以及早期版本中，每个形参都需要一个实参，如下例所示。

[!code-csharp[csProgGuideNamedAndOptional#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/namedandoptcom.cs#3)]

但是，可以通过使用 C# 4.0 中引入的命名实参和可选实参来大大简化对 `AutoFormat` 的调用。 如果不希望更改形参的默认值，则可以通过使用命名实参和可选实参来省略可选形参的实参。 在下面的调用中，仅为 7 个形参中的其中一个指定了值。

[!code-csharp[csProgGuideNamedAndOptional#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/namedandoptcom.cs#13)]

有关详细信息和示例，请参阅[如何在 Office 编程中使用命名参数和可选参数](./how-to-use-named-and-optional-arguments-in-office-programming.md)和[如何使用 C# 功能访问 Office 互操作性对象](../interop/how-to-access-office-onterop-objects.md)。
  
## <a name="overload-resolution"></a>重载决策

使用命名实参和可选实参将在以下方面对重载决策产生影响：

- 如果方法、索引器或构造函数的每个参数是可选的，或按名称或位置对应于调用语句中的单个自变量，且该自变量可转换为参数的类型，则方法、索引器或构造函数为执行的候选项。  
- 如果找到多个候选项，则会将用于首选转换的重载决策规则应用于显式指定的自变量。 将忽略可选形参已省略的实参。
- 如果两个候选项不相上下，则会将没有可选形参的候选项作为首选项，对于这些可选形参，已在调用中为其省略了实参。 重载决策通常首选具有较少形参的候选项。
  
## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]
