---
title: -imports
ms.date: 03/10/2018
helpviewer_keywords:
- /imports compiler option [Visual Basic]
- imports compiler option [Visual Basic]
- -imports compiler option [Visual Basic]
ms.assetid: 9a93fb53-c080-497b-bf9b-441022dbbc39
ms.openlocfilehash: 69781dff1474e42ae5f735fdefd694c6447636b5
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085251"
---
# <a name="-imports-visual-basic"></a>-imports (Visual Basic)

从指定程序集导入命名空间。  
  
## <a name="syntax"></a>语法  
  
```console  
-imports:namespaceList  
```  
  
## <a name="arguments"></a>自变量  
  
|术语|定义|  
|---|---|  
|`namespaceList`|必需。 要导入的命名空间的逗号分隔列表。|  
  
## <a name="remarks"></a>备注  

 `-imports` 选项会导入在当前源文件集中定义的任何命名空间或是从任何引用程序集导入任何命名空间。  
  
 使用 `-imports` 指定的命名空间中的成员可用于编译中的所有源代码文件。 使用 [Imports 语句（.NET 命名空间和类型）](../../language-reference/statements/imports-statement-net-namespace-and-type.md)可将命名空间用于单个源代码文件中。  
  
|若在 Visual Studio 集成开发环境中设置 -imports|  
|---|  
|1.在 **“解决方案资源管理器”** 中选择一个项目。 在“项目”菜单上，单击“属性”   。 <br />2.单击“引用”  选项卡。<br />3.在“添加用户导入”  按钮旁的框中输入命名空间名称。<br />4.单击“添加用户导入”  按钮。|  
  
## <a name="example"></a>示例  

 以下代码在指定 `-imports:system.globalization` 时进行编译。 如果没有它，则成功编译需要将 `Imports System.Globalization` 语句包含在源代码文件的开头，或者属性完全限定为 `System.Globalization.CultureInfo.CurrentCulture.Name`。

```vb
Module Example
   Public Sub Main()
      Console.WriteLine($"The current culture is {CultureInfo.CurrentCulture.Name}")
   End Sub
End Module
```

## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [引用和 Imports 语句](../../programming-guide/program-structure/references-and-the-imports-statement.md)
- [示例编译命令行](sample-compilation-command-lines.md)
