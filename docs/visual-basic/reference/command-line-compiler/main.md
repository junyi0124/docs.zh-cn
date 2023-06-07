---
title: -main
ms.date: 03/13/2018
helpviewer_keywords:
- main compiler option [Visual Basic]
- /main compiler option [Visual Basic]
- -main compiler option [Visual Basic]
ms.assetid: 83fc339d-6652-415d-b205-b5133319b5b0
ms.openlocfilehash: fb317b3c555d151e132122c476ce19bdeceb1321
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91065614"
---
# <a name="-main"></a>-main

指定包含 `Sub Main` 过程的类或模块。  
  
## <a name="syntax"></a>语法  
  
```console  
-main:location  
```  
  
## <a name="arguments"></a>自变量  

 `location`  
 必需。 类或模块的名称，其中包含在程序启动时要调用的 `Sub Main` 过程。 此格式可以是 -main:module  或 -main:namespace.module  。  
  
## <a name="remarks"></a>备注  

 创建可执行文件或 Windows 可执行程序时，请使用此选项。 如果省略“-main”  选项，编译器将在所有公共类和模块中搜索有效的共享 `Sub Main`。  
  
 有关 `Main` 过程的各种形式的讨论，请参阅 [Visual Basic 中的 Main 过程](../../programming-guide/program-structure/main-procedure.md)。  
  
 如果 `location` 是从 <xref:System.Windows.Forms.Form> 继承的类，则编译器将提供一个默认的 `Main` 过程，该过程在类没有 `Main` 过程的情况下启动应用程序。 这使你可以在开发环境中创建的命令行上编译代码。  
  
 [!code-vb[VbVbalrCompiler#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/Class1.vb#16)]  
  
### <a name="to-set--main-in-the-visual-studio-integrated-development-environment"></a>在 Visual Studio 集成开发环境中设置 -main  
  
1. 在 **“解决方案资源管理器”** 中选择一个项目。 在“项目”菜单上，单击“属性”   。  
  
2. 单击“应用程序”  选项卡。  
  
3. 请确保未选中“启用应用程序框架”  复选框。  
  
4. 修改“启动对象”  框中的值。  
  
## <a name="example"></a>示例  

 下面的代码编译 `T2.vb` 和 `T3.vb`，指定将在 `Test2` 类中找到 `Sub Main` 过程。  
  
```console
vbc t2.vb t3.vb -main:Test2  
```  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [-target (Visual Basic)](target.md)
- [示例编译命令行](sample-compilation-command-lines.md)
- [Visual Basic 中的 Main 过程](../../programming-guide/program-structure/main-procedure.md)
