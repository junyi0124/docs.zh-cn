---
title: -define
ms.date: 03/10/2018
helpviewer_keywords:
- -d compiler option [Visual Basic]
- /d compiler option [Visual Basic]
- -define compiler option [Visual Basic]
- d compiler option [Visual Basic]
- /define compiler option [Visual Basic]
- define compiler option [Visual Basic]
ms.assetid: f735c57d-1cf9-4f2f-a26f-0de630fd4077
ms.openlocfilehash: cb56e727479fd249cb0d7e5e7c3c50d5b68b3a72
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91072010"
---
# <a name="-define-visual-basic"></a>-define (Visual Basic)

定义条件编译器常数。  
  
## <a name="syntax"></a>语法  
  
```console  
-define:["]symbol[=value][,symbol[=value]]["]  
```

or

```console  
-d:["]symbol[=value][,symbol[=value]]["]  
```  
  
## <a name="arguments"></a>自变量  
  
|术语|定义|  
|---|---|  
|`symbol`|必需。 要定义的符号。|  
|`value`|可选。 指派给 `symbol` 的值。 如果 `value` 是一个字符串，它必须放在反斜杠/双引号序列 (\\") 内而不只是双引号内。 如果未指定值，则视为 True。|  
  
## <a name="remarks"></a>备注  

 `-define` 选项具有与在源文件中使用 `#Const` 预处理器指令类似的效果，只是使用 `-define` 定义的常数为公共的且应用于项目中的所有文件。  
  
 可以将由此选项创建的符号同 `#If`...`Then`...`#Else` 指令一起使用，以对源文件进行条件编译。  
  
 `-d` 是 `-define` 的缩写形式。  
  
 通过使用逗号分隔符号定义，可以用 `-define` 定义多个符号。  
  
|在 Visual Studio 集成开发环境中设置 -define|  
|---|  
|1.在 **“解决方案资源管理器”** 中选择一个项目。 在“项目”菜单上，单击“属性”   。 <br />2.单击“编译”  选项卡。<br />3.单击 **“高级”** 。<br />4.修改“自定义常量”  框中的值。|  
  
## <a name="example"></a>示例  

 下面的代码定义并使用两个条件编译器常数。  
  
 [!code-vb[VbVbalrCompiler#45](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/Class1.vb#45)]  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [#If...Then...#Else 指令](../../language-reference/directives/if-then-else-directives.md)
- [#Const 指令](../../language-reference/directives/const-directive.md)
- [示例编译命令行](sample-compilation-command-lines.md)
