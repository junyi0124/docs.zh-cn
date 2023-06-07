---
title: -addmodule
ms.date: 03/09/2018
helpviewer_keywords:
- /addmodule compiler option [Visual Basic]
- addmodule compiler option [Visual Basic]
- -addmodule compiler option [Visual Basic]
ms.assetid: fb4b89d4-4926-4f20-868d-427fa28497b2
ms.openlocfilehash: 2db122acc03056a9cb6f355119d4c4e6da6ed175
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91097782"
---
# <a name="-addmodule"></a>-addmodule

使编译器让指定文件中的所有类型信息可供当前正在编译的项目使用。  
  
## <a name="syntax"></a>语法  
  
```console  
-addmodule:fileList  
```  
  
## <a name="arguments"></a>自变量  

 `fileList`  
 必需。 包含元数据但不包含程序集清单的文件的逗号分隔列表。 包含空格的文件名称应括在引号 (" ") 中。  
  
## <a name="remarks"></a>备注  

 由 `fileList` 参数列出的文件必须使用 `-target:module` 选项生成，或者使用其它编译器的 `-target:module` 等效项。  
  
 通过 `-addmodule` 添加的所有模块在运行时必须位于与输出文件相同的目录中。 也就是说，在编译时可在任何目录中指定模块，但在运行时该模块必须位于应用程序目录中。 若非如此，你会获得 <xref:System.TypeLoadException> 错误。  
  
 若你使用 `-addmodule` 指定（隐式或显式）除 `-target:module` 之外的任何 [-target (Visual Basic)](target.md) 选项，传递给 `-addmodule` 的文件会成为该项目的程序集的一部分。 必须具有程序集，才能运行包含一个或多个由 `-addmodule` 添加的文件的输出文件。  
  
 使用 [-reference (Visual Basic)](reference.md) 从包含程序集的文件导入元数据。  
  
> [!NOTE]
> `-addmodule` 选项在 Visual Studio 开发环境内无法使用；仅当从命令行编译时才可用。  
  
## <a name="example"></a>示例  

 下面的代码创建模块。  
  
 [!code-vb[VbVbalrCompiler#47](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/OptionStrictOff.vb#47)]  
  
 下面的代码导入模块的类型。  
  
 [!code-vb[VbVbalrCompiler#48](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/OptionStrictOff.vb#48)]  
  
 运行 `t1` 时，它会输出 `802`。  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [-target (Visual Basic)](target.md)
- [-reference (Visual Basic)](reference.md)
- [示例编译命令行](sample-compilation-command-lines.md)
