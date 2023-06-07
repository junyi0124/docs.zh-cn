---
title: 特性
description: 了解属性F#如何使元数据应用于编程构造。
ms.date: 02/19/2020
ms.openlocfilehash: 77b84713ab9360166b3634d406993cf209c8a623
ms.sourcegitcommit: 771c554c84ba38cbd4ac0578324ec4cfc979cf2e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77543633"
---
# <a name="attributes"></a>特性

特性使元数据可以应用于编程构造。

## <a name="syntax"></a>语法

```fsharp
[<target:attribute-name(arguments)>]
```

## <a name="remarks"></a>备注

在前面的语法中，*目标*是可选的，如果存在，则指定应用该属性的程序实体的类型。 *目标*的有效值显示在本文档后面的表中。

*特性名称*是指具有有效属性类型的名称（可能用命名空间限定），其中包含或不包含在属性类型名称中通常使用的后缀 `Attribute`。 例如，可以将类型 `ObsoleteAttribute` 缩短为仅在此上下文中 `Obsolete`。

*参数*是属性类型的构造函数的参数。 如果特性具有无参数的构造函数，则可以省略参数列表和括号。 特性支持位置参数和命名参数。 *位置参数*是按其出现顺序使用的参数。 如果特性具有公共属性，则可以使用命名参数。 可以通过在参数列表中使用以下语法来设置这些参数。

```fsharp
property-name = property-value
```

此类属性初始化可以按任意顺序进行，但必须遵循任何位置自变量。 下面是使用位置参数和属性初始化的特性的一个示例：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6202.fs)]

在此示例中，属性是 `DllImportAttribute`的，此处以缩写形式使用。 第一个参数是位置参数，第二个参数是一个属性。

特性是一个 .NET 编程构造，使称为*属性*的对象可与类型或其他程序元素相关联。 应用特性的程序元素称为*属性目标*。 该属性通常包含有关其目标的元数据。 在这种情况下，元数据可以是除字段和成员之外的任何类型的数据。

中F#的属性可应用于以下编程构造：函数、方法、程序集、模块、类型（类、记录、结构、接口、委托、枚举、联合等）、构造函数、属性、字段、参数、类型参数和返回值。 不允许在类、表达式或工作流表达式中的 `let` 绑定上使用特性。

通常，特性声明直接出现在属性目标的声明之前。 可以将多个属性声明一起使用，如下所示：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6603.fs)]

可以通过使用 .NET 反射在运行时查询属性。

如前面的代码示例中所示，可以单独声明多个特性，也可以在一组方括号中将它们声明为，前提是使用分号分隔各个特性和构造函数，如下所示：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6604.fs)]

常见的属性包括 `Obsolete` 特性、安全注意事项特性、COM 支持的特性、与代码所有权相关的特性，以及指示是否可以序列化类型的特性。 下面的示例演示如何使用 `Obsolete` 特性。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6605.fs)]

对于特性目标 `assembly` 和 `module`，请将特性应用于程序集中的顶级 `do` 绑定。 可以在属性声明中包含单词 `assembly` 或 `module`，如下所示：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6606.fs)]

如果省略应用于 `do` 绑定的属性的属性目标，则编译器将F#尝试确定特性目标。 许多特性类都有一个类型 `System.AttributeUsageAttribute` 的属性，其中包括有关该特性支持的可能目标的信息。 如果 `System.AttributeUsageAttribute` 指示特性支持作为目标的函数，则会将该特性应用于程序的主入口点。 如果 `System.AttributeUsageAttribute` 指示特性支持作为目标的程序集，则编译器会将特性应用于程序集。 大多数特性并不适用于函数和程序集，但在这些特性的情况下，会将特性应用于程序的 main 函数。 如果显式指定了特性目标，则该特性将应用于指定的目标。

尽管通常不需要显式指定属性目标，但在属性中，*目标*的有效值和用法示例如下表中所示：

<table>
  <tr>
    <th>特性目标</td>
    <th>示例</td>
  </tr>
  <tr>
    <td>程序集 (assembly)</td>
    <td><pre><code class="lang-fsharp">[&lt;assembly: AssemblyVersion("1.0.0.0")&gt;]</code></pre></td>
  </tr>
  <tr>
    <td>return</td>
    <td><pre><code class="lang-fsharp">let function1 x : [&lt;return: MyCustomAttributeThatWorksOnReturns&gt;] int = x + 1</code></pre></td>
  </tr>
  <tr>
    <td>Field — 字段</td>
    <td><pre><code class="lang-fsharp">[&lt;DefaultValue&gt;] val mutable x: int</code></pre></td>
  </tr>
  <tr>
    <td>属性</td>
    <td><pre><code class="lang-fsharp">[&lt;Obsolete&gt;] this.MyProperty = x</code></pre></td>
  </tr>
  <tr>
    <td>param</td>
    <td><pre><code class="lang-fsharp">member this.MyMethod([&lt;Out&gt;] x : ref&lt;int&gt;) = x := 10</code></pre></td>
  </tr>
  <tr>
    <td>类型</td>
    <td>
        <pre><code class="lang-fsharp">
[&lt;type: StructLayout(LayoutKind.Sequential)&gt;]
type MyStruct =
  struct
    val x : byte
    val y : int
  end</code></pre>
    </td>
  </tr>
</table>

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
