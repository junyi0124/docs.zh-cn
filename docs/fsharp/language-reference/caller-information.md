---
title: 调用方信息
description: 介绍如何使用调用方信息参数特性从方法获取调用方信息。
ms.date: 11/04/2019
ms.openlocfilehash: 700cde26fbe4e6c48155f88bfc63af59af86cfe2
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739771"
---
# <a name="caller-information"></a>调用方信息

通过使用调用方信息特性，可获取有关方法的调用方的信息。 可以获取源代码的文件路径、源代码中的行号和调用方的成员名称。 此信息有助于跟踪、调试和创建诊断工具。

若要获取此信息，可以使用应用于可选参数的特性，每个特性都具有默认值。 下表列出了在 [system.runtime.compilerservices](/dotnet/api/system.runtime.compilerservices) 命名空间中定义的调用方信息特性：

|Attribute|描述|类型|
|---------|-----------|----|
|[CallerFilePath](/dotnet/api/system.runtime.compilerservices.callerfilepathattribute)|包含调用方的源文件的完整路径。 这是编译时的文件路径。|`String`
|[CallerLineNumber](/dotnet/api/system.runtime.compilerservices.callerlinenumberattribute)|源文件中调用方法的行号。|`Integer`|
|[CallerMemberName](/dotnet/api/system.runtime.compilerservices.callermembernameattribute)|调用方的方法或属性名称。 请参阅本主题后面的成员名称部分。|`String`|

## <a name="example"></a>示例

下面的示例演示如何使用这些特性跟踪调用方。

```fsharp
open System.Diagnostics
open System.Runtime.CompilerServices
open System.Runtime.InteropServices

type Tracer() =
    member _.DoTrace(message: string,
                      [<CallerMemberName; Optional; DefaultParameterValue("")>] memberName: string,
                      [<CallerFilePath; Optional; DefaultParameterValue("")>] path: string,
                      [<CallerLineNumber; Optional; DefaultParameterValue(0)>] line: int) =
        Trace.WriteLine(sprintf $"Message: {message}")
        Trace.WriteLine(sprintf $"Member name: {memberName}")
        Trace.WriteLine(sprintf $"Source file path: {path}")
        Trace.WriteLine(sprintf $"Source line number: {line}")
```

## <a name="remarks"></a>备注

调用方信息属性只能应用于可选参数。 调用方信息特性导致编译器为使用调用方信息特性修饰的每个可选参数写入正确的值。

在编译时，调用方信息值将作为文本传入中间语言 (IL)。 不同于异常的 [StackTrace](/dotnet/api/system.diagnostics.stacktrace) 属性的结果，结果不受模糊处理的影响。

你可显式提供可选参数来控制调用方信息或隐藏调用方信息。

## <a name="member-names"></a>成员名称

可以使用 [`CallerMemberName`](/dotnet/api/system.runtime.compilerservices.callermembernameattribute) 特性来避免将成员名称指定为 `String` 所调用的方法的参数。 通过使用此方法，您可以避免 "重命名重构" 不更改 `String` 值的问题。 此好处对于以下任务特别有用：

- 使用跟踪和诊断例程。
- 在绑定数据时实现 [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged) 接口。 此接口允许对象的属性通知绑定控件该属性已更改，以便此控件能够显示更新的信息。 如果没有 [`CallerMemberName`](/dotnet/api/system.runtime.compilerservices.callermembernameattribute) 特性，则必须将属性名称指定为文本。

下图显示了使用 CallerMemberName 特性时返回的成员名称。

|调用发生中|成员名称结果|
|-------------------|------------------|
|方法、属性或事件|从中发起调用的方法、属性或事件的名称。|
|构造函数|字符串“.ctor”|
|静态构造函数|字符串“.cctor”|
|析构函数|字符串“Finalize”|
|用户定义的运算符或转换|为成员生成的名称，例如，“op_Addition”。|
|特性构造函数|要应用特性的成员的名称。 如果该特性是成员中的任何元素（如参数、返回值或泛型参数），则此结果是与该元素关联的成员的名称。|
|无包含的成员（例如，程序集级别或应用于类型的特性）|可选参数的默认值。|

## <a name="see-also"></a>请参阅

- [特性](attributes.md)
- [命名参数](parameters-and-arguments.md#named-arguments)
- [可选参数](parameters-and-arguments.md#optional-parameters)
