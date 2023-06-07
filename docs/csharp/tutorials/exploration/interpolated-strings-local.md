---
title: 字符串内插 - C# 教程
description: 本教程介绍了如何使用 C# 字符串内插功能将格式化表达式结果添加到较大的字符串中。
ms.date: 10/23/2018
ms.openlocfilehash: d1b78670361e8b333499d12b68c0364ad9e40a85
ms.sourcegitcommit: de7f589de07a9979b6ac28f54c3e534a617d9425
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82796049"
---
# <a name="use-string-interpolation-to-construct-formatted-strings"></a>使用字符串内插构造格式化字符串

本教程介绍了如何使用 C# [字符串内插](../../language-reference/tokens/interpolated.md)将值插入单个结果字符串中。 读者可以编写 C# 代码并查看代码编译和运行结果。 本教程包含一系列课程，介绍了如何将值插入字符串，以及用不同方式设置这些值的格式。

本教程要求你有一台可用于开发的计算机。 .NET 教程 [Hello World 10 分钟入门](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/intro)介绍了如何在 Windows、Linux 或 macOS 上设置本地开发环境。 另外，还可在浏览器中完成本教程的[交互式版本](interpolated-strings.yml)。

## <a name="create-an-interpolated-string"></a>创建内插字符串

创建名为 interpolated 的目录  。 将其设置为当前目录并从控制台窗口运行以下命令：

```dotnetcli
dotnet new console
```

此命令会在当前目录中创建一个新的 .NET Core 控制台应用程序。

在常用编辑器中，打开 Program.cs，将行 `Console.WriteLine("Hello World!");` 替换为以下代码，并将 `<name>` 替换为你的姓名  ：

```csharp
var name = "<name>";
Console.WriteLine($"Hello, {name}. It's a pleasure to meet you!");
```

通过在控制台窗口键入 `dotnet run` 试运行此代码。 当运行该程序时，它会在问候语中显示一个包含你的姓名的字符串。 <xref:System.Console.WriteLine%2A> 方法调用中包含的字符串是一个内插字符串表达式  。 这是一种模板，可让你用包含嵌入代码的字符串构造单个字符串（称为结果字符串  ）。 内插字符串特别适用于将值插入字符串或连接字符串（将字符串联在一起）。

该简单示例包含了每个内插字符串必须具有的两个元素：

- 字符串文本以 `$` 字符开头，后接左双引号字符。 `$` 符号和引号字符之间不能有空格。 （如果希望看到包含空格会发生什么情况，请在 `$` 字符后面插入一个空格并保存该文件，然后在控制台窗口中键入 `dotnet run` 再次运行该程序。 C# 编译器显示错误消息“错误 CS1056:意外的字符 '$'”。）

- 一个或多个内插表达式  。 左大括号和右大括号（`{` 和 `}`）指示内插表达式。 可将任何返回值的 C# 表达式置于大括号内（包括 `null`）。

下面再尝试一些其他数据类型的字符串内插示例。

## <a name="include-different-data-types"></a>包含不同的数据类型

上一节使用了字符串内插将一个字符串插入到了另一字符串中。 不过，内插字符串表达式的结果可以是任何数据类型。 下面让我们在内插字符串中添加多种数据类型的值。

在以下示例中，首先定义一个具有 `Name` [属性](../../properties.md)和 `ToString` [方法](../../methods.md)的[类](../../programming-guide/classes-and-structs/classes.md)数据类型 `Vegetable`，它可以[替代](../../language-reference/keywords/override.md) <xref:System.Object.ToString?displayProperty=nameWithType> 方法的行为。 [`public` 访问修饰符](../../language-reference/keywords/public.md)使该方法可用于任何客户端代码以获取 `Vegetable` 实例的字符串表示形式。 在本示例中，`Vegetable.ToString` 方法返回在 `Vegetable` [构造函数](../../programming-guide/classes-and-structs/constructors.md)处初始化的 `Name` 属性的值：

```csharp
public Vegetable(string name) => Name = name;
```

然后，通过使用 [`new` 运算符](../../language-reference/operators/new-operator.md)并为构造函数 `Vegetable` 提供一个名称来创建名为 `item` 的 `Vegetable` 类的实例：

```csharp
var item = new Vegetable("eggplant");
```

最后，将 `item` 变量添加到同样包含 <xref:System.DateTime> 值、<xref:System.Decimal> 值和 `Unit` [枚举](../../language-reference/builtin-types/enum.md)值的内插字符串中。 将编辑器中的所有 C# 代码替换为以下代码，然后使用 `dotnet run` 命令运行：

```csharp
using System;

public class Vegetable
{
   public Vegetable(string name) => Name = name;

   public string Name { get; }

   public override string ToString() => Name;
}

public class Program
{
   public enum Unit { item, kilogram, gram, dozen };

   public static void Main()
   {
      var item = new Vegetable("eggplant");
      var date = DateTime.Now;
      var price = 1.99m;
      var unit = Unit.item;
      Console.WriteLine($"On {date}, the price of {item} was {price} per {unit}.");
   }
}
```

注意，内插字符串中的内插表达式 `item` 会解析为结果字符串中的“eggplant”文本。 这是因为，当表达式结果的类型不是字符串时，会按照以下方式将其解析为字符串：

- 如果内插表达式的计算结果为 `null`，则会使用一个空字符串（"" 或 <xref:System.String.Empty?displayProperty=nameWithType>）。

- 如果内插表达式的计算结果不是 `null`，通常会调用结果类型的 `ToString` 方法。 可以通过更新 `Vegetable.ToString` 方法的实现来进行测试。 你甚至不用实现 `ToString` 方法，因为每个类型都有一些此方法的实现。 可通过注释掉示例中 `Vegetable.ToString` 方法的定义（在它前面添加注释符号 `//` 即可）来进行测试。 在输出中，字符串“eggplant”被替换为完全限定的类型名称（本示例中为“Vegetable”），这是 <xref:System.Object.ToString?displayProperty=nameWithType> 方法的默认行为。 对于枚举值的 `ToString` 方法，其默认行为是返回该值的字符串表示形式。

在此示例的输出中，日期过于精确（eggplant 的价格不会以秒为单位变化），且价格值没有标明货币单位。 下一节将介绍如何通过控制表达式结果的字符串表示形式来解决这些问题。

## <a name="control-the-formatting-of-interpolation-expressions"></a>控制内插表达式的格式

上一节将两个格式不正确的字符串插入到了结果字符串中。 一个是日期和时间值，只有日期是合适的。 第二个是没有标明货币单位的价格。 这两个问题都很容易解决。 通过字符串内插，可以指定用于控制特定类型格式的格式字符串  。 将前面示例中的调用修改为 `Console.WriteLine`，从而包含日期和价格表达式的格式字符串，如以下行所示：

```csharp
Console.WriteLine($"On {date:d}, the price of {item} was {price:C2} per {unit}.");
```

可通过在内插表达式后接冒号（“:”）和格式字符串来指定格式字符串。 “d”是[标准日期和时间格式字符串](../../../standard/base-types/standard-date-and-time-format-strings.md#the-short-date-d-format-specifier)，表示短日期格式。 “C2”是[标准数值格式字符串](../../../standard/base-types/standard-numeric-format-strings.md#the-currency-c-format-specifier)，用数字表示货币值（精确到小数点后两位）。

.NET 库中的许多类型支持一组预定义的格式字符串。 这些格式字符串包括所有数值类型以及日期和时间类型。 有关支持格式字符串的完整类型列表，请参阅 [.NET 中的格式化类型](../../../standard/base-types/formatting-types.md)文章中的[格式字符串和. NET 类库类型](../../../standard/base-types/formatting-types.md#format-strings-and-net-types)。

尝试在文本编辑器中修改格式字符串，并在每次更改时重新运行该程序，查看更改如何影响日期和时间以及数值的格式。 将 `{date:d}` 中的“d”更改为“t”（显示短时间格式）、“y”（显示年份和月份）和“yyyy”（显示四位数年份）。 将 `{price:C2}` 中的“C2”更改为“e”（用于指数计数法）和“F3”（使数值在小数点后保持三位数字）。

除了控制格式之外，还可以控制结果字符串中包含的格式字符串的字段宽度和对齐方式。 下一节会介绍如何执行此操作。

## <a name="control-the-field-width-and-alignment-of-interpolation-expressions"></a>控制内插表达式的字段宽度和对齐方式

通常，当内插表达式的结果格式化为字符串时，结果字符串中会包含该字符串，但没有前导或尾随空格。 特别是对于使用一组数据的情况，控制字段宽度和对齐方式有助于增强输出的可读性。 为此，用下方代码替换文本编辑器中的所有代码，然后键入 `dotnet run` 来执行程序：

```csharp
using System;
using System.Collections.Generic;

public class Example
{
   public static void Main()
   {
      var titles = new Dictionary<string, string>()
      {
          ["Doyle, Arthur Conan"] = "Hound of the Baskervilles, The",
          ["London, Jack"] = "Call of the Wild, The",
          ["Shakespeare, William"] = "Tempest, The"
      };

      Console.WriteLine("Author and Title List");
      Console.WriteLine();
      Console.WriteLine($"|{"Author",-25}|{"Title",30}|");
      foreach (var title in titles)
         Console.WriteLine($"|{title.Key,-25}|{title.Value,30}|");
   }
}
```

创建者姓名采用左对齐方式，其所写标题采用右对齐方式。 通过在内插表达式后面添加一个逗号（“,”）并指定“最小”  字段宽度来指定对齐方式。 如果指定的值是正数，则该字段为右对齐。 如果它为负数，则该字段为左对齐。

尝试删除 `{"Author",-25}` 和 `{title.Key,-25}` 代码中的负号，然后再次运行该示例，如以下代码所示：

```csharp
Console.WriteLine($"|{"Author",25}|{"Title",30}|");
foreach (var title in titles)
   Console.WriteLine($"|{title.Key,25}|{title.Value,30}|");
```

此时，创建者信息为右对齐。

可合并单个内插表达式中的对齐说明符和格式字符串。 为此，请先指定对齐方式，然后是冒号和格式字符串。 将 `Main` 方法中的所有代码替换为以下代码，该代码用定义的字段宽度显示格式化字符串。 然后输入 `dotnet run` 命令来运行程序。

```csharp
Console.WriteLine($"[{DateTime.Now,-20:d}] Hour [{DateTime.Now,-10:HH}] [{1063.342,15:N2}] feet");
```

输出类似于以下内容：

```console
[04/14/2018          ] Hour [16        ] [       1,063.34] feet
```

你已完成“字符串内插”教程。

有关详细信息，请参阅[字符串内插](../../language-reference/tokens/interpolated.md)主题和 [C# 中的字符串内插](../string-interpolation.md)教程。
