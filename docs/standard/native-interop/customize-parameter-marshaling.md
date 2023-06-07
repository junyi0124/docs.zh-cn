---
title: 自定义参数封送 - .NET
description: 了解如何自定义 .NET 将参数封送到本机表示形式的方式。
ms.date: 01/18/2019
ms.openlocfilehash: 1999cad057875f15b283421f87f485c2e5ca2306
ms.sourcegitcommit: e7748001b1cee80ced691d8a76ca814c0b02dd9b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2020
ms.locfileid: "86374307"
---
# <a name="customizing-parameter-marshaling"></a>自定义参数封送

当 .NET 运行时的默认参数封送行为不执行所需的操作时，可以使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> 属性自定义参数的封送方式。

## <a name="customizing-string-parameters"></a>自定义字符串参数

.NET 具有各种可用来封送字符串的格式。 这些方法分为有关 C 样式字符串和以 Windows 为中心的字符串的不同格式部分。

### <a name="c-style-strings"></a>C 样式字符串

其中每个格式都会将以 null 结尾的字符串传递到本机代码。 它们的差别在于本机字符串的编码。

| `System.Runtime.InteropServices.UnmanagedType` 值 | 编码 |
|------------------------------------------------------|----------|
| LPStr | ANSI |
| LPUTF8Str | UTF-8 |
| LPWStr | UTF-16 |
| LPTStr | UTF-16 |

<xref:System.Runtime.InteropServices.UnmanagedType.VBByRefStr?displayProperty=nameWithType> 格式稍有不同。 与 `LPWStr` 一样，该格式会将字符串封送到采用 UTF-16 编码的本机 C 样式字符串。 但是，托管签名允许你按引用传入字符串，而匹配的本机签名会按值获取字符串。 这一区别允许你使用按值获取字符串的本机 API，并对其进行就地修改而无需使用 `StringBuilder`。 我们建议不要手动使用此格式，因为很容易导致与不匹配的本机和托管签名混淆。

### <a name="windows-centric-string-formats"></a>以 Windows 为中心的字符串格式

与 COM 或 OLE 接口交互时，可能会发现本机函数将字符串用作 `BSTR` 参数。 可以使用 <xref:System.Runtime.InteropServices.UnmanagedType.BStr?displayProperty=nameWithType> 非托管类型将字符串作为 `BSTR` 进行封送。

如果要与 WinRT API 交互，则可以使用 <xref:System.Runtime.InteropServices.UnmanagedType.HString?displayProperty=nameWithType> 格式将字符串作为 `HSTRING` 进行封送。

## <a name="customizing-array-parameters"></a>自定义数组参数

.NET 还提供多种方式来封送数组参数。 如果要调用获取 C 样式数组的 API，则使用 <xref:System.Runtime.InteropServices.UnmanagedType.LPArray?displayProperty=nameWithType> 非托管类型。 如果数组中的值需要进行自定义封送，则可以为其使用 `[MarshalAs]` 属性上的 <xref:System.Runtime.InteropServices.MarshalAsAttribute.ArraySubType> 字段。

如果使用的是 COM API，则可能必须将数组参数作为 `SAFEARRAY*` 进行封送。 为此，可以使用 <xref:System.Runtime.InteropServices.UnmanagedType.SafeArray?displayProperty=nameWithType> 非托管类型。 [自定义 `object` 字段](./customize-struct-marshaling.md#marshal-systemobject)上的表中显示了 `SAFEARRAY` 的元素的默认类型。 可以使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArraySubType?displayProperty=nameWithType> 和 <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType?displayProperty=nameWithType> 字段自定义 `SAFEARRAY` 的确切元素类型。

## <a name="customizing-boolean-or-decimal-parameters"></a>自定义布尔或十进制参数

有关封送布尔或十进制参数的信息，请参阅[自定义结构封送](customize-struct-marshaling.md)。

## <a name="customizing-object-parameters-windows-only"></a>自定义对象参数（仅适用于 Windows）

在 Windows 上，.NET 运行时提供多种不同方式，将对象参数封送到本机代码。

### <a name="marshaling-as-specific-com-interfaces"></a>作为特定的 COM 接口进行封送

如果 API 使用指向 COM 对象的指针，则可以使用类型为 `object` 的参数上的以下任一 `UnmanagedType` 格式告知 .NET 作为这些特定接口进行封送：

- `IUnknown`
- `IDispatch`
- `IInspectable`

此外，如果类型标记为 `[ComVisible(true)]`，或如果要封送 `object` 类型，则可以使用 <xref:System.Runtime.InteropServices.UnmanagedType.Interface?displayProperty=nameWithType> 格式将对象作为类型的 COM 视图的 COM 可调用包装器进行封送。

### <a name="marshaling-to-a-variant"></a>封送到 `VARIANT`

如果本机 API 采用 Win32 `VARIANT`，则可以使用 `object` 参数上的 <xref:System.Runtime.InteropServices.UnmanagedType.Struct?displayProperty=nameWithType> 格式将对象作为 `VARIANT` 进行封送。 如需了解 .NET 类型与 `VARIANT` 类型之间的映射，请参阅有关[自定义 `object` 字段](customize-struct-marshaling.md#marshal-systemobject)的文档。

### <a name="custom-marshalers"></a>自定义封送处理程序

如果要将本机 COM 接口投影到其他托管类型，则可以使用 `UnmanagedType.CustomMarshaler` 格式和 <xref:System.Runtime.InteropServices.ICustomMarshaler> 的实现来提供自己的自定义封送处理代码。
