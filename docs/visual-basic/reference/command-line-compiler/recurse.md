---
title: -recurse
ms.date: 03/13/2018
helpviewer_keywords:
- /recurse compiler option [Visual Basic]
- -recurse compiler option [Visual Basic]
- recurse compiler option [Visual Basic]
ms.assetid: 84a0b670-33ae-44c4-a46a-b90388809317
ms.openlocfilehash: 7ded2b7d102430d8d4e545da5ab6ce8bafe3609e
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91095416"
---
# <a name="-recurse"></a>-recurse

在指定目录或项目目录的所有子目录中编译源代码文件。  
  
## <a name="syntax"></a>语法  
  
```console  
-recurse:[dir\]file  
```  
  
## <a name="arguments"></a>自变量  

 `dir`  
 可选。 希望从中开始搜索的目录。 如未指定目录，搜索将从项目目录开始。  
  
 `file`  
 必需。 要搜索的文件。 允许通配符。  
  
## <a name="remarks"></a>备注  

 可在文件名中使用通配符，对项目目录中的所有匹配文件进行编译，而无需使用 `-recurse`。 如果未指定输出文件名，编译器会将输出文件名建立在处理的第一个输入文件的基础上。 这通常是按字母顺序查看时所编译文件列表中的第一个文件。 因此，最好使用 `-out` 选项来指定输出文件。  
  
> [!NOTE]
> `-recurse` 选项在 Visual Studio 开发环境内无法使用；仅当从命令行编译时才可用。  
  
## <a name="example"></a>示例  

 以下命令编译当前目录中的所有 Visual Basic 文件。  
  
```console
vbc *.vb  
```  
  
 以下命令编译 `Test\ABC` 目录及其下任何目录中的所有 Visual Basic 文件，然后生成 `Test.ABC.dll`。  
  
```console
vbc -target:library -out:Test.ABC.dll -recurse:Test\ABC\*.vb  
```  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [-out (Visual Basic)](out.md)
- [示例编译命令行](sample-compilation-command-lines.md)
