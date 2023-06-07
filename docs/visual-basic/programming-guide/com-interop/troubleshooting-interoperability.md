---
title: 互操作性疑难解答
ms.date: 07/20/2015
helpviewer_keywords:
- interop, deploying assemblies
- assemblies [Visual Basic]
- interop, installing assemblies that share components
- COM objects, troubleshooting
- interop, sharing components
- troubleshooting interoperability [Visual Basic]
- interoperability, troubleshooting
- COM interop [Visual Basic], troubleshooting
- assemblies [Visual Basic], deploying
- troubleshooting Visual Basic, interoperability
- interop assemblies
- interoperability, sharing components
- shared components, using with assemblies
ms.assetid: b324cc1e-b03c-4f39-aea6-6a6d5bfd0e37
ms.openlocfilehash: 135b121638b92adc5a3b0920aa29d10fd1d62d14
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075988"
---
# <a name="troubleshooting-interoperability-visual-basic"></a>互操作性疑难解答 (Visual Basic)

当你在 COM 和 .NET Framework 的托管代码之间进行互操作时，你可能会遇到以下一个或多个常见问题。  
  
## <a name="interop-marshaling"></a><a name="vbconinteroperabilitymarshalinganchor1"></a> 互操作封送  

 有时，可能需要使用不属于 .NET Framework 的数据类型。 互操作程序集处理 COM 对象的大部分工作，但您可能需要控制向 COM 公开托管对象时使用的数据类型。 例如，类库中的结构必须 `BStr` 在发送到由 Visual Basic 6.0 及更早版本创建的 COM 对象的字符串上指定非托管类型。 在这种情况下，可以使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性来将托管类型公开为非托管类型。  
  
## <a name="exporting-fixed-length-strings-to-unmanaged-code"></a><a name="vbconinteroperabilitymarshalinganchor2"></a> 将固定长度的字符串导出到非托管代码  

 在 Visual Basic 6.0 及更早版本中，字符串将以字节序列的形式导出到 COM 对象，而不包含 null 终止字符。 为了与其他语言兼容，在导出字符串时 Visual Basic .NET 包含终止字符。 解决这种不兼容问题的最佳方式是将缺少终止字符的字符串导出为 `Byte` 或数组 `Char` 。  
  
## <a name="exporting-inheritance-hierarchies"></a><a name="vbconinteroperabilitymarshalinganchor3"></a> 导出继承层次结构  

 公开为 COM 对象时，托管类层次结构将展开。 例如，如果您定义一个具有成员的基类，然后继承作为 COM 对象公开的派生类中的基类，则使用 COM 对象中的派生类的客户端将无法使用继承的成员。 只能从 COM 对象访问基类成员作为基类的实例，并且仅当基类作为 COM 对象创建时才可以访问。  
  
## <a name="overloaded-methods"></a>重载方法  

 尽管可以使用 Visual Basic 创建重载方法，但 COM 不支持这些方法。 当包含重载方法的类公开为 COM 对象时，将为重载方法生成新的方法名称。  
  
 例如，假设有一个具有两个方法重载的类 `Synch` 。 当类公开为 COM 对象时，生成的新方法名称可以是 `Synch` 和 `Synch_2` 。  
  
 重命名可能会导致 COM 对象的使用者出现两个问题。  
  
1. 客户端可能不需要生成的方法名称。  
  
2. 当向类或其基类添加新的重载时，公开为 COM 对象的类中生成的方法名可能会更改。 这可能会导致版本控制问题。  
  
 若要解决这两个问题，请在开发将作为 COM 对象公开的对象时，为每个方法指定唯一名称，而不是使用重载。  
  
## <a name="use-of-com-objects-through-interop-assemblies"></a><a name="vbconinteroperabilitymarshalinganchor4"></a> 通过互操作程序集使用 COM 对象  

 使用互操作程序集的方式与它们所表示的 COM 对象的托管代码替换几乎相同。 但是，由于它们是包装器而不是实际的 COM 对象，因此使用互操作程序集和标准程序集之间存在一些差异。 这些差异区域包括类的公开以及参数和返回值的数据类型。  
  
## <a name="classes-exposed-as-both-interfaces-and-classes"></a><a name="vbconinteroperabilitymarshalinganchor5"></a> 作为接口和类公开的类  

 与标准程序集中的类不同，COM 类在互操作程序集中作为接口和表示 COM 类的类公开。 接口的名称与 COM 类的名称相同。 互操作类的名称与原始 COM 类的名称相同，但附加了 "Class" 一词。 例如，假设有一个项目，其中包含对 COM 对象的互操作程序集的引用。 如果 COM 类命名为 `MyComClass` ，IntelliSense 和对象浏览器将显示一个名为的接口 `MyComClass` 和一个名为的类 `MyComClassClass` 。  
  
## <a name="creating-instances-of-a-net-framework-class"></a><a name="vbconinteroperabilitymarshalinganchor6"></a> 创建 .NET Framework 类的实例  

 通常，使用具有类名的语句创建 .NET Framework 类的实例 `New` 。 具有由互操作程序集表示的 COM 类是一种情况，您可以在其中使用 `New` 带有接口的语句。 除非您将 COM 类与语句一起使用 `Inherits` ，否则您可以像使用类一样使用接口。 下面的代码演示如何 `Command` 在项目中创建一个对象，该对象具有对 Microsoft ActiveX 数据对象2.8 库 COM 对象的引用：  
  
 [!code-vb[VbVbalrInterop#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#20)]  
  
 但是，如果使用 COM 类作为派生类的基类，则必须使用表示 COM 类的互操作类，如以下代码所示：  
  
 [!code-vb[VbVbalrInterop#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#21)]  
  
> [!NOTE]
> 互操作程序集隐式实现表示 COM 类的接口。 不应尝试使用 `Implements` 语句来实现这些接口，否则将产生错误。  
  
## <a name="data-types-for-parameters-and-return-values"></a><a name="vbconinteroperabilitymarshalinganchor7"></a> 参数和返回值的数据类型  

 与标准程序集的成员不同，互操作程序集成员的数据类型可能不同于原始对象声明中使用的类型。 尽管互操作程序集将 COM 类型隐式转换为兼容的公共语言运行时类型，但您应注意双方使用的数据类型，以防止运行时错误。 例如，在 Visual Basic 6.0 及更早版本中创建的 COM 对象中，类型的值 `Integer` 采用 .NET Framework 等效类型 `Short` 。 建议在使用之前，使用对象浏览器来检查导入成员的特征。  
  
## <a name="module-level-com-methods"></a><a name="vbconinteroperabilitymarshalinganchor8"></a> 模块级 COM 方法  

 大多数 COM 对象的使用方法是：使用关键字创建 COM 类的实例 `New` ，然后调用该对象的方法。 此规则的一个例外涉及包含 `AppObj` 或 com 类的 com 对象 `GlobalMultiUse` 。 此类类类似于 Visual Basic .NET 类中的模块级方法。 Visual Basic 6.0 及更早版本在你首次调用其中一个方法时，会隐式创建此类对象的实例。 例如，在 Visual Basic 6.0 中，可以添加对 Microsoft DAO 3.6 对象库的引用，并在 `DBEngine` 不首先创建实例的情况下调用方法：  
  
```vb  
Dim db As DAO.Database  
' Open the database.  
Set db = DBEngine.OpenDatabase("C:\nwind.mdb")  
' Use the database object.  
```  
  
 Visual Basic .NET 要求始终创建 COM 对象的实例，然后才能使用其方法。 若要在 Visual Basic 中使用这些方法，请声明所需类的变量，并使用 new 关键字将对象分配给对象变量。 `Shared`若要确保仅创建类的一个实例，可以使用关键字。  
  
 [!code-vb[VbVbalrInterop#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#23)]  
  
## <a name="unhandled-errors-in-event-handlers"></a><a name="vbconinteroperabilitymarshalinganchor9"></a> 事件处理程序中的未处理错误  

 一个常见的互操作问题涉及事件处理程序中的错误，这些事件处理程序处理 COM 对象引发的事件。 此类错误将被忽略，除非使用或语句具体检查错误 `On Error` `Try...Catch...Finally` 。 例如，下面的示例来自一个 Visual Basic 的 .NET 项目，该项目具有对 Microsoft ActiveX 数据对象2.8 库 COM 对象的引用。  
  
 [!code-vb[VbVbalrInterop#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#24)]  
  
 此示例将按预期方式引发错误。 但是，如果您尝试不带块的相同示例，则将 `Try...Catch...Finally` 忽略该错误，就像您使用了 `OnError Resume Next` 语句一样。 如果没有错误处理，被零除会失败。 由于此类错误从不引发未经处理的异常错误，因此在处理 COM 对象的事件的事件处理程序中使用某种形式的异常处理是非常重要的。  
  
### <a name="understanding-com-interop-errors"></a>了解 COM 互操作错误  

 如果没有错误处理，互操作调用通常会生成提供少量信息的错误。 尽可能使用结构化错误处理来提供有关发生问题的详细信息。 调试应用程序时，这会特别有用。 例如：  
  
 [!code-vb[VbVbalrInterop#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#25)]  
  
 可以通过检查 exception 对象的内容来查找错误说明、HRESULT 和 COM 错误源等信息。  
  
## <a name="activex-control-issues"></a><a name="vbconinteroperabilitymarshalinganchor10"></a> ActiveX 控件问题  

 大多数与 Visual Basic 6.0 一起使用的 ActiveX 控件都适用于 Visual Basic .NET，而不会出现问题。 主要异常是容器控件，或以可视方式包含其他控件的控件。 以下是无法正常使用 Visual Studio 的较旧控件的一些示例：  
  
- Microsoft Forms 2.0 框架控件  
  
- Up-down 控件，也称为数值调节钮控件  
  
- Sheridan 选项卡控件  
  
 对于不支持的 ActiveX 控件问题，只需解决几个问题。 如果你拥有原始源代码，则可以将现有控件迁移到 Visual Studio。 否则，你可以咨询软件供应商以进行更新。用于替换不受支持的 ActiveX 控件的 NET 兼容版本的控件。  
  
## <a name="passing-readonly-properties-of-controls-byref"></a><a name="vbconinteroperabilitymarshalinganchor11"></a> 传递控件的 ReadOnly 属性 ByRef  

 当你将 `ReadOnly` 一些较旧的 ActiveX 控件的属性作为 `ByRef` 参数传递给其他过程时，Visual Basic .net 有时会引发 COM 错误，如 "错误 0x800A017F CTL_E_SETNOTSUPPORTED"。 来自 Visual Basic 6.0 的类似过程调用不会引发错误，并且这些参数被视为按值传递。 Visual Basic .NET 错误消息指示您尝试更改不具有属性过程的属性 `Set` 。  
  
 如果有权访问被调用的过程，则可以通过使用 `ByVal` 关键字声明接受属性的参数来防止此错误 `ReadOnly` 。 例如：  
  
 [!code-vb[VbVbalrInterop#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#26)]  
  
 如果您无法访问所调用过程的源代码，则可以通过在调用过程的前后添加一组额外的括号来强制通过值传递属性。 例如，在引用 Microsoft ActiveX 数据对象2.8 库 COM 对象的项目中，您可以使用：  
  
 [!code-vb[VbVbalrInterop#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#27)]  
  
## <a name="deploying-assemblies-that-expose-interop"></a><a name="vbconinteroperabilitymarshalinganchor12"></a> 部署公开互操作的程序集  

 部署公开 COM 接口的程序集会带来一些独特的挑战。 例如，当单独的应用程序引用同一 COM 程序集时，可能会出现问题。 当安装了新版本的程序集，而另一个应用程序仍在使用旧版本的程序集时，这种情况很常见。 如果卸载共享 DLL 的程序集，则可能会无意中使其对其他程序集不可用。  
  
 若要避免此问题，应将共享程序集安装到全局程序集缓存 (GAC) 并使用组件的用于合并模块。 如果无法在 GAC 中安装应用程序，则应将其安装到特定于版本的子目录中的 CommonFilesFolder。  
  
 未共享的程序集应与调用应用程序并排放置在目录中。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [COM 互操作](index.md)
- [Tlbimp.exe（类型库导入程序）](../../../framework/tools/tlbimp-exe-type-library-importer.md)
- [Tlbexp.exe（类型库导出程序）](../../../framework/tools/tlbexp-exe-type-library-exporter.md)
- [演练：使用 COM 对象实现继承](walkthrough-implementing-inheritance-with-com-objects.md)
- [Inherits Statement](../../language-reference/statements/inherits-statement.md)
- [全局程序集缓存](../../../framework/app-domains/gac.md)
