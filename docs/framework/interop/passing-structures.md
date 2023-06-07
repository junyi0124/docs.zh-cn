---
title: 传递结构
description: 了解如何将结构和类传递到非托管函数。 了解用于定义格式化类型的 StructLayoutAttribute 属性。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- platform invoke, calling unmanaged functions
ms.assetid: 9b92ac73-32b7-4e1b-862e-6d8d950cf169
ms.openlocfilehash: ece5db8fdf803ce2f450ebeaaad66a379cfbf992
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96268906"
---
# <a name="passing-structures"></a>传递结构

许多未托管的函数希望你以函数参数的形式传递结构成员（Visual Basic 中用户定义的类型），或托管代码中定义的类的成员。 使用平台调用将结构或类传递给非托管代码时，必须提供其他信息以保留原始布局和对齐方式。 本主题介绍用于定义格式化类型的 <xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性。 对于托管结构和类，可从 LayoutKind 枚举提供的几种可预测布局行为中进行选择。  
  
 本主题提出的概念核心是结构和类类型之间的重要区别。 结构是值类型，类是引用类型 - 类始终提供至少一个级别的内存间接（指向值的指针）。 这种差异很重要，因为未托管的函数通常要求间接，如下表第一列中的签名所示。 其余列中的托管结构和类声明显示可在声明中调整间接级别的程度。Visual Basic 和 Visual C# 均提供有声明。  
  
|非托管的签名|托管的声明： <br />非间接<br />`Structure MyType`<br />`struct MyType;`|托管的声明： <br />一级间接<br />`Class MyType`<br />`class MyType;`|  
|-------------------------|---------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|  
|`DoWork(MyType x);`<br /><br /> 要求零级间接。|`DoWork(ByVal x As MyType)` <br /> `DoWork(MyType x)`<br /><br /> 添加零级间接。|不可能，因为已存在一级间接。|  
|`DoWork(MyType* x);`<br /><br /> 要求一级间接。|`DoWork(ByRef x As MyType)` <br /> `DoWork(ref MyType x)`<br /><br /> 添加一级间接。|`DoWork(ByVal x As MyType)` <br /> `DoWork(MyType x)`<br /><br /> 添加零级间接。|  
|`DoWork(MyType** x);`<br /><br /> 要求二级间接。|不可能，因为不能使用 ByRef ByRef 或 `ref` `ref`。|`DoWork(ByRef x As MyType)` <br /> `DoWork(ref MyType x)`<br /><br /> 添加一级间接。|  
  
 该表介绍了以下平台调用声明的准则：  
  
- 当未托管的函数不要求间接时，使用由值传递的结构。  
  
- 当未托管的函数要求一级间接时，使用由引用传递的结构或由值传递的类。  
  
- 当未托管的函数要求二级间接时，使用由引用传递的类。  
  
## <a name="declaring-and-passing-structures"></a>声明和传递结构  

 以下示例演示了如何在托管代码中定义 `Point` 和 `Rect` 结构，并将类型作为参数传递给 User32.dll 文件中的 PtInRect 函数。 PtInRect 具有以下非托管签名：  
  
```cpp
BOOL PtInRect(const RECT *lprc, POINT pt);  
```  
  
 请注意，必须通过引用传递 Rect 结构，因为该函数需要指向 RECT 类型的指针。  
  
```vb  
Imports System.Runtime.InteropServices  
  
<StructLayout(LayoutKind.Sequential)> Public Structure Point  
    Public x As Integer  
    Public y As Integer  
End Structure  
  
Public Structure <StructLayout(LayoutKind.Explicit)> Rect  
    <FieldOffset(0)> Public left As Integer  
    <FieldOffset(4)> Public top As Integer  
    <FieldOffset(8)> Public right As Integer  
    <FieldOffset(12)> Public bottom As Integer  
End Structure  
  
Friend Class NativeMethods
    Friend Declare Auto Function PtInRect Lib "user32.dll" (
        ByRef r As Rect, p As Point) As Boolean  
End Class  
```  
  
```csharp  
using System.Runtime.InteropServices;  
  
[StructLayout(LayoutKind.Sequential)]  
public struct Point {  
    public int x;  
    public int y;  
}
  
[StructLayout(LayoutKind.Explicit)]  
public struct Rect {  
    [FieldOffset(0)] public int left;  
    [FieldOffset(4)] public int top;  
    [FieldOffset(8)] public int right;  
    [FieldOffset(12)] public int bottom;  
}
  
internal static class NativeMethods
{  
    [DllImport("User32.dll")]  
    internal static extern bool PtInRect(ref Rect r, Point p);  
}  
```  
  
## <a name="declaring-and-passing-classes"></a>声明和传递类  

 只要类具有固定成员布局，就可将类的成员传递给非托管 DLL 函数。 以下示例演示如何将按顺序定义的 `MySystemTime` 类的成员传递给 User32.dll 文件中的 GetSystemTime。 GetSystemTime 具有以下非托管签名：  
  
```cpp
void GetSystemTime(SYSTEMTIME* SystemTime);  
```  
  
 与值类型不同，类总是至少具有一级间接。  
  
```vb  
Imports System.Runtime.InteropServices  
  
<StructLayout(LayoutKind.Sequential)> Public Class MySystemTime  
    Public wYear As Short  
    Public wMonth As Short  
    Public wDayOfWeek As Short
    Public wDay As Short  
    Public wHour As Short  
    Public wMinute As Short  
    Public wSecond As Short  
    Public wMiliseconds As Short  
End Class  
  
Friend Class NativeMethods  
    Friend Declare Auto Sub GetSystemTime Lib "Kernel32.dll" (
        sysTime As MySystemTime)  
    Friend Declare Auto Function MessageBox Lib "User32.dll" (
        hWnd As IntPtr, lpText As String, lpCaption As String, uType As UInteger) As Integer  
End Class  
  
Public Class TestPlatformInvoke
    Public Shared Sub Main()  
        Dim sysTime As New MySystemTime()  
        NativeMethods.GetSystemTime(sysTime)  
  
        Dim dt As String  
        dt = "System time is:" & ControlChars.CrLf & _  
              "Year: " & sysTime.wYear & _  
              ControlChars.CrLf & "Month: " & sysTime.wMonth & _  
              ControlChars.CrLf & "DayOfWeek: " & sysTime.wDayOfWeek & _  
              ControlChars.CrLf & "Day: " & sysTime.wDay  
        NativeMethods.MessageBox(IntPtr.Zero, dt, "Platform Invoke Sample", 0)
    End Sub  
End Class  
```  
  
```csharp  
[StructLayout(LayoutKind.Sequential)]  
public class MySystemTime {  
    public ushort wYear;
    public ushort wMonth;  
    public ushort wDayOfWeek;
    public ushort wDay;
    public ushort wHour;
    public ushort wMinute;
    public ushort wSecond;
    public ushort wMilliseconds;
}  
internal static class NativeMethods
{  
    [DllImport("Kernel32.dll")]  
    internal static extern void GetSystemTime(MySystemTime st);  
  
    [DllImport("user32.dll", CharSet = CharSet.Auto)]  
    internal static extern int MessageBox(
        IntPtr hWnd, string lpText, string lpCaption, uint uType);  
}  
  
public class TestPlatformInvoke  
{  
    public static void Main()  
    {  
        MySystemTime sysTime = new MySystemTime();  
        NativeMethods.GetSystemTime(sysTime);  
  
        string dt;  
        dt = "System time is: \n" +  
              "Year: " + sysTime.wYear + "\n" +  
              "Month: " + sysTime.wMonth + "\n" +  
              "DayOfWeek: " + sysTime.wDayOfWeek + "\n" +  
              "Day: " + sysTime.wDay;  
        NativeMethods.MessageBox(IntPtr.Zero, dt, "Platform Invoke Sample", 0);  
    }  
}  
```  
  
## <a name="see-also"></a>请参阅

- [调用 DLL 函数](calling-a-dll-function.md)
- <xref:System.Runtime.InteropServices.StructLayoutAttribute>
- <xref:System.Runtime.InteropServices.FieldOffsetAttribute>
