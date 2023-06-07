---
title: 字符串的默认封送处理
description: 查看 .NET 中接口、平台调用、结构和定长字符串缓冲区中的字符串的默认封送行为。
ms.date: 03/20/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- strings, interop marshaling
- interop marshaling, strings
ms.assetid: 9baea3ce-27b3-4b4f-af98-9ad0f9467e6f
ms.openlocfilehash: 81df2dcc132c8ce057fa3e0e7d0ad04832f7a48b
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90555050"
---
# <a name="default-marshaling-for-strings"></a>字符串的默认封送处理

<xref:System.String?displayProperty=nameWithType> 和 <xref:System.Text.StringBuilder?displayProperty=nameWithType> 类均具有类似的封送处理行为。

字符串以 COM 样式 `BSTR` 类型或以 null 结尾的字符串（以 null 字符结尾的字符数组）进行封送。 字符串中的字符可以采用 Unicode（Windows 系统上的默认值）或 ANSI 进行封送。

## <a name="strings-used-in-interfaces"></a>在接口中使用的字符串

下表显示以非托管代码的方法自变量封送字符串数据类型时的封送处理选项。 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 特性向 COM 接口的封送字符串提供若干个 <xref:System.Runtime.InteropServices.UnmanagedType> 枚举值。

|枚举类型|非托管格式说明|
|----------------------|-------------------------------------|
|`UnmanagedType.BStr`（默认值）|具有预先固定长度和 Unicode 字符的 COM 样式 `BSTR`。|
|`UnmanagedType.LPStr`|指向以 null 终止的 ANSI 字符数组的指针。|
|`UnmanagedType.LPWStr`|指向以 null 终止的 Unicode 字符数组的指针。|

此表适用于 <xref:System.String>。 对于 <xref:System.Text.StringBuilder>，仅允许使用 `UnmanagedType.LPStr` 和`UnmanagedType.LPWStr` 两个选项。

以下示例演示在 `IStringWorker` 接口中声明的字符串。

```csharp
public interface IStringWorker
{
    void PassString1(string s);
    void PassString2([MarshalAs(UnmanagedType.BStr)] string s);
    void PassString3([MarshalAs(UnmanagedType.LPStr)] string s);
    void PassString4([MarshalAs(UnmanagedType.LPWStr)] string s);
    void PassStringRef1(ref string s);
    void PassStringRef2([MarshalAs(UnmanagedType.BStr)] ref string s);
    void PassStringRef3([MarshalAs(UnmanagedType.LPStr)] ref string s);
    void PassStringRef4([MarshalAs(UnmanagedType.LPWStr)] ref string s);
}
```

```vb
Public Interface IStringWorker
    Sub PassString1(s As String)
    Sub PassString2(<MarshalAs(UnmanagedType.BStr)> s As String)
    Sub PassString3(<MarshalAs(UnmanagedType.LPStr)> s As String)
    Sub PassString4(<MarshalAs(UnmanagedType.LPWStr)> s As String)
    Sub PassStringRef1(ByRef s As String)
    Sub PassStringRef2(<MarshalAs(UnmanagedType.BStr)> ByRef s As String)
    Sub PassStringRef3(<MarshalAs(UnmanagedType.LPStr)> ByRef s As String)
    Sub PassStringRef4(<MarshalAs(UnmanagedType.LPWStr)> ByRef s As String)
End Interface
```

以下示例演示类型库中描述的相应接口。

```cpp
interface IStringWorker : IDispatch
{
    HRESULT PassString1([in] BSTR s);
    HRESULT PassString2([in] BSTR s);
    HRESULT PassString3([in] LPStr s);
    HRESULT PassString4([in] LPWStr s);
    HRESULT PassStringRef1([in, out] BSTR *s);
    HRESULT PassStringRef2([in, out] BSTR *s);
    HRESULT PassStringRef3([in, out] LPStr *s);
    HRESULT PassStringRef4([in, out] LPWStr *s);
};
```

## <a name="strings-used-in-platform-invoke"></a>在平台调用中使用的字符串

平台调用复制字符串参数，将其从.NET Framework 格式 (Unicode) 转换为平台非托管格式。 当调用返回时，字符串固定不变，且不会从非托管内存复制回托管内存。

下表列出以平台调用的方法参数封送字符串时的封送处理选项。 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 特性向封送字符串提供若干个 <xref:System.Runtime.InteropServices.UnmanagedType> 枚举值。

|枚举类型|非托管格式说明|
|----------------------|-------------------------------------|
|`UnmanagedType.AnsiBStr`|具有预先固定长度和 ANSI 字符的 COM 样式 `BSTR`。|
|`UnmanagedType.BStr`|具有预先固定长度和 Unicode 字符的 COM 样式 `BSTR`。|
|`UnmanagedType.LPStr`（默认值）|指向以 null 终止的 ANSI 字符数组的指针。|
|`UnmanagedType.LPTStr`|指向以 null 终止的平台相关字符数组的指针。|
|`UnmanagedType.LPUTF8Str`|指向以 null 终止的 UTF-8 编码字符数组的指针。|
|`UnmanagedType.LPWStr`|指向以 null 终止的 Unicode 字符数组的指针。|
|`UnmanagedType.TBStr`|具有预先固定长度和平台相关字符的 COM 样式 `BSTR`。|
|`VBByRefStr`|使 Visual Basic .NET 能够在非托管代码中更改字符串，并在托管代码中反映出结果的值。 此值仅支持用于平台调用。 这是 `ByVal` 字符串在 Visual Basic 中的默认值。|

此表适用于 <xref:System.String>。 对于 <xref:System.Text.StringBuilder>，仅允许使用 `LPStr`、`LPTStr` 和 `LPWStr` 三个选项。

以下类型定义演示 `MarshalAsAttribute` 平台调用的正确用法。

```csharp
class StringLibAPI
{
    [DllImport("StringLib.dll")]
    public static extern void PassLPStr([MarshalAs(UnmanagedType.LPStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassLPWStr([MarshalAs(UnmanagedType.LPWStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassLPTStr([MarshalAs(UnmanagedType.LPTStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassLPUTF8Str([MarshalAs(UnmanagedType.LPUTF8Str)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassBStr([MarshalAs(UnmanagedType.BStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassAnsiBStr([MarshalAs(UnmanagedType.AnsiBStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassTBStr([MarshalAs(UnmanagedType.TBStr)] string s);
}
```

```vb
Class StringLibAPI
    Public Declare Auto Sub PassLPStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.LPStr)> s As String)
    Public Declare Auto Sub PassLPWStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.LPWStr)> s As String)
    Public Declare Auto Sub PassLPTStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.LPTStr)> s As String)
    Public Declare Auto Sub PassLPUTF8Str Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.LPUTF8Str)> s As String)
    Public Declare Auto Sub PassBStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.BStr)> s As String)
    Public Declare Auto Sub PassAnsiBStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.AnsiBStr)> s As String)
    Public Declare Auto Sub PassTBStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.TBStr)> s As String)
End Class
```

## <a name="strings-used-in-structures"></a>在结构中使用的字符串

字符串是结构的有效成员，但是 <xref:System.Text.StringBuilder> 缓冲区在结构中无效。 下表显示以字段形式封送 <xref:System.String> 数据类型时的封送选项。 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 特性向字段的封送字符串提供若干个 <xref:System.Runtime.InteropServices.UnmanagedType> 枚举值。

|枚举类型|非托管格式说明|
|----------------------|-------------------------------------|
|`UnmanagedType.BStr`|具有预先固定长度和 Unicode 字符的 COM 样式 `BSTR`。|
|`UnmanagedType.LPStr`（默认值）|指向以 null 终止的 ANSI 字符数组的指针。|
|`UnmanagedType.LPTStr`|指向以 null 终止的平台相关字符数组的指针。|
|`UnmanagedType.LPUTF8Str`|指向以 null 终止的 UTF-8 编码字符数组的指针。|
|`UnmanagedType.LPWStr`|指向以 null 终止的 Unicode 字符数组的指针。|
|`UnmanagedType.ByValTStr`|定长字符数组；数组的类型由包含结构的字符集确定。|

`ByValTStr` 类型用于结构中出现的内联定长字符数组。 其他类型适用于包含指向字符串的指针的结构中包含的字符串引用。

应用于包含结构的 <xref:System.Runtime.InteropServices.StructLayoutAttribute> 的 `CharSet` 参数确定结构中字符串的字符格式。 以下示例结构包含字符串引用和内联字符串，以及 ANSI、Unicode 和平台相关字符。 这些结构在类型库中的表示形式如下面的 C++ 代码所示：

```cpp
struct StringInfoA
{
    char *  f1;
    char    f2[256];
};

struct StringInfoW
{
    WCHAR * f1;
    WCHAR   f2[256];
    BSTR    f3;
};

struct StringInfoT
{
    TCHAR * f1;
    TCHAR   f2[256];
};
```

以下示例演示如何使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 以不同的格式定义相同的结构。

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
struct StringInfoA
{
    [MarshalAs(UnmanagedType.LPStr)] public string f1;
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 256)] public string f2;
}

[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
struct StringInfoW
{
    [MarshalAs(UnmanagedType.LPWStr)] public string f1;
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 256)] public string f2;
    [MarshalAs(UnmanagedType.BStr)] public string f3;
}

[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Auto)]
struct StringInfoT
{
    [MarshalAs(UnmanagedType.LPTStr)] public string f1;
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 256)] public string f2;
}
```

```vb
<StructLayout(LayoutKind.Sequential, CharSet := CharSet.Ansi)> _
Structure StringInfoA
    <MarshalAs(UnmanagedType.LPStr)> Public f1 As String
    <MarshalAs(UnmanagedType.ByValTStr, SizeConst := 256)> _
    Public f2 As String
End Structure

<StructLayout(LayoutKind.Sequential, CharSet := CharSet.Unicode)> _
Structure StringInfoW
    <MarshalAs(UnmanagedType.LPWStr)> Public f1 As String
    <MarshalAs(UnmanagedType.ByValTStr, SizeConst := 256)> _
    Public f2 As String
<MarshalAs(UnmanagedType.BStr)> Public f3 As String
End Structure

<StructLayout(LayoutKind.Sequential, CharSet := CharSet.Auto)> _
Structure StringInfoT
    <MarshalAs(UnmanagedType.LPTStr)> Public f1 As String
    <MarshalAs(UnmanagedType.ByValTStr, SizeConst := 256)> _
    Public f2 As String
End Structure
```

## <a name="fixed-length-string-buffers"></a>定长字符串缓冲区

在某些情况下，必须将定长字符缓冲区传递到要操作的非托管代码中。 在这种情况下，只传递字符串将不起作用，因为被调用方不能修改所传递缓冲区的内容。 即使通过引用传递字符串，仍无法将缓冲区初始化为给定大小。

解决方法是将 <xref:System.Text.StringBuilder> 缓冲区作为参数（而不是 <xref:System.String>）传递。 被调用方可以取消引用和修改 `StringBuilder`，前提是不超过 `StringBuilder` 的容量。 还可以初始化为固定长度。 例如，如果将 `StringBuilder` 缓冲区初始化为 `N` 容量，则封送处理程序提供大小为 (`N`+ 1) 个字符的缓冲区。 +1 解释了非托管字符串具有 null 终止符而帐户 `StringBuilder` 却不具有这一事实。

例如，Windows [`GetWindowText`](/windows/desktop/api/winuser/nf-winuser-getwindowtextw) API 函数（在 *winuser.h* 中定义）要求调用方传递函数向其中写入窗口文本的固定长度字符缓冲区。 `LpString` 指向调用方分配的大小为 `nMaxCount` 的缓冲区。 调用方应分配缓冲区，并将 `nMaxCount` 自变量设置为已分配缓冲区的大小。 以下示例显示 `GetWindowText` 函数声明，如 *winuser.h* 中所定义。

```cpp
int GetWindowText(
    HWND hWnd,        // Handle to window or control.
    LPTStr lpString,  // Text buffer.
    int nMaxCount     // Maximum number of characters to copy.
);
```

被调用方可以取消引用和修改 `StringBuilder`，前提是不超过 `StringBuilder` 的容量。 下面的代码示例演示如何将 `StringBuilder` 初始化为固定长度。

```csharp
using System;
using System.Runtime.InteropServices;
using System.Text;

internal static class NativeMethods
{
    [DllImport("User32.dll")]
    internal static extern void GetWindowText(IntPtr hWnd, StringBuilder lpString, int nMaxCount);
}

public class Window
{
    internal IntPtr h;        // Internal handle to Window.
    public String GetText()
    {
        StringBuilder sb = new StringBuilder(256);
        NativeMethods.GetWindowText(h, sb, sb.Capacity + 1);
        return sb.ToString();
    }
}
```

```vb
Imports System.Text

Friend Class NativeMethods
    Friend Declare Auto Sub GetWindowText Lib "User32.dll" _
        (hWnd As IntPtr, lpString As StringBuilder, nMaxCount As Integer)
End Class

Public Class Window
    Friend h As IntPtr ' Friend handle to Window.
    Public Function GetText() As String
        Dim sb As New StringBuilder(256)
        NativeMethods.GetWindowText(h, sb, sb.Capacity + 1)
        Return sb.ToString()
   End Function
End Class
```

## <a name="see-also"></a>请参阅

- [默认封送处理行为](default-marshaling-behavior.md)
- [封送处理字符串](marshaling-strings.md)
- [可直接复制到本机结构中的类型和非直接复制到本机结构中的类型](blittable-and-non-blittable-types.md)
- [方向特性](/previous-versions/dotnet/netframework-4.0/77e6taeh(v=vs.100))
- [复制和锁定](copying-and-pinning.md)
