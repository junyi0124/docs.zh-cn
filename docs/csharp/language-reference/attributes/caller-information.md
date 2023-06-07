---
title: C# 保留的特性：跟踪调用方信息
ms.date: 04/09/2020
description: 这些特性指示编译器生成有关调用成员的代码的信息。 可使用 CallerFilePath、CallerLineNumber 和 CallerMemberName 提供详细的跟踪信息
ms.openlocfilehash: ee061d4cae35bdcc0b89007e360e94fee8c5f87c
ms.sourcegitcommit: c91110ef6ee3fedb591f3d628dc17739c4a7071e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2020
ms.locfileid: "81389822"
---
# <a name="reserved-attributes-determine-caller-information"></a>保留的特性：确定调用方信息

使用信息属性，可以获取有关方法调用方的信息。 可以获取源代码的文件路径、源代码中的行号和调用方的成员名称。 若要获取成员调用方信息，可以使用应用于可选参数的特性。 每个可选参数指定一个默认值。 下表列出在 <xref:System.Runtime.CompilerServices?displayProperty=nameWithType> 命名空间中定义的调用方信息特性：

|特性|描述|类型|
|---|---|---|
|<xref:System.Runtime.CompilerServices.CallerFilePathAttribute>|包含调用方的源文件的完整路径。 完整路径是编译时的路径。|`String`|
|<xref:System.Runtime.CompilerServices.CallerLineNumberAttribute>|源文件中调用方法的行号。|`Integer`|
|<xref:System.Runtime.CompilerServices.CallerMemberNameAttribute>|调用方的方法名称或属性名称。|`String`|

此信息有助于你编写跟踪、调试和创建诊断工具。 下面的示例演示如何使用调用方信息特性。 每次调用 `TraceMessage` 方法时，调用方信息将替换为可选参数的变量。

```csharp
public void DoProcessing()
{
    TraceMessage("Something happened.");
}

public void TraceMessage(string message,
        [CallerMemberName] string memberName = "",
        [CallerFilePath] string sourceFilePath = "",
        [CallerLineNumber] int sourceLineNumber = 0)
{
    Trace.WriteLine("message: " + message);
    Trace.WriteLine("member name: " + memberName);
    Trace.WriteLine("source file path: " + sourceFilePath);
    Trace.WriteLine("source line number: " + sourceLineNumber);
}

// Sample Output:
//  message: Something happened.
//  member name: DoProcessing
//  source file path: c:\Visual Studio Projects\CallerInfoCS\CallerInfoCS\Form1.cs
//  source line number: 31
```

为每个可选参数指定显式默认值。 不能将调用方信息特性应用于未指定为可选的参数。 调用方信息特性不会使参数成为可选参数。 相反，它们会在忽略此参数时影响传入的默认值。 在编译时，调用方信息值将作为文本传入中间语言 (IL)。 与异常的 <xref:System.Exception.StackTrace%2A> 属性的结果不同，这些结果不受模糊处理的影响。 你可显式提供可选参数来控制调用方信息或隐藏调用方信息。

### <a name="member-names"></a>成员名称

可以使用 `CallerMemberName` 特性来避免将成员名称指定为所调用的方法的 `String` 参数。 通过使用这种技术，可以避免“重命名重构”  不更改 `String` 值的问题。 此好处对于以下任务特别有用：

- 使用跟踪和诊断例程。
- 在绑定数据时实现 <xref:System.ComponentModel.INotifyPropertyChanged> 接口。 此接口允许对象的属性通知绑定控件该属性已更改，以便此控件能够显示更新的信息。 如果没有 `CallerMemberName` 特性，则必须将属性名称指定为文本。

以下图表显示在使用 `CallerMemberName` 特性时返回的成员名称。

|调用发生中|成员名称结果|
|-|-|
|方法、属性或事件|从中发起调用的方法、属性或事件的名称。|
|构造函数|字符串“.ctor”|
|静态构造函数|字符串“.cctor”|
|析构函数|字符串“Finalize”|
|用户定义的运算符或转换|为成员生成的名称，例如，“op_Addition”。|
|特性构造函数|要应用特性的方法或属性的名称。 如果该特性是成员中的任何元素（如参数、返回值或泛型参数），则此结果是与该元素关联的成员的名称。|
|无包含的成员（例如，程序集级别或应用于类型的特性）|可选参数的默认值。|

## <a name="see-also"></a>请参阅

- [命名参数和可选参数](../../programming-guide/classes-and-structs/named-and-optional-arguments.md)
- <xref:System.Reflection>
- <xref:System.Attribute>
- [特性](../../../standard/attributes/index.md)
