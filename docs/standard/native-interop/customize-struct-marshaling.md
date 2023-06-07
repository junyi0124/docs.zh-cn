---
title: 自定义结构封送 - .NET
description: 了解如何自定义 .NET 将结构封送到本机表示形式的方式。
ms.date: 01/18/2019
dev_langs:
- csharp
- cpp
ms.openlocfilehash: c82e0099c44b8033cad241d69bdd284243711a50
ms.sourcegitcommit: e7748001b1cee80ced691d8a76ca814c0b02dd9b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2020
ms.locfileid: "86374528"
---
# <a name="customize-structure-marshaling"></a>自定义结构封送

有时，结构的默认封送规则无法完全满足要求。 .NET 运行时提供了几个扩展点以自定义结构的布局和字段的封送方式。

## <a name="customizing-structure-layout"></a>自定义结构布局

.NET 提供了 <xref:System.Runtime.InteropServices.StructLayoutAttribute?displayProperty=nameWithType> 属性和 <xref:System.Runtime.InteropServices.LayoutKind?displayProperty=nameWithType> 枚举，允许用户自定义字段在内存中的放置方式。 以下指南将帮助你避免常见问题。

✔️ 请考虑尽量使用 `LayoutKind.Sequential`。

✔️ 当本机结构还具有显式布局（如联合）时，请务必仅将 `LayoutKind.Explicit` 用于封送。

❌ 如果需要在 .NET Core 3.0 之前以运行时为目标，请避免在非 Windows 平台上封送结构时使用 `LayoutKind.Explicit`。 3\.0 之前的 .NET Core 运行时不支持在 Intel 或 AMD 64 位的非 Windows 系统上，按值将显式结构传递到本机函数。 但是，运行时支持在所有平台上按引用传递显式结构。

## <a name="customizing-boolean-field-marshaling"></a>自定义布尔字段封送

本机代码具有许多不同的布尔表示形式。 仅在 Windows 上，有三种方式可用于表示布尔值。 运行时不知道结构的本机定义，因此，它最多只能对如何封送布尔值做出猜测。 .NET 运行时提供指示如何封送布尔字段的方式。 下面的示例介绍如何将 .NET `bool` 封送到不同的本机布尔类型。

布尔值默认作为本机 4 字节 Win32 [`BOOL`](/windows/desktop/winprog/windows-data-types#BOOL) 值进行封送，如下面的示例所示：

```csharp
public struct WinBool
{
    public bool b;
}
```

```cpp
struct WinBool
{
    public BOOL b;
};
```

如果想要明确指出，则可以使用 <xref:System.Runtime.InteropServices.UnmanagedType.Bool?displayProperty=nameWithType> 值获取如上所述的行为：

```csharp
public struct WinBool
{
    [MarshalAs(UnmanagedType.Bool)]
    public bool b;
}
```

```cpp
struct WinBool
{
    public BOOL b;
};
```

使用下面的 `UnmanagedType.U1` 或 `UnmanagedType.I1` 值，可以告知运行时将 `b` 字段作为 1 字节本机 `bool` 类型进行封送。

```csharp
public struct CBool
{
    [MarshalAs(UnmanagedType.U1)]
    public bool b;
}
```

```cpp
struct CBool
{
    public bool b;
};
```

在 Windows 上，可以使用 <xref:System.Runtime.InteropServices.UnmanagedType.VariantBool?displayProperty=nameWithType> 值告知运行时将布尔值封送到 2 字节的 `VARIANT_BOOL` 值：

```csharp
public struct VariantBool
{
    [MarshalAs(UnmanagedType.VariantBool)]
    public bool b;
}
```

```cpp
struct VariantBool
{
    public VARIANT_BOOL b;
};
```

> [!NOTE]
> `VARIANT_BOOL` 与 `VARIANT_TRUE = -1` 和 `VARIANT_FALSE = 0` 中的大多数 bool 类型不同。 此外，不等于 `VARIANT_TRUE` 的所有值都将被视为 false。

## <a name="customizing-array-field-marshaling"></a>自定义数组字段封送

.NET 还包括自定义数组封送的多种方式。

默认情况下，.NET 将数组作为指向元素的连续列表的指针进行封送：

```csharp
public struct DefaultArray
{
    public int[] values;
}
```

```cpp
struct DefaultArray
{
    int* values;
};
```

如果要与 COM API 交互，则可能必须将数组作为 `SAFEARRAY*` 对象进行封送。 可以使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> 和 <xref:System.Runtime.InteropServices.UnmanagedType.SafeArray?displayProperty=nameWithType> 值告知运行时将数组作为 `SAFEARRAY*` 进行封送：

```csharp
public struct SafeArrayExample
{
    [MarshalAs(UnmanagedType.SafeArray)]
    public int[] values;
}
```

```cpp
struct SafeArrayExample
{
    SAFEARRAY* values;
};
```

如果需要自定义 `SAFEARRAY` 中的元素类型，则可以使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArraySubType?displayProperty=nameWithType> 和 <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType?displayProperty=nameWithType> 字段自定义 `SAFEARRAY` 的确切元素类型。

如果需要就地封送数组，则可以使用 <xref:System.Runtime.InteropServices.UnmanagedType.ByValArray?displayProperty=nameWithType> 值告知封送处理程序就地封送数组。 使用此封送时，还必须为数组中的元素数对应的 <xref:System.Runtime.InteropServices.MarshalAsAttribute.SizeConst?displayProperty=nameWithType> 字段提供一个值，以便运行时可以正确地为结构分配空间。

```csharp
public struct InPlaceArray
{
    [MarshalAs(UnmanagedType.ByValArray, SizeConst = 4)]
    public int[] values;
}
```

```cpp
struct InPlaceArray
{
    int values[4];
};
```

> [!NOTE]
> .NET 不支持将变长数组字段作为 C99 可变数组成员进行封送。

## <a name="customizing-string-field-marshaling"></a>自定义字符串字段封送

.NET 还提供用于封送字符串字段的各种自定义。

默认情况下，.NET 将字符串作为指向以 null 结尾的字符串的指针进行封送。 编码取决于 <xref:System.Runtime.InteropServices.StructLayoutAttribute?displayProperty=nameWithType> 中的 <xref:System.Runtime.InteropServices.StructLayoutAttribute.CharSet?displayProperty=nameWithType> 字段的值。 如果未指定任何属性，则编码将默认为 ANSI 编码。

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
public struct DefaultString
{
    public string str;
}
```

```cpp
struct DefaultString
{
    char* str;
};
```

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
public struct DefaultString
{
    public string str;
}
```

```cpp
struct DefaultString
{
    char16_t* str; // Could also be wchar_t* on Windows.
};
```

如果需要对不同字段使用不同编码或只是希望在结构定义中更加明确，则可以在 <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> 属性上使用 <xref:System.Runtime.InteropServices.UnmanagedType.LPStr?displayProperty=nameWithType> 或 <xref:System.Runtime.InteropServices.UnmanagedType.LPWStr?displayProperty=nameWithType> 值。

```csharp
public struct AnsiString
{
    [MarshalAs(UnmanagedType.LPStr)]
    public string str;
}
```

```cpp
struct AnsiString
{
    char* str;
};
```

```csharp
public struct UnicodeString
{
    [MarshalAs(UnmanagedType.LPWStr)]
    public string str;
}
```

```cpp
struct UnicodeString
{
    char16_t* str; // Could also be wchar_t* on Windows.
};
```

如果想要使用 UTF-8 编码封送字符串，则可以在 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 中使用 <xref:System.Runtime.InteropServices.UnmanagedType.LPUTF8Str?displayProperty=nameWithType> 值。

```csharp
public struct UTF8String
{
    [MarshalAs(UnmanagedType.LPUTF8Str)]
    public string str;
}
```

```cpp
struct UTF8String
{
    char* str;
};
```

> [!NOTE]
> 使用 <xref:System.Runtime.InteropServices.UnmanagedType.LPUTF8Str?displayProperty=nameWithType> 需要 .NET Framework 4.7（或更高版本）或 .NET Core 1.1（或更高版本）。 不能在 .NET Standard 2.0 中使用。

如果使用 COM API，则可能需要将字符串作为 `BSTR` 进行封送。 使用 <xref:System.Runtime.InteropServices.UnmanagedType.BStr?displayProperty=nameWithType> 值可以将字符串作为 `BSTR` 进行封送。

```csharp
public struct BString
{
    [MarshalAs(UnmanagedType.BStr)]
    public string str;
}
```

```cpp
struct BString
{
    BSTR str;
};
```

使用基于 WinRT 的 API 时，可能需要将字符串作为 `HSTRING` 进行封送。 使用 <xref:System.Runtime.InteropServices.UnmanagedType.HString?displayProperty=nameWithType> 值可以将字符串作为 `HSTRING` 进行封送。

```csharp
public struct HString
{
    [MarshalAs(UnmanagedType.HString)]
    public string str;
}
```

```cpp
struct BString
{
    HSTRING str;
};
```

如果 API 要求你将字符串就地传入结构，则可以使用 <xref:System.Runtime.InteropServices.UnmanagedType.ByValTStr?displayProperty=nameWithType> 值。 务必注意，通过 `ByValTStr` 封送的字符串的编码由 `CharSet` 属性确定。 此外，还需要通过 <xref:System.Runtime.InteropServices.MarshalAsAttribute.SizeConst?displayProperty=nameWithType> 字段传递字符串长度。

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
public struct DefaultString
{
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 4)]
    public string str;
}
```

```cpp
struct DefaultString
{
    char str[4];
};
```

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
public struct DefaultString
{
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 4)]
    public string str;
}
```

```cpp
struct DefaultString
{
    char16_t str[4]; // Could also be wchar_t[4] on Windows.
};
```

## <a name="customizing-decimal-field-marshaling"></a>自定义十进制字段封送

如果在 Windows 上操作，则可能会遇到一些使用本机 [`CY` 或 `CURRENCY`](/windows/win32/api/wtypes/ns-wtypes-cy-r1) 结构的 API。 默认情况下，.NET `decimal` 类型会封送到本机 [`DECIMAL`](/windows/win32/api/wtypes/ns-wtypes-decimal-r1) 结构。 但是，可以使用包含 <xref:System.Runtime.InteropServices.UnmanagedType.Currency?displayProperty=nameWithType> 值的 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 来指示封送处理程序将 `decimal` 值转换为本机 `CY` 值。

```csharp
public struct Currency
{
    [MarshalAs(UnmanagedType.Currency)]
    public decimal dec;
}
```

```cpp
struct Currency
{
    CY dec;
};
```

## <a name="marshal-systemobject"></a>封送 `System.Object`

在 Windows 上，可以将类型为 `object` 的字段封送到本机代码。 可以将这些字段封送到以下三个类型之一：

- [`VARIANT`](/windows/win32/api/oaidl/ns-oaidl-variant)
- [`IUnknown*`](/windows/desktop/api/unknwn/nn-unknwn-iunknown)
- [`IDispatch*`](/windows/desktop/api/oaidl/nn-oaidl-idispatch)

默认情况下，将类型为 `object` 的字段封送到用来包装对象的 `IUnknown*`。

```csharp
public struct ObjectDefault
{
    public object obj;
}
```

```cpp
struct ObjectDefault
{
    IUnknown* obj;
};
```

如果要将对象字段封送到 `IDispatch*`，请添加包含 <xref:System.Runtime.InteropServices.UnmanagedType.IDispatch?displayProperty=nameWithType> 值的 <xref:System.Runtime.InteropServices.MarshalAsAttribute>。

```csharp
public struct ObjectDispatch
{
    [MarshalAs(UnmanagedType.IDispatch)]
    public object obj;
}
```

```cpp
struct ObjectDispatch
{
    IDispatch* obj;
};
```

如果要将其作为 `VARIANT` 进行封送，请添加包含 <xref:System.Runtime.InteropServices.UnmanagedType.Struct?displayProperty=nameWithType> 值的 <xref:System.Runtime.InteropServices.MarshalAsAttribute>。

```csharp
public struct ObjectVariant
{
    [MarshalAs(UnmanagedType.Struct)]
    public object obj;
}
```

```cpp
struct ObjectVariant
{
    VARIANT obj;
};
```

下表介绍了如何将 `obj` 字段的不同运行时类型映射到存储在 `VARIANT` 中的各种类型：

| .NET 类型 | VARIANT 类型 | | .NET 类型 | VARIANT 类型 |
|------------|--------------|-|----------|--------------|
|  `byte`  | `VT_UI1` |     | `System.Runtime.InteropServices.BStrWrapper` | `VT_BSTR` |
| `sbyte`  | `VT_I1`  |     | `object`  | `VT_DISPATCH` |
| `short`  | `VT_I2`  |     | `System.Runtime.InteropServices.UnknownWrapper` | `VT_UNKNOWN` |
| `ushort` | `VT_UI2` |     | `System.Runtime.InteropServices.DispatchWrapper` | `VT_DISPATCH` |
| `int`    | `VT_I4`  |     | `System.Reflection.Missing` | `VT_ERROR` |
| `uint`   | `VT_UI4` |     | `(object)null` | `VT_EMPTY` |
| `long`   | `VT_I8`  |     | `bool` | `VT_BOOL` |
| `ulong`  | `VT_UI8` |     | `System.DateTime` | `VT_DATE` |
| `float`  | `VT_R4`  |     | `decimal` | `VT_DECIMAL` |
| `double` | `VT_R8`  |     | `System.Runtime.InteropServices.CurrencyWrapper` | `VT_CURRENCY` |
| `char`   | `VT_UI2` |     | `System.DBNull` | `VT_NULL` |
| `string` | `VT_BSTR`|
