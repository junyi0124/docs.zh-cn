---
title: 如何：实现回调函数
description: 请参阅如何实现回调函数。 在此示例中，使用平台调用的托管应用程序在计算机上打印每个窗口的句柄值。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- callback function, implementing
ms.assetid: e55b3712-b9ea-4453-bd9a-ad5cfa2f6bfa
ms.openlocfilehash: 9fa1d3b3ece334e109584f38ba69b796dfe24491
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96267047"
---
# <a name="how-to-implement-callback-functions"></a>如何：实现回调函数

下面的过程和示例演示使用平台调用的托管应用程序如何在本地计算机上打印每个窗口的句柄值。 具体而言，过程和示例使用“EnumWindows” 函数来逐句通过窗口列表，使用托管回调函数（名为 CallBack）来打印窗口句柄的值。  
  
### <a name="to-implement-a-callback-function"></a>实现回调函数的步骤  
  
1. 在进一步执行实现前，请查看“EnumWindows”函数的签名。 “EnumWindows”具有以下签名：  
  
    ```cpp
    BOOL EnumWindows(WNDENUMPROC lpEnumFunc, LPARAM lParam)
    ```
  
     此函数需要回调的线索之一是存在“lpEnumFunc”自变量。 经常可以看到在采用指向回调函数的指针的参数名称中“lp”（长指针）前缀与“Func”后缀结合在一起。 有关 Win32 函数的文档，请参阅 Microsoft Platform SDK。  
  
2. 创建托管回调函数。 此示例声明一个名为 `CallBack` 的委托类型，该类型采用两个自变量（“hwnd”和“lparam”）。 第一个参数是窗口的句柄；第二个参数是应用程序定义的。 在此版本中，这两个自变量都必须是整数。  
  
     回调函数通常返回非零值来指示成功，返回零值来指示失败。 此示例将返回值显式设置为“true”以继续进行枚举。  
  
3. 创建一个委托，并将其作为自变量传递到“EnumWindows”函数。 平台调用自动将该委托转换为常见的回调格式。  
  
4. 确保在回调函数完成其工作之前，垃圾回收器不会回收委托。 当将委托作为参数传递，或传递作为字段包括到结构中的委托时，在调用期间不会对其进行回收。 因此，正如下面的枚举示例一样，调用返回并不再需要托管调用方执行任何其他操作之前，回调函数完成其工作。  
  
     但是，如果调用返回后可以调用回调函数，托管调用方必须采取措施来确保委托在回调函数完成之前不会被回收。 有关防止垃圾回收的详细信息，请参阅使用平台调用的[互操作封送处理](interop-marshaling.md)。  
  
## <a name="example"></a>示例  
  
```vb  
Imports System  
Imports System.Runtime.InteropServices  
  
Public Delegate Function CallBack( _  
hwnd As Integer, lParam As Integer) As Boolean  
  
Public Class EnumReportApp  
  
    Declare Function EnumWindows Lib "user32" ( _  
       x As CallBack, y As Integer) As Integer  
  
    Public Shared Sub Main()  
        EnumWindows(AddressOf EnumReportApp.Report, 0)  
    End Sub 'Main  
  
    Public Shared Function Report(hwnd As Integer, lParam As Integer) _  
    As Boolean  
        Console.Write("Window handle is ")  
        Console.WriteLine(hwnd)  
        Return True  
    End Function 'Report  
End Class 'EnumReportApp  
```  
  
```csharp  
using System;  
using System.Runtime.InteropServices;  
  
public delegate bool CallBack(int hwnd, int lParam);  
  
public class EnumReportApp  
{  
    [DllImport("user32")]  
    public static extern int EnumWindows(CallBack x, int y);
  
    public static void Main()
    {  
        CallBack myCallBack = new CallBack(EnumReportApp.Report);  
        EnumWindows(myCallBack, 0);  
    }  
  
    public static bool Report(int hwnd, int lParam)  
    {
        Console.Write("Window handle is ");  
        Console.WriteLine(hwnd);  
        return true;  
    }  
}  
```  
  
```cpp  
using namespace System;  
using namespace System::Runtime::InteropServices;  
  
// A delegate type.  
delegate bool CallBack(int hwnd, int lParam);  
  
// Managed type with the method to call.  
ref class EnumReport  
{  
// Report the window handle.  
public:  
    [DllImport("user32")]  
    static int EnumWindows(CallBack^ x, int y);  
  
    static void Main()  
    {  
        EnumReport^ er = gcnew EnumReport;  
        CallBack^ myCallBack = gcnew CallBack(&EnumReport::Report);  
        EnumWindows(myCallBack, 0);  
    }  
  
    static bool Report(int hwnd, int lParam)  
    {  
       Console::Write(L"Window handle is ");  
       Console::WriteLine(hwnd);  
       return true;  
    }  
};  
  
int main()  
{  
    EnumReport::Main();  
}  
```  
  
## <a name="see-also"></a>请参阅

- [回调函数](callback-functions.md)
- [调用 DLL 函数](calling-a-dll-function.md)
