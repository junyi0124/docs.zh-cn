---
title: 类型封送 - .NET
description: 了解 .NET 如何将类型封送到本机表示形式。
ms.date: 01/18/2019
ms.openlocfilehash: 7fc3dfe950ecd3ed0ff5e4eb0e101c1596a831e1
ms.sourcegitcommit: 30a686fd4377fe6472aa04e215c0de711bc1c322
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94440993"
---
# <a name="type-marshaling"></a>类型封送

**封送** 是当类型需要在托管代码和本机代码之间切换时转换类型的过程。

需要封送的原因在于托管代码与非托管代码中的类型并不相同。 例如，在托管代码中，可指定 `String`。但在非托管环境中，字符串类型可以是 Unicode（“宽型”）、非 Unicode、null 结尾、ASCII 等。默认情况下，P/Invoke 子系统会尝试基于默认行为执行正确的操作，如本文中所述。 但是，如果需要额外的控制，可以使用 [MarshalAs](xref:System.Runtime.InteropServices.MarshalAsAttribute) 属性指定要在非托管端上使用的预期类型。 例如，如果想要将字符串作为以 null 结尾的 ANSI 字符串发送，则可以执行如下操作：

```csharp
[DllImport("somenativelibrary.dll")]
static extern int MethodA([MarshalAs(UnmanagedType.LPStr)] string parameter);
```

## <a name="default-rules-for-marshaling-common-types"></a>封送通用类型的默认规则

通常，运行时会尝试在封送时执行“正确的操作”，从而最大限地减少用户的工作。 以下表格介绍了每种类型在用于参数或字段时的默认封送方式。 C99/C++11 固定宽度的整数和字符类型用于确保下表适用于所有平台。 可以使用与这些类型具有相同的对齐和大小要求的任何本机类型。

第一个表介绍了其封送与 P/Invoke 和字段封送相同的各种类型的映射。

| .NET 类型 | 本机类型  |
|-----------|-------------------------|
| `byte`    | `uint8_t`               |
| `sbyte`   | `int8_t`                |
| `short`   | `int16_t`               |
| `ushort`  | `uint16_t`              |
| `int`     | `int32_t`               |
| `uint`    | `uint32_t`              |
| `long`    | `int64_t`               |
| `ulong`   | `uint64_t`              |
| `char`    | `char` 或 `char16_t` 依赖于 P/Invoke 或结构的 `CharSet`。 请参阅[字符集文档](charset.md)。 |
| `string`  | `char*` 或 `char16_t*` 依赖于 P/Invoke 或结构的 `CharSet`。 请参阅[字符集文档](charset.md)。 |
| `System.IntPtr` | `intptr_t`        |
| `System.UIntPtr` | `uintptr_t`      |
| .NET 指针类型（例如， `void*`)  | `void*` |
| 从 `System.Runtime.InteropServices.SafeHandle` 派生的类型 | `void*` |
| 从 `System.Runtime.InteropServices.CriticalHandle` 派生的类型 | `void*`          |
| `bool`    | Win32 `BOOL` 类型       |
| `decimal` | COM `DECIMAL` 结构 |
| .NET 委托 | 本机函数指针 |
| `System.DateTime` | Win32 `DATE` 类型 |
| `System.Guid` | Win32 `GUID` 类型 |

如果作为参数或结构进行封送，则部分封送类别具有不同的默认设置。

| .NET 类型 | 本机类型（参数） | 本机类型（字段） |
|-----------|-------------------------|---------------------|
| .NET 数组 | 指向数组元素的本机表示形式的数组开头的指针。 | 不允许不带 `[MarshalAs]` 属性|
| `LayoutKind` 为 `Sequential` 或 `Explicit` 的类 | 指向类的本机表示形式的指针 | 类的本机表示形式 |

下表包含仅适用于 Windows 的默认封送规则。 在非 Windows 平台上，无法封送这些类型。

| .NET 类型 | 本机类型（参数） | 本机类型（字段） |
|-----------|-------------------------|---------------------|
| `object`  | `VARIANT`               | `IUnknown*`         |
| `System.Array` | COM 接口 | 不允许不带 `[MarshalAs]` 属性 |
| `System.ArgIterator` | `va_list` | 不允许 |
| `System.Collections.IEnumerator` | `IEnumVARIANT*` | 不允许 |
| `System.Collections.IEnumerable` | `IDispatch*` | 不允许 |
| `System.DateTimeOffset` | `int64_t` 表示自 1601 年 1 月 1 日午夜以来的时钟周期数 | `int64_t` 表示自 1601 年 1 月 1 日午夜以来的时钟周期数 |

某些类型只能作为参数封送，而不能作为字段封送。 下表列出了这些类型：

| .NET 类型 | 本机类型（仅参数） |
|-----------|------------------------------|
| `System.Text.StringBuilder` | `char*` 或 `char16_t*` 依赖于 P/Invoke 的 `CharSet`。  请参阅[字符集文档](charset.md)。 |
| `System.ArgIterator` | `va_list`（仅在 Windows x86/x64/arm64 上） |
| `System.Runtime.InteropServices.ArrayWithOffset` | `void*` |
| `System.Runtime.InteropServices.HandleRef` | `void*` |

如果这些默认设置不执行所需的操作，则可以自定义参数的封送方式。 [参数封送](customize-parameter-marshaling.md)一文介绍自定义不同参数类型的封送方式的具体步骤。

## <a name="default-marshaling-in-com-scenarios"></a>COM 方案中的默认封送

在 .NET 中调用 COM 对象上的方法时，.NET 运行时会更改默认封送规则，以匹配常见的 COM 语义。 下表列出了 .NET 运行时在 COM 方案中使用的规则：

| .NET 类型 | 本机类型（COM 方法调用） |
|-----------|--------------------------------|
| `bool`    | `VARIANT_BOOL`                 |
| `StringBuilder` | `LPWSTR`                 |
| `string`  | `BSTR`                         |
| 委托类型 | 在 .NET Framework 中为 `_Delegate*`。 .NET Core 和 .NET 5+ 中不允许使用。 |
| `System.Drawing.Color` | `OLECOLOR`        |
| .NET 数组 | `SAFEARRAY`                   |
| `string[]` | `BSTR` 的 `SAFEARRAY`        |

## <a name="marshaling-classes-and-structs"></a>封送类和结构

有关类型封送的另一个问题是如何将结构传入非托管方法。 例如，某些非托管方法需要使用结构作为参数。 在这种情况下，需要在环境的托管部分中创建相应的结构或类，以便将它用作参数。 不过，仅仅是定义类并不足够，还需要告知封送处理程序如何将类中的字段映射到非托管结构。 在此处，`StructLayout` 属性将会很有用。

```csharp
[DllImport("kernel32.dll")]
static extern void GetSystemTime(SystemTime systemTime);

[StructLayout(LayoutKind.Sequential)]
class SystemTime {
    public ushort Year;
    public ushort Month;
    public ushort DayOfWeek;
    public ushort Day;
    public ushort Hour;
    public ushort Minute;
    public ushort Second;
    public ushort Milsecond;
}

public static void Main(string[] args) {
    SystemTime st = new SystemTime();
    GetSystemTime(st);
    Console.WriteLine(st.Year);
}
```

前面的代码演示了如何调用 `GetSystemTime()` 函数的简单示例。 值得注意的部分是第 4 行。 该属性指定应按顺序将类的字段映射到另一端（非托管端）上的结构。 这意味着，字段的命名并不重要，唯一重要的是字段顺序，因为这种顺序需对应于非托管结构，如下面的示例所示：

```c
typedef struct _SYSTEMTIME {
  WORD wYear;
  WORD wMonth;
  WORD wDayOfWeek;
  WORD wDay;
  WORD wHour;
  WORD wMinute;
  WORD wSecond;
  WORD wMilliseconds;
} SYSTEMTIME, *PSYSTEMTIME;
```

有时，默认结构封送不执行所需的操作。 [自定义结构封送](./customize-struct-marshaling.md)一文介绍如何自定义结构的封送方式。
