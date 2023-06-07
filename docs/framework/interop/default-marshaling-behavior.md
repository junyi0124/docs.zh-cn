---
title: 默认封送处理行为
description: 了解 .NET 中的默认封送处理行为。 查看使用互操作封送处理进行内存管理，并查看类、委托和值类型的默认封送处理。
ms.date: 06/26/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- interop marshaling, default
- interoperation with unmanaged code, marshaling
- marshaling behavior
ms.assetid: c0a9bcdf-3df8-4db3-b1b6-abbdb2af809a
ms.openlocfilehash: 3e18bb5c4caa43a8e951eed3fc6992ec1b2d2afb
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96256647"
---
# <a name="default-marshaling-behavior"></a>默认封送处理行为

互操作封送处理根据规则进行操作，该规则指定与方法参数相关联的数据在托管和非托管内存之间传递时的行为方式。 这些内置规则控制诸如此类的封送处理活动：数据类型转换、被调用方是否可以更改传递给它的数据并将这些更改返回给调用方以及在何种情况下封送拆收器提供性能优化。  
  
 本部分确定互操作封送处理服务的默认行为特征。 它提供有关封送处理数组、布尔值类型、char 类型、委托、类、对象、字符串和结构的详细信息。  
  
> [!NOTE]
> 不支持泛型类型的封送处理。 有关详细信息，请参阅[使用泛型类型进行交互操作](/previous-versions/dotnet/netframework-4.0/ms229590(v=vs.100))。  
  
## <a name="memory-management-with-the-interop-marshaler"></a>使用互操作封送拆收器进行内存管理  

 互操作封送拆收器始终尝试释放由非托管代码分配的内存。 此行为符合 COM 内存管理规则，但不同于用于管理本机 C++ 的规则。  
  
 使用为指针自动释放内存的平台调用时，如果你预期有本机 C++ 行为（无内存释放），则可能产生混淆。 例如，从 C++ DLL 调用以下非托管方法不会自动释放任何内存。  
  
### <a name="unmanaged-signature"></a>非托管的签名  
  
```cpp  
BSTR MethodOne (BSTR b) {  
     return b;  
}  
```  
  
 但是，如果将方法定义为平台调用原型，将每个 BSTR 类型替换为 <xref:System.String> 类型并调用 `MethodOne`，则公共语言运行时会尝试释放 `b` 两次。 可使用 <xref:System.IntPtr> 类型而不是字符串类型来更改封送处理行为。  
  
 运行时始终使用 CoTaskMemFree 方法来释放内存。 如果正在使用的内存未通过 **CoTaskMemAlloc** 方法分配，则必须使用 **IntPtr** 并通过适当的方法手动释放内存。 同样，可在永不应释放内存的情况下避免自动释放内存，例如，从 kernel32.dll（它将指针返回内核内存）使用 GetCommandLine 函数时。 有关手动释放内存的详细信息，请参阅[缓冲区示例](/previous-versions/dotnet/netframework-4.0/x3txb6xc(v=vs.100))。  
  
## <a name="default-marshaling-for-classes"></a>类的默认封送处理  

 类仅能由 COM 互操作封送，并总是作为接口封送。 在某些情况下用来将该类封送的接口称为类接口。 有关使用所选接口替代类接口的信息，请参阅[类接口简介](../../standard/native-interop/com-callable-wrapper.md#introducing-the-class-interface)。  
  
### <a name="passing-classes-to-com"></a>向 COM 传递类  

 当托管类传递给 COM 时，互操作封送拆收器自动使用 COM 代理包装类，并将由代理所生成的类接口传递到 COM 方法调用。 然后，代理委托对类接口的所有调用返回托管对象。 代理还公开其他不由类显式实现的接口。 代理代表类自动实现接口，如 IUnknown 和 IDispatch 。  
  
### <a name="passing-classes-to-net-code"></a>向 .NET 代码传递类  

 组件类通常不用作 COM 中的方法参数。 而是通常以默认界接口代替组件类进行传递。  
  
 当接口传递到托管代码中时，互操作封送拆收器负责用适当的包装来包装接口，并将该包装传递给托管方法。 确定要使用的包装可能会很困难。 COM 对象的每个实例都具有一个唯一的包装，无论该对象实现多少个接口。 例如，实现五个不同接口的单个 COM 对象只有一个包装。 同一个包装公开所有五个接口。 如果创建了两个 COM 对象的实例，则创建两个包装的实例。  
  
 对于在生存期内保持相同类型的包装，在由对象公开的接口第一次通过封送拆收器时，互操作封送拆收器必须识别正确的包装。 封送拆收器通过查看该对象实现的一个接口来识别对象。  
  
 例如，封送拆收器确定应使用类包装来包装已传递到托管代码的接口。 当接口第一次通过封送拆收器时，封送拆收器检查该接口是否来自已知的对象。 在两种情况中会发生此检查行为：  
  
- 一个接口正在由另一个传递到 COM 其他位置的托管对象实现。 封送拆收器可轻易识别由托管对象公开的接口，并能够将接口与提供实现的托管对象相匹配。 接着托管对象被传递给该方法且不需要包装。  
  
- 已包装的对象将实现该接口。 要确定是否就是这种情况，封送拆收器向该对象查询其 IUnknown 接口，并将返回的接口与其他已包装对象的接口进行比较。 如果该接口与另一包装的接口相同，则这些对象具有相同的标识，并且现有的包装会被传递给该方法。  
  
 如果接口不是来自已知对象，则封送拆收器执行以下操作：  
  
1. 封送拆收器向对象查询 IProvideClassInfo2 接口。 封送拆收器使用从 IProvideClassInfo2.GetGUID 返回的 CLSID（如果已提供）来识别提供接口的组件类。 如果以前注册过程序集，通过 CLSID，封送拆收器可以从注册表定位包装。  
  
2. 封送拆收器向接口查询 IProvideClassInfo 接口。 封送拆收器使用从 IProvideClassInfo.GetClassinfo 返回 的 ITypeInfo（如果已提供）来确定公开该接口的类的 CLSID 。 封送拆收器可以使用 CLSID 定位包装的元数据。  
  
3. 如果封送拆收器仍不能识别类，则使用名为 System.__ComObject 的泛型包装类包装接口。  
  
## <a name="default-marshaling-for-delegates"></a>委托的默认封送处理  

 托管委托基于后述调用机制封送为 COM 接口或函数指针：  
  
- 对于平台调用，默认情况下，委托作为非托管函数指针进行封送。  
  
- 对于 COM 互操作，默认情况下，委托作为 _Delegate 类型的 COM 接口进行封送。 在 Mscorlib.tlb 类型库中定义 **_Delegate** 接口且该接口包含 <xref:System.Delegate.DynamicInvoke%2A?displayProperty=nameWithType> 方法，使你可以调用该委托引用的方法。  
  
 下表显示托管委托数据类型的封送处理选项。 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性提供若干 <xref:System.Runtime.InteropServices.UnmanagedType> 枚举值来封送委托。  
  
|枚举类型|非托管格式说明|  
|----------------------|-------------------------------------|  
|UnmanagedType.FunctionPtr|非托管函数指针。|  
|UnmanagedType.Interface|在 Mscorlib.tlb 中定义的 _Delegate 类型的接口。|  
  
 请考虑下面的示例代码，其中将 `DelegateTestInterface` 的方法导出到 COM 类型库。 请注意，只有标记为 ref（或 ByRef）关键字的委托作为 In/Out 参数传递 。  
  
```csharp  
using System;  
using System.Runtime.InteropServices;  
  
public interface DelegateTest {  
void m1(Delegate d);  
void m2([MarshalAs(UnmanagedType.Interface)] Delegate d);
void m3([MarshalAs(UnmanagedType.Interface)] ref Delegate d);
void m4([MarshalAs(UnmanagedType.FunctionPtr)] Delegate d);
void m5([MarshalAs(UnmanagedType.FunctionPtr)] ref Delegate d);
}  
```  
  
### <a name="type-library-representation"></a>类型库表示形式  
  
```cpp  
importlib("mscorlib.tlb");  
interface DelegateTest : IDispatch {  
[id(…)] HRESULT m1([in] _Delegate* d);  
[id(…)] HRESULT m2([in] _Delegate* d);  
[id(…)] HRESULT m3([in, out] _Delegate** d);  
[id()] HRESULT m4([in] int d);  
[id()] HRESULT m5([in, out] int *d);  
   };  
```  
  
 可以取消引用函数指针，就像可以取消引用任何其他非托管函数指针一样。  

在此示例中，当两个委托作为 <xref:System.Runtime.InteropServices.UnmanagedType.FunctionPtr?displayProperty=nameWithType> 封送时，得到的结果是一个 `int` 和一个指向 `int` 的指针。 由于要封送委托类型，此处 `int` 表示指向 void (`void*`) 的指针，这是该委托在内存中的地址。 也就是说，此结果是针对 32 位 Windows 系统的，因为此处 `int` 表示的是函数指针大小。

> [!NOTE]
> 对指向由非托管代码持有的托管委托的函数指针的引用不会阻止公共语言运行时对托管对象执行垃圾收集。  
  
 例如，下面的代码是错误的，因为对传递给 `SetChangeHandler` 方法的 `cb` 对象的引用不会使 `cb` 在超出 `Test` 方法的生存期时仍保持活动。 一旦对 `cb` 对象进行垃圾收集，传递给 `SetChangeHandler` 的函数指针将不再有效。  
  
```csharp  
public class ExternalAPI {  
   [DllImport("External.dll")]  
   public static extern void SetChangeHandler(  
      [MarshalAs(UnmanagedType.FunctionPtr)]ChangeDelegate d);  
}  
public delegate bool ChangeDelegate([MarshalAs(UnmanagedType.LPWStr) string S);  
public class CallBackClass {  
   public bool OnChange(string S){ return true;}  
}  
internal class DelegateTest {  
   public static void Test() {  
      CallBackClass cb = new CallBackClass();  
      // Caution: The following reference on the cb object does not keep the
      // object from being garbage collected after the Main method
      // executes.  
      ExternalAPI.SetChangeHandler(new ChangeDelegate(cb.OnChange));
   }  
}  
```  
  
 若要补偿意外的垃圾回收，只要非托管的函数指针在使用中时，调用方必须确保 `cb` 对象保持活动状态。 可以在不再需要函数指针时，按以下示例所示让非托管代码通知托管代码（可选）。  
  
```csharp  
internal class DelegateTest {  
   CallBackClass cb;  
   // Called before ever using the callback function.  
   public static void SetChangeHandler() {  
      cb = new CallBackClass();  
      ExternalAPI.SetChangeHandler(new ChangeDelegate(cb.OnChange));  
   }  
   // Called after using the callback function for the last time.  
   public static void RemoveChangeHandler() {  
      // The cb object can be collected now. The unmanaged code is
      // finished with the callback function.  
      cb = null;  
   }  
}  
```  
  
## <a name="default-marshaling-for-value-types"></a>值类型的默认封送处理  

 大多数值类型（如整数和浮点数）是 [blittable](blittable-and-non-blittable-types.md)类型，不需要封送处理。 其他[非 blittable](blittable-and-non-blittable-types.md) 类型在托管和非托管内存中具有不同的表示形式，且需要封送处理。 其他类型仍需要跨互操作边界的显式格式设置。  
  
 本部分提供有关以下格式化值类型的信息：  
  
- [在平台调用中使用的值类型](#value-types-used-in-platform-invoke)  
  
- [在 COM 互操作中使用的值类型](#value-types-used-in-com-interop)  
  
 除了介绍格式化的类型，本主题识别具有异常封送处理行为的[系统值类型](#system-value-types)。  
  
 格式化的类型是复杂类型，其中包含显式控制其成员在内存中的布局的信息。 使用 <xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性提供成员布局信息。 布局可以是以下 <xref:System.Runtime.InteropServices.LayoutKind> 枚举值之一：  
  
- LayoutKind.Auto  
  
     指示公共语言运行时可以自由重新排序类型的成员以提高效率。 但是，当值类型传递到非托管代码中时，成员的布局是可预测的。 尝试将这种结构进行自动封送处理会导致异常。  
  
- LayoutKind.Sequential  
  
     指示在非托管内存内布局该类型的成员，其顺序与其在托管类型定义中出现的顺序相同。  
  
- LayoutKind.Explicit  
  
     指示根据随每个字段提供的 <xref:System.Runtime.InteropServices.FieldOffsetAttribute> 对成员进行布局。  
  
### <a name="value-types-used-in-platform-invoke"></a>在平台调用中使用的值类型  

 在以下示例中，`Point` 和 `Rect` 类型使用 StructLayoutAttribute 提供成员布局信息。  
  
```vb  
Imports System.Runtime.InteropServices  
<StructLayout(LayoutKind.Sequential)> Public Structure Point  
   Public x As Integer  
   Public y As Integer  
End Structure  
<StructLayout(LayoutKind.Explicit)> Public Structure Rect  
   <FieldOffset(0)> Public left As Integer  
   <FieldOffset(4)> Public top As Integer  
   <FieldOffset(8)> Public right As Integer  
   <FieldOffset(12)> Public bottom As Integer  
End Structure  
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
```  
  
 当封送到非托管代码时，这些格式化的类型作为 C 样式结构进行封送。 这为调用具有结构自变量的非托管 API 提供一种简单的方法。 例如，`POINT` 和 `RECT` 结构可按下述方式传递到 Microsoft Windows API“PtInRect”函数：  
  
```cpp  
BOOL PtInRect(const RECT *lprc, POINT pt);  
```  
  
 可以使用下面的平台调用定义传递结构：  
  
```vb
Friend Class NativeMethods
    Friend Declare Auto Function PtInRect Lib "User32.dll" (
        ByRef r As Rect, p As Point) As Boolean
End Class
```
  
```csharp
internal static class NativeMethods
{
   [DllImport("User32.dll")]
   internal static extern bool PtInRect(ref Rect r, Point p);
}
```
  
 必须由引用传递 `Rect` 值类型，因为非托管 API 预期有一个指向 `RECT` 的指针被传递给函数。 由值传递 `Point` 值类型，因为非托管的 API 需要在堆栈上传递 `POINT`。 此细微差别非常重要。 引用作为指针传递到非托管代码。 值传递到堆栈上的非托管代码。  
  
> [!NOTE]
> 当格式化的类型作为结构进行封送处理时，仅可访问类型中的字段。 如果该类型具有方法、属性或事件，无法从非托管代码对它们进行访问。  
  
 类也可以作为 C 样式结构封送到非托管代码，只要它们具有固定成员布局。 类的成员布局信息也通过 <xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性提供。 具有固定布局的值类型与具有固定布局的类之间的主要区别在于它们被封送到非托管代码的方式。 由值（在堆栈上）传递值类型，由此，调用方看不到任何由被调用方对该类型的成员所做的更改。 由引用（在堆栈上传递对该类型的引用）传递引用类型；由此，调用方将看到被调用方对该类型的可直接复制到本机结构中的类型成员所做的所有更改。  
  
> [!NOTE]
> 如果引用类型具有非直接复制到本机结构中的类型成员，则需要进行两次转换：第一次是当参数传递到非托管端时，第二次是从调用返回时。 由于此增加的开销，如果调用方想要查看被调用方所做的更改，必须将输入/输出参数显式应用到某个参数。  
  
 在以下示例中，`SystemTime` 类具有顺序成员布局，并且可以被传递给 Windows API GetSystemTime 函数。  
  
```vb  
<StructLayout(LayoutKind.Sequential)> Public Class SystemTime  
   Public wYear As System.UInt16  
   Public wMonth As System.UInt16  
   Public wDayOfWeek As System.UInt16  
   Public wDay As System.UInt16  
   Public wHour As System.UInt16  
   Public wMinute As System.UInt16  
   Public wSecond As System.UInt16  
   Public wMilliseconds As System.UInt16  
End Class  
```  
  
```csharp  
[StructLayout(LayoutKind.Sequential)]  
   public class SystemTime {  
   public ushort wYear;
   public ushort wMonth;  
   public ushort wDayOfWeek;
   public ushort wDay;
   public ushort wHour;
   public ushort wMinute;
   public ushort wSecond;
   public ushort wMilliseconds;
}  
```  
  
 GetSystemTime 函数定义如下：  
  
```cpp  
void GetSystemTime(SYSTEMTIME* SystemTime);  
```  
  
 GetSystemTime 的等效平台调用定义如下：  
  
```vb
Friend Class NativeMethods
    Friend Declare Auto Sub GetSystemTime Lib "Kernel32.dll" (
        ByVal sysTime As SystemTime)
End Class
```
  
```csharp
internal static class NativeMethods
{
   [DllImport("Kernel32.dll", CharSet = CharSet.Auto)]
   internal static extern void GetSystemTime(SystemTime st);
}
```
  
 请注意，`SystemTime` 参数并不类型化为引用参数，因为 `SystemTime` 是一个类，而不是值类型。 与值类型不同，类始终由引用传递。  
  
 下面的代码示例显示一个不同的 `Point` 类，此类具有一个名为 `SetXY` 的方法。 因为该类型具有顺序布局，因此可以传递到非托管代码并作为一种结构封送。 但是，`SetXY` 成员不能从非托管代码中调用，即使由引用传递对象。  
  
```vb  
<StructLayout(LayoutKind.Sequential)> Public Class Point  
   Private x, y As Integer  
   Public Sub SetXY(x As Integer, y As Integer)  
      Me.x = x  
      Me.y = y  
   End Sub  
End Class  
```  
  
```csharp  
[StructLayout(LayoutKind.Sequential)]  
public class Point {  
   int x, y;  
   public void SetXY(int x, int y){
      this.x = x;  
      this.y = y;  
   }  
}  
```  
  
### <a name="value-types-used-in-com-interop"></a>在 COM 互操作中使用的值类型  

 格式化的类型也可传递给 COM 互操作方法调用。 实际上，当导出到类型库时，值类型就自动转换为结构。 如下面的示例所示，`Point` 值类型变为类型定义 (typedef)，名称为 `Point`。 在类型库中其他位置对 `Point` 值类型的所有引用都替换为 `Point` typedef。  
  
 **类型库表示形式**  
  
```cpp  
typedef struct tagPoint {  
   int x;  
   int y;  
} Point;  
interface _Graphics {  
   …  
   HRESULT SetPoint ([in] Point p)  
   HRESULT SetPointRef ([in,out] Point *p)  
   HRESULT GetPoint ([out,retval] Point *p)  
}  
```  
  
 当通过 COM 接口进行封送处理时，使用用于封送值和封送对平台调用的调用的引用的规则。 例如，当 `Point` 值类型的实例从 .NET Framework 传递到 COM 时，则由值传递 `Point`。 如果 `Point` 值类型由引用传递，则在堆栈上传递指向 `Point` 的指针。 互操作封送拆收器不支持任一方向更高级别的间接寻址 (Point \*\*)。  
  
> [!NOTE]
> 将 <xref:System.Runtime.InteropServices.LayoutKind> 枚举值设置为显式的结构无法用于 COM 互操作，因为导出的类型库不能表达显式布局。  
  
### <a name="system-value-types"></a>系统值类型  

 <xref:System> 命名空间具有多个表示运行时原始类型装箱形式的值类型。 例如，值类型 <xref:System.Int32?displayProperty=nameWithType> 结构表示 ELEMENT_TYPE_I4 的装箱形式。 不像其他格式化类型将这些类型作为结构进行封送处理，而是以它们装箱的原始类型的相同方式将它们封送处理。 因此，System.Int32 作为 ELEMENT_TYPE_I4 封送处理，而不是作为包含长类型的单个成员的结构封送处理  。 下表包含系统命名空间中的值类型列表，这些值类型是基元类型的装箱表示形式。  
  
|系统值类型|元素类型|  
|-----------------------|------------------|  
|<xref:System.Boolean?displayProperty=nameWithType>|ELEMENT_TYPE_BOOLEAN|  
|<xref:System.SByte?displayProperty=nameWithType>|ELEMENT_TYPE_I1|  
|<xref:System.Byte?displayProperty=nameWithType>|ELEMENT_TYPE_UI1|  
|<xref:System.Char?displayProperty=nameWithType>|ELEMENT_TYPE_CHAR|  
|<xref:System.Int16?displayProperty=nameWithType>|ELEMENT_TYPE_I2|  
|<xref:System.UInt16?displayProperty=nameWithType>|ELEMENT_TYPE_U2|  
|<xref:System.Int32?displayProperty=nameWithType>|ELEMENT_TYPE_I4|  
|<xref:System.UInt32?displayProperty=nameWithType>|ELEMENT_TYPE_U4|  
|<xref:System.Int64?displayProperty=nameWithType>|ELEMENT_TYPE_I8|  
|<xref:System.UInt64?displayProperty=nameWithType>|ELEMENT_TYPE_U8|  
|<xref:System.Single?displayProperty=nameWithType>|ELEMENT_TYPE_R4|  
|<xref:System.Double?displayProperty=nameWithType>|ELEMENT_TYPE_R8|  
|<xref:System.String?displayProperty=nameWithType>|ELEMENT_TYPE_STRING|  
|<xref:System.IntPtr?displayProperty=nameWithType>|ELEMENT_TYPE_I|  
|<xref:System.UIntPtr?displayProperty=nameWithType>|ELEMENT_TYPE_U|  
  
 系统命名空间中一些其他值类型的处理方式则不同。 由于非托管代码已具备这些类型的完善格式，因此封送拆收器具有用于将其封送的特殊规则。 下表列出系统命名空间中的特殊值类型，以及其封送到的非托管类型。  
  
|系统值类型|IDL 类型|  
|-----------------------|--------------|  
|<xref:System.DateTime?displayProperty=nameWithType>|DATE|  
|<xref:System.Decimal?displayProperty=nameWithType>|DECIMAL|  
|<xref:System.Guid?displayProperty=nameWithType>|**GUID**|  
|<xref:System.Drawing.Color?displayProperty=nameWithType>|OLE_COLOR|  
  
 下面的代码显示 Stdole2 类型库中非托管类型 DATE、GUID、DECIMAL 和 OLE_COLOR 的定义   。  
  
#### <a name="type-library-representation"></a>类型库表示形式  
  
```cpp  
typedef double DATE;  
typedef DWORD OLE_COLOR;  
  
typedef struct tagDEC {  
    USHORT    wReserved;  
    BYTE      scale;  
    BYTE      sign;  
    ULONG     Hi32;  
    ULONGLONG Lo64;  
} DECIMAL;  
  
typedef struct tagGUID {  
    DWORD Data1;  
    WORD  Data2;  
    WORD  Data3;  
    BYTE  Data4[ 8 ];  
} GUID;  
```  
  
 下面的代码显示托管 `IValueTypes` 接口中的相应定义。  
  
```vb  
Public Interface IValueTypes  
   Sub M1(d As System.DateTime)  
   Sub M2(d As System.Guid)  
   Sub M3(d As System.Decimal)  
   Sub M4(d As System.Drawing.Color)  
End Interface  
```  
  
```csharp  
public interface IValueTypes {  
   void M1(System.DateTime d);  
   void M2(System.Guid d);  
   void M3(System.Decimal d);  
   void M4(System.Drawing.Color d);  
}  
```  
  
#### <a name="type-library-representation"></a>类型库表示形式  
  
```cpp  
[…]  
interface IValueTypes : IDispatch {  
   HRESULT M1([in] DATE d);  
   HRESULT M2([in] GUID d);  
   HRESULT M3([in] DECIMAL d);  
   HRESULT M4([in] OLE_COLOR d);  
};  
```  
  
## <a name="see-also"></a>请参阅

- [可直接复制到本机结构中的类型和非直接复制到本机结构中的类型](blittable-and-non-blittable-types.md)
- [复制和锁定](copying-and-pinning.md)
- [数组的默认封送处理](default-marshaling-for-arrays.md)
- [对象的默认封送处理](default-marshaling-for-objects.md)
- [字符串的默认封送处理](default-marshaling-for-strings.md)
