---
title: -link
ms.date: 03/10/2018
helpviewer_keywords:
- l compiler option [Visual Basic]
- EmbedInteropTypes
- embed interop types [COM Interop]
- -link compiler option [Visual Basic]
- /link compiler option [Visual Basic]
- link compiler option [Visual Basic]
- -l compiler option [Visual Basic]
- /l compiler option [Visual Basic]
ms.assetid: 1885f24a-86f5-486c-a064-9fb7e455ccec
ms.openlocfilehash: 3c02c914a27e6095f8f6bc34e29e2447a1a373e5
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91065718"
---
# <a name="-link-visual-basic"></a>-link (Visual Basic)

使编译器让指定程序集中的 COM 类型信息可供当前正在编译的项目使用。  
  
## <a name="syntax"></a>语法  
  
```console  
-link:fileList  
```

or  

```console
-l:fileList  
```  
  
## <a name="arguments"></a>自变量  
  
|术语|定义|  
|---|---|  
|`fileList`|必需。 程序集文件名的逗号分隔列表。 如果文件名包含空格，则将名称括在引号内。|  
  
## <a name="remarks"></a>备注  

 `-link` 选项使你可以部署具有嵌入类型信息的应用程序。 应用程序随后可以使用运行时程序集中实现嵌入类型信息的类型，而无需引用运行时程序集。 如果发布了各种版本的运行时程序集，则包含嵌入类型信息的应用程序可以使用各种版本，而无需重新编译。 有关示例，请参阅[演练：嵌入托管程序集中的类型](../../../standard/assembly/embed-types-visual-studio.md)。  
  
 在使用 COM 互操作时，使用 `-link` 选项会尤其有用。 可以嵌入 COM 类型，以便应用程序在目标计算机上不再需要主互操作程序集 (PIA)。 `-link` 选项指示编译器将引用的互操作程序集中的 COM 类型信息嵌入到生成的已编译代码中。 COM 类型由 CLSID (GUID) 值进行标识。 因此，应用程序可以在安装了具有相同 CLSID 值的相同 COM 类型的目标计算机上运行。 自动执行 Microsoft Office 的应用程序是一个很好的示例。 由于 Office 等应用程序通常在不同版本间保持相同的 CLSID 值，因此只要在目标计算机上安装了 .NET Framework 4 或更高版本，并且应用程序使用引用的 COM 类型中包含的方法、属性或事件，应用程序便可以使用引用的 COM 类型。  
  
 `-link` 选项只嵌入接口、结构和委托。 不支持嵌入 COM 类。  
  
> [!NOTE]
> 在代码中创建嵌入 COM 类型的实例时，必须使用适当的接口创建该实例。 尝试使用组件类创建嵌入 COM 类型的实例会导致错误。  
  
 若要在 Visual Studio 中设置 `-link` 选项，请添加程序集引用并将 `Embed Interop Types` 属性设置为“true”  。 `Embed Interop Types` 属性的默认值为 **false**。  
  
 如果链接到本身引用了其他 COM 程序集（程序集 B）的 COM 程序集（程序集 A），则在满足以下任一条件时，还必须链接到程序集 B：  
  
- 程序集 A 中的类型继承自程序集 B 中的类型或实现程序集 B 中的接口。  
  
- 调用具有程序集 B 中的返回类型或参数类型的字段、属性、事件或方法。  
  
 使用 [-libpath](libpath.md) 指定一个或多个程序集引用所在的目录。  
  
 与 [-reference](reference.md) 编译器选项类似，`-link` 编译器选项使用 Vbc.rsp 响应文件，该文件引用常用 .NET Framework 程序集。 如果不希望编译器使用 Vbc.rsp 文件，则使用 [-noconfig](noconfig.md) 编译器选项。  
  
 `-link` 的缩写形式是 `-l`。  
  
## <a name="generics-and-embedded-types"></a>泛型类型和嵌入类型  

 以下各部分介绍对在嵌入互操作类型的应用程序中使用泛型类型的限制。  
  
### <a name="generic-interfaces"></a>泛型接口  

 不能使用从互操作程序集中嵌入的泛型接口。 这在下面的示例中显示。  
  
 [!code-vb[VbLinkCompiler#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vblinkcompiler/vb/module1.vb#1)]  
  
### <a name="types-that-have-generic-parameters"></a>具有泛型参数的类型  

 对于具有类型是从互操作程序集嵌入的泛型参数的类型，如果该类型来自外部程序集，则无法使用这种类型。 此限制不适用于接口。 例如，考虑在 <xref:Microsoft.Office.Interop.Excel> 程序集中定义的 <xref:Microsoft.Office.Interop.Excel.Range> 接口。 如果某个库从 <xref:Microsoft.Office.Interop.Excel> 程序集嵌入互操作类型，并且公开的一个方法返回具有类型是 <xref:Microsoft.Office.Interop.Excel.Range> 接口的参数的泛型类型，则该方法必须返回泛型接口，如下面的代码示例所示。  
  
 [!code-vb[VbLinkCompiler#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vblinkcompiler/vb/utility.vb#2)]  
[!code-vb[VbLinkCompiler#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vblinkcompiler/vb/utility.vb#3)]  
[!code-vb[VbLinkCompiler#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vblinkcompiler/vb/utility.vb#4)]  
  
 在下面的示例中，客户端代码可以调用返回 <xref:System.Collections.IList> 泛型接口的方法而不会出现错误。  
  
 [!code-vb[VbLinkCompiler#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vblinkcompiler/vb/module1.vb#5)]  
  
## <a name="example"></a>示例  

 下面的命令行编译源文件 `OfficeApp.vb` 并引用来自 `COMData1.dll` 和 `COMData2.dll` 的程序集以生成 `OfficeApp.exe`。  
  
```console  
vbc -link:COMData1.dll,COMData2.dll -out:OfficeApp.exe OfficeApp.vb  
```  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [演练：嵌入托管程序集中的类型](../../../standard/assembly/embed-types-visual-studio.md)
- [-reference (Visual Basic)](reference.md)
- [-noconfig](noconfig.md)
- [-libpath](libpath.md)
- [示例编译命令行](sample-compilation-command-lines.md)
- [COM 互操作介绍](../../programming-guide/com-interop/introduction-to-com-interop.md)
