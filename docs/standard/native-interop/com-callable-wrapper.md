---
title: COM 可调用包装
description: COM 客户端调用 .NET 对象时，CLR 将创建托管对象和该对象的 COM 可调用包装器。 COM 客户端会调用该对象的包装器。
ms.date: 10/23/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- CCW
- COM interop, COM wrappers
- COM wrappers
- callable wrappers
- interoperation with unmanaged code, COM wrappers
- COM callable wrappers
ms.assetid: d04be3b5-27b9-4f5b-8469-a44149fabf78
ms.openlocfilehash: cc27ba47c88d424a80eb47aaa310bdfd6d18433a
ms.sourcegitcommit: 7588b1f16b7608bc6833c05f91ae670c22ef56f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2020
ms.locfileid: "93187913"
---
# <a name="com-callable-wrapper"></a>COM 可调用包装

COM 客户端调用 .NET 对象时，公共语言运行时将创建托管对象和该对象的 COM 可调用包装器 (CCW)。 无法直接引用 .NET 对象，COM 客户端使用 CCW 作为托管对象的代理。

无论请求其服务的 COM 客户端数量是多少，该运行时都只为托管对象创建恰好一个 CCW。 如下图所示，多个 COM 客户端可以保持对公开 INew 接口的 CCW 的引用。 反之，CCW 保持对实现该接口的托管对象的单一引用，并被垃圾回收。 COM 和 .NET 客户端可以同时对同一托管对象发出请求。

![多个 COM 客户端保留对公开 INew 的 CCW 的引用。](./media/com-callable-wrapper/com-callable-wrapper-clients.gif)

COM 可调用包装器对在 .NET 运行时内运行的其他类不可见。 它们的主要目的是封送托管和非托管代码之间的调用；但是，CCW 还托管它们包装的托管对象的对象标识和对象生存期。

## <a name="object-identity"></a>对象标识

运行时为其垃圾回收堆中的 .NET 对象分配内存，从而使运行时能根据需要在内存中移动对象。 与此相反，运行时为非回收堆中的 CCW 分配内存，使 COM 客户端直接引用包装器成为可能。

## <a name="object-lifetime"></a>对象生存期

与其包装的 .NET 客户端不同，CCW 在传统 COM 方式中会进行引用计数。 当 CCW 上的引用计数达到零时，包装器将释放其对托管对象的引用。 将在下一个垃圾回收周期期间收集无剩余引用的托管对象。

## <a name="simulating-com-interfaces"></a>模拟 COM 接口

CCW 公开所有公共的 COM 可见接口和数据类型，并以与 COM 对基于接口的交互的强制一致的方式向 COM 客户端返回值。 对于 COM 客户端而言，调用 .NET 对象上的方法与调用 COM 对象上的方法相同。

若要创建这一无缝的方法，CCW 生成传统 COM 接口，如 IUnknown 和 IDispatch。 如下图所示，CCW 保持对其包装的 .NET 对象的单一引用。 COM 客户端和 .NET 对象都通过代理和 CCW 的存根构造进行相互交互。

![展示 CCW 如何生成 COM 接口的图。](./media/com-callable-wrapper/com-callable-wrapper-interfaces.gif)

除公开由托管环境中的类显式实现的接口外，.NET 运行时代表对象提供对下表中列出的 COM 接口的实现。 .NET 类可以通过提供其自身对这些接口的实现而替代默认行为。 但是，运行时始终提供 IUnknown 和 IDispatch 接口的实现。

|接口|描述|
|---------------|-----------------|
|**IDispatch**|为晚期绑定到类型提供一个机制。|
|**IErrorInfo**|提供以下内容的文字描述：错误、错误源、帮助文件，帮助上下文以及定义错误的接口的 GUID（.NET 类始终为 GUID_NULL）。|
|**IProvideClassInfo**|启用 COM 客户端，以获取对由托管类实现的 ITypeInfo 接口的访问。 对于未从 COM 导入的类型，在 .NET Core 上返回 `COR_E_NOTSUPPORTED`。 |
|**ISupportErrorInfo**|启用 COM 客户端，以确定托管对象是否支持 IErrorInfo 接口。 如果支持，则启用客户端，以获取指向最新异常对象的指针。 所有托管类型都支持 IErrorInfo 接口。|
|**ITypeInfo** （仅限 .NET Framework）|为类提供与 Tlbexp.exe 生成的类型信息完全相同的类型信息。|
|**IUnknown**|提供 IUnknown 接口的标准实现，COM 客户端使用该接口管理 CCW 的生存期并提供类型强制转换。|

 托管类还可以提供下表中介绍的 COM 接口。

|接口|描述|
|---------------|-----------------|
|(\_classname) 类接口|该接口由运行时公开但未显式定义，它公开托管对象上显式公开的所有公共接口、方法、属性和字段。|
|**IConnectionPoint** 和 **IConnectionPointContainer**|以基于委托的事件（用于注册事件订阅服务器的接口）为源的对象的接口。|
|**IDispatchEx** （仅限 .NET Framework）|如果类实现 IExpando，则为由运行时提供的接口。 IDispatchEx 接口是 IDispatch 接口的扩展，与 IDispatch 不同，它可枚举、添加、删除和以区分大小的方式调用成员  。|
|**IEnumVARIANT**|集合类型类的接口，如果类实现 IEnumerable，则该接口将枚举集合中的对象。|

## <a name="introducing-the-class-interface"></a>类接口简介

类接口，未在托管代码中显式定义，是公开 .NET 对象中显式公开的所有公共方法、属性、字段和事件的接口。 此接口可以是双重接口或仅支持调度的接口。 类接口接收 .NET 类本身的名称（前面带下划线）。 例如，对于类 Mammal，类接口为 \_Mammal。

对于派生类，类接口也公开基类的所有公共方法、属性和字段。 派生类还公开各基类的类接口。 例如，如果类 Mammal 扩展类 MammalSuperclass（MammalSuperclass 类本身扩展 System.Object），则 .NET 对象向 COM 客户端公开名为 \_Mammal、\_MammalSuperclass 和 \_Object 的三个类接口。

例如，请考虑以下 .NET 类：

```vb
' Applies the ClassInterfaceAttribute to set the interface to dual.
<ClassInterface(ClassInterfaceType.AutoDual)> _
' Implicitly extends System.Object.
Public Class Mammal
    Sub Eat()
    Sub Breathe()
    Sub Sleep()
End Class
```

```csharp
// Applies the ClassInterfaceAttribute to set the interface to dual.
[ClassInterface(ClassInterfaceType.AutoDual)]
// Implicitly extends System.Object.
public class Mammal
{
    public void Eat() {}
    public void Breathe() {}
    public void Sleep() {}
}
```

COM 客户端可以获取指向名为 `_Mammal` 的类接口的指针。 在 .NET Framework 上，可使用 [类型库导出程序 (Tlbexp.exe)](../../framework/tools/tlbexp-exe-type-library-exporter.md) 工具生成包含 `_Mammal` 接口定义的类型库。 .NET Core 不支持类型库导出程序。 如果 `Mammal` 类实现了一个或多个接口，则这些接口将出现在组件类下。

```console
[odl, uuid(…), hidden, dual, nonextensible, oleautomation]
interface _Mammal : IDispatch
{
    [id(0x00000000), propget] HRESULT ToString([out, retval] BSTR*
        pRetVal);
    [id(0x60020001)] HRESULT Equals([in] VARIANT obj, [out, retval]
        VARIANT_BOOL* pRetVal);
    [id(0x60020002)] HRESULT GetHashCode([out, retval] short* pRetVal);
    [id(0x60020003)] HRESULT GetType([out, retval] _Type** pRetVal);
    [id(0x6002000d)] HRESULT Eat();
    [id(0x6002000e)] HRESULT Breathe();
    [id(0x6002000f)] HRESULT Sleep();
}
[uuid(…)]
coclass Mammal
{
    [default] interface _Mammal;
}
```

生成类接口是可选操作。 默认情况下，COM 互操作会为你导出到类型库中的每个类生成仅支持调度的接口。 可以通过将 <xref:System.Runtime.InteropServices.ClassInterfaceAttribute> 应用到你的类来阻止或修改此接口的自动创建。 尽管类接口可以减轻向 COM 公开托管类的任务，但其用途相当有限。

> [!CAUTION]
> 使用类接口（而不是显式定义你自己的类接口）可以增加托管类的未来版本控制的复杂性。 请在使用类接口之前阅读以下指南。

### <a name="define-an-explicit-interface-for-com-clients-to-use-rather-than-generating-the-class-interface"></a>定义要使用的 COM 客户端的显式接口，而非生成类接口。

由于 COM 互操作会自动生成类接口，对类进行的后期版本更改可改变由公共语言运行时公开的类接口的布局。 由于 COM 客户端通常没有准备处理接口布局中的更改，因此如果更改类的成员布局，它们将发生中断。

本指南强调了向 COM 客户端公开的接口必须保持不变这一概念。 若要降低因对接口布局无意地重新排序而中断 COM 客户端的风险，请通过显式定义接口从接口布局中隔离对类的所有更改。

使用 ClassInterfaceAttribute  来取消类接口的自动生成，并实现类的显式接口，如以下代码片段所示：

```vb
<ClassInterface(ClassInterfaceType.None)>Public Class LoanApp
    Implements IExplicit
    Sub M() Implements IExplicit.M
…
End Class
```

```csharp
[ClassInterface(ClassInterfaceType.None)]
public class LoanApp : IExplicit
{
    int IExplicit.M() { return 0; }
}
```

ClassInterfaceType.None 值防止类元数据导出到类型库时生成类接口。 在前面的示例中，COM 客户端只能通过 `IExplicit` 接口访问 `LoanApp` 类。

### <a name="avoid-caching-dispatch-identifiers-dispids"></a>避免缓存调度标识符 (DispId)

对于脚本化客户端、Microsoft Visual Basic 6.0 客户端或不缓存接口成员的 DispId 的任何后期绑定客户端，可接受使用类接口。 DispId 标识接口成员，以启用后期绑定。

对于类接口，基于接口中成员的位置生成 DispId。 如果更改了成员顺序并将类导出到类型库中，则将改变类接口中生成的 DispId。

若要避免在使用类接口时中断后期绑定 COM 客户端，请应用具有 ClassInterfaceAttribute 值的 ClassInterfaceType.AutoDispatch。 此值实现仅支持调度的类接口，但将省略类型库中的接口说明。 没有接口说明，客户端就无法在编译时缓存 DispId。 尽管这是类接口的默认接口类型，但你可以显式地应用属性值。

```vb
<ClassInterface(ClassInterfaceType.AutoDispatch)> Public Class LoanApp
    Implements IAnother
    Sub M() Implements IAnother.M
…
End Class
```

```csharp
[ClassInterface(ClassInterfaceType.AutoDispatch)]
public class LoanApp
{
    public int M() { return 0; }
}
```

若要在运行时获取接口成员的 DispId，COM 客户端可以调用 IDispatch.GetIdsOfNames。 若要调用接口上的方法，请将返回的 DispId 作为参数传递给 IDispatch.Invoke。

### <a name="restrict-using-the-dual-interface-option-for-the-class-interface"></a>限制使用类接口的双重接口选项。

双重接口通过 COM 客户端启用对接口成员的早期绑定和后期绑定。 在设计时和测试期间，将类接口设置为双重可能会非常有用。 对于永远不会被修改的托管类（及其基类），此选项也是可接受的。 在所有其它情况下，请避免将类接口设置为双重。

自动生成的双重接口可能适合少数情况；但是，更多情况下，它将造成与版本相关的复杂性。 例如，使用派生类的类接口的 COM 客户端可以通过对基类的更改轻松中断。 当第三方提供基类时，类接口的布局将不受你的控制。 进一步来说，与仅支持调度的接口不同，双重接口 (ClassInterfaceType.AutoDual)提供对导出的类型库中的类接口的说明。 此类说明会促使后期绑定的客户端在编译时缓存 DispId。

### <a name="ensure-that-all-com-event-notifications-are-late-bound"></a>确保所有 COM 事件通知都是后期绑定的。

默认情况下，COM 类型信息直接嵌入到托管程序集中，这会消除对主互操作程序集 (PIA) 的需要。 但是，嵌入式类型信息的一个限制是它不支持通过早期绑定的 vtable 调用传递 COM 事件通知，而仅支持后期绑定的 `IDispatch::Invoke` 调用。

如果应用程序需要对 COM 事件接口方法进行早期绑定调用，则可以将 Visual Studio 中的”嵌入互操作类型”属性设置为 `true`，或在项目文件中包含以下元素：

```xml
<EmbedInteropTypes>True</EmbedInteropTypes>
```

## <a name="see-also"></a>请参阅

- <xref:System.Runtime.InteropServices.ClassInterfaceAttribute>
- [COM 包装](com-wrappers.md)
- [向 COM 公开 .NET Framework 组件](../../framework/interop/exposing-dotnet-components-to-com.md)
- [向 COM 公开 .NET Core 组件](../../core/native-interop/expose-components-to-com.md)
- [为互操作限定 .NET 类型](qualify-net-types-for-interoperation.md)
- [运行时可调用包装器](runtime-callable-wrapper.md)
