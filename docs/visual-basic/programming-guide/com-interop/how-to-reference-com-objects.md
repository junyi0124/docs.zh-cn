---
title: 如何：从 Visual Basic 中引用 COM 对象
ms.date: 07/20/2015
helpviewer_keywords:
- COM interop [Visual Basic], referencing COM objects
- referencing objects, COM objects from Visual Basic
- objects [Visual Basic], referencing
- COM objects, referencing
- interop assemblies
ms.assetid: 9c518fb4-27d9-4112-9e6a-5a7d0210af6f
ms.openlocfilehash: 43ba068663db9f8c3816a6f731395a6682a130e6
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91083288"
---
# <a name="how-to-reference-com-objects-from-visual-basic"></a>如何：从 Visual Basic 中引用 COM 对象

在 Visual Basic 中，添加对具有类型库的 COM 对象的引用需要为 COM 库创建互操作程序集。 对 COM 对象成员的引用将路由到互操作程序集，然后转发到实际的 COM 对象。 将 COM 对象的响应路由到互操作程序集并转发到 .NET Framework 应用程序。  
  
 通过在 .NET 程序集中嵌入 COM 对象的类型信息，可以引用 COM 对象，而无需使用互操作程序集。 若要嵌入类型信息，请将对 `Embed Interop Types` COM 对象的引用的属性设置为 `True` 。 如果要使用命令行编译器进行编译，请使用 `/link` 选项来引用 COM 库。 有关详细信息，请参阅 [-link (Visual Basic) ](../../reference/command-line-compiler/link.md)。  
  
 从集成开发环境 (IDE) 添加对类型库的引用时，Visual Basic 会自动创建互操作程序集。 在命令行中工作时，可以使用 Tlbimp 实用工具手动创建互操作程序集。  
  
### <a name="to-add-references-to-com-objects"></a>添加对 COM 对象的引用  
  
1. 在 " **项目** " 菜单上，选择 " **添加引用** "，然后单击对话框中的 " **COM** " 选项卡。  
  
2. 从 COM 对象列表中选择要使用的组件。  
  
3. 若要简化对互操作程序集的访问，请将 `Imports` 语句添加到要在其中使用 COM 对象的类或模块的顶部。 例如，下面的代码示例导入 `INKEDLib` 库中引用的对象的命名空间 `Microsoft InkEdit Control 1.0` 。  
  
     [!code-vb[VbVbalrInterop#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#40)]  
  
### <a name="to-create-an-interop-assembly-using-tlbimp"></a>使用 Tlbimp 创建互操作程序集  
  
1. 将 Tlbimp 的位置添加到搜索路径中（如果它不在搜索路径中），并且当前不在该位置的目录中。  
  
2. 从命令提示符调用 Tlbimp，提供以下信息：  
  
    - 包含类型库的 DLL 的名称和位置  
  
    - 应放置信息的命名空间的名称和位置  
  
    - 目标互操作程序集的名称和位置  
  
     以下代码提供了一个示例：  
  
    ```console  
    Tlbimp test3.dll /out:NameSpace1 /out:Interop1.dll  
    ```  
  
     可以使用 Tlbimp 为类型库创建互操作程序集，即使对于未注册的 COM 对象也是如此。 但是，互操作程序集所引用的 COM 对象必须在要使用它们的计算机上正确注册。 你可以使用 Windows 操作系统随附的 Regsvr32 实用程序来注册 COM 对象。  
  
## <a name="see-also"></a>请参阅

- [COM 互操作](index.md)
- [Tlbimp.exe（类型库导入程序）](../../../framework/tools/tlbimp-exe-type-library-importer.md)
- [Tlbexp.exe（类型库导出程序）](../../../framework/tools/tlbexp-exe-type-library-exporter.md)
- [演练：使用 COM 对象实现继承](walkthrough-implementing-inheritance-with-com-objects.md)
- [互操作性疑难解答](troubleshooting-interoperability.md)
- [Imports 语句（.NET 命名空间和类型）](../../language-reference/statements/imports-statement-net-namespace-and-type.md)
