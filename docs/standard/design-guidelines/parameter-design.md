---
title: 参数设计
ms.date: 10/22/2008
helpviewer_keywords:
- member design guidelines [.NET Framework], parameters
- members [.NET Framework], parameters
- names [.NET Framework], parameters
- parameters, design guidelines
- reserved parameters
ms.assetid: 3f33bf46-4a7b-43b3-bb78-1ffebe0dcfa6
ms.openlocfilehash: 815075198f34c0c045603b9d377b9d5fbdf1a91d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95707875"
---
# <a name="parameter-design"></a>参数设计

本部分提供有关参数设计的广泛准则，包括包含检查参数准则的部分。 此外，还应参阅 [命名参数](naming-parameters.md)中所述的准则。

 ✔️使用最不提供成员所需功能的派生参数类型。

 例如，假设要设计一个方法，该方法可枚举集合并将每个项输出到控制台。 此类方法应采用 <xref:System.Collections.IEnumerable> 参数，而不是 <xref:System.Collections.ArrayList> 或 <xref:System.Collections.IList> ，例如。

 ❌ 不要使用保留的参数。

 如果在将来的某个版本中需要对成员进行更多输入，则可以添加新的重载。

 ❌ 不具有公开的方法，这些方法采用指针、指针数组或多维数组作为参数。

 指针和多维数组相对较难正确使用。 几乎在所有情况下，都可以重新设计 Api，以避免将这些类型作为参数。

 ✔️将所有 `out` 参数都置于除参数数组) 之外的所有按值和 `ref` 参数 (，即使它导致重载之间参数排序不一致 (参阅 [成员重载](member-overloading.md)) 。

 `out`参数可以视为额外的返回值，并将它们组合在一起，使方法签名更易于理解。

 重写成员或实现接口成员时，✔️在命名参数中保持一致。

 这更好地传达了方法之间的关系。

### <a name="choosing-between-enum-and-boolean-parameters"></a>选择枚举参数和布尔参数  

 如果成员将使用两个或多个布尔参数，✔️确实使用枚举。

 ❌ 不要使用布尔值，除非你完全确定不需要两个以上的值。

 枚举为您提供了一些空间供将来添加值，但您应了解将值添加到枚举的所有含义，如 [枚举设计](enum.md)中所述。

 ✔️考虑为构造函数参数使用布尔值，这些参数是真正的双状态值，只用于初始化布尔属性。

### <a name="validating-arguments"></a>验证参数

 ✔️验证传递到公共、受保护或显式实现的成员的参数。 <xref:System.ArgumentException?displayProperty=nameWithType>如果验证失败，则引发或其子类之一。

 请注意，实际验证不一定要在公共或受保护的成员本身中发生。 在某些专用或内部例程中，它可能会在较低的级别上发生。 要点是向最终用户公开的整个图面区域会检查参数。

 <xref:System.ArgumentNullException>如果传递了 null 参数并且该成员不支持 null 参数，✔️将引发。

 ✔️验证枚举参数。

 不要假设枚举参数将在由枚举定义的范围内。 CLR 允许将任何整数值强制转换为枚举值，即使枚举中未定义该值。

 ❌ 不要用于 <xref:System.Enum.IsDefined%2A?displayProperty=nameWithType> 枚举范围检查。

 ✔️请注意，可变参数在验证后可能已更改。

 如果成员区分安全，则建议您创建一个副本，然后验证并处理参数。

### <a name="parameter-passing"></a>参数传递

 从框架设计器的角度来看，有三个主要参数组：按值参数、 `ref` 参数和 `out` 参数。

 当通过值参数传递参数时，该成员将接收传入的实参的副本。 如果参数是值类型，则将参数的副本放在堆栈上。 如果参数是引用类型，则将引用的副本放在堆栈上。 最常用的 CLR 语言（例如 c #、VB.NET 和 c + +）默认为通过值传递参数。

 当通过参数传递参数时 `ref` ，该成员将接收对传入的实际参数的引用。 如果参数是值类型，则将对该参数的引用置于堆栈上。 如果参数是引用类型，则对该引用的引用将放在堆栈上。 `Ref` 参数可用于允许成员修改调用方传递的参数。

 `Out` 参数类似于 `ref` 参数，但有一些细微的差异。 参数最初被视为未分配，在为其赋值之前，无法在成员正文中读取。 此外，在成员返回之前，必须为参数赋值。

 ❌ 避免使用 `out` 或 `ref` 参数。

 使用 `out` 或 `ref` 参数需要使用指针的经验，了解值类型和引用类型的不同之处，以及处理具有多个返回值的方法。 此外，和参数之间的差异 `out` 并 `ref` 不被广泛理解。 一般受众设计框架架构师不应指望用户使用 `out` 或 `ref` 参数。

 ❌ 不要通过引用传递引用类型。

 规则有一些例外情况，例如可用于交换引用的方法。

### <a name="members-with-variable-number-of-parameters"></a>参数数目可变的成员

 可以通过提供数组参数来表示可采用可变数量的参数的成员。 例如， <xref:System.String> 提供以下方法：

```csharp
public class String {
    public static string Format(string format, object[] parameters);
}
```

 然后，用户可以调用 <xref:System.String.Format%2A?displayProperty=nameWithType> 方法，如下所示：

 `String.Format("File {0} not found in {1}",new object[]{filename,directory});`

 向数组参数添加 c # params 关键字会将参数更改为所谓的 params 数组参数，并提供创建临时数组的快捷方式。

```csharp
public class String {
    public static string Format(string format, params object[] parameters);
}
```

 这样，用户就可以通过在参数列表中直接传递数组元素来调用方法。

 `String.Format("File {0} not found in {1}",filename,directory);`

 请注意，只能将 params 关键字添加到参数列表中的最后一个参数。

 如果预计最终用户要传递具有少量元素的数组，✔️考虑将 params 关键字添加到数组参数。 如果预计会在常见方案中传递多个元素，则用户可能无法以内联方式传递这些元素，因此不需要 params 关键字。

 ❌ 如果调用方几乎始终具有数组中的输入，请避免使用 params 数组。

 例如，具有字节数组参数的成员几乎不会通过传递单个字节来调用。 出于此原因，.NET Framework 中的字节数组参数不使用 params 关键字。

 ❌ 如果使用 params 数组参数的成员修改了数组，则不要使用 params 数组。

 由于很多编译器会将成员的参数转换为调用站点的临时数组，因此数组可能是临时对象，因此对数组进行的任何修改都将丢失。

 ✔️考虑在简单重载中使用 params 关键字，即使比较复杂的重载无法使用它也是如此。

 询问用户是否要将 params 数组作为参数的值，即使它不在所有重载中也是如此。

 ✔️尝试对参数进行排序，以便可以使用 params 关键字。

 ✔️考虑为在极高性能的 Api 中使用少量参数的调用提供特殊重载和代码路径。

 这样，当使用少量参数调用 API 时，就可以避免创建数组对象。 通过采用数组参数的单数形式并添加数字后缀，形成参数的名称。

 仅当要使用特殊的代码路径（而不仅仅是创建数组并调用更常规的方法）时，才应执行此操作。

 ✔️请注意，null 可作为参数数组参数进行传递。

 在处理之前，应该验证数组是否为 null。

 ❌ 不要使用 `varargs` 方法，也称为省略号。

 某些 CLR 语言（如 c + +）支持用于传递变量参数列表（称为方法）的替代约定 `varargs` 。 不应在框架中使用约定，因为它不符合 CLS。

### <a name="pointer-parameters"></a>指针参数

 通常，指针不应出现在设计良好的托管代码框架的公共表面区中。 大多数情况下，应封装指针。 但是，在某些情况下，出于互操作性原因需要使用指针，因此在这种情况下使用指针是合适的。

 ✔️确实为采用指针参数的任何成员提供了一种替代方法，因为指针不符合 CLS。

 ❌ 避免对指针参数执行昂贵的参数检查。

 ✔️在用指针设计成员时遵循常见的指针相关约定。

 例如，无需传递开始索引，因为可以使用简单指针算法来实现相同的结果。

 *部分 &copy; 2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [成员设计准则](member.md)
- [框架设计准则](index.md)
