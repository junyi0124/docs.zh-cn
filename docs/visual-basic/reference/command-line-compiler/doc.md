---
title: -doc
ms.date: 03/10/2018
helpviewer_keywords:
- doc compiler option [Visual Basic]
- -doc compiler option [Visual Basic]
- /doc compiler option [Visual Basic]
ms.assetid: 5fc32ec9-a149-4648-994c-a8d0cccd0a65
ms.openlocfilehash: 8b80629ce9b2cd62f10d9a53279b83ba41bc4ece
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91097704"
---
# <a name="-doc"></a>-doc

将文档注释处理到一个 XML 文件中。  
  
## <a name="syntax"></a>语法  
  
```console  
-doc[+ | -]  
```

or  

```console
-doc:file  
```  
  
## <a name="arguments"></a>自变量  
  
|术语|定义|  
|---|---|  
|`+` &#124; `-`|可选。 指定 + 或仅 `-doc` 会导致编译器生成文档信息并将其置于 XML 文件中。 指定 `-` 相当于未指定 `-doc`，因此不会创建任何文档信息。|  
|`file`|如果使用 `-doc:`，则是必需的。 指定输出 XML 文件（由编译的源代码文件中的注释填充）。 如果文件名包含空格，请用引号 (" ") 括住该名称。|  
  
## <a name="remarks"></a>备注  

 `-doc` 选项控制编译器是否生成包含文档注释的 XML 文件。 如果使用 `-doc:file` 语法，`file` 参数会指定 XML 文件的名称。 如果使用 `-doc` 或 `-doc+`，编译器会从其正在创建的可执行文件或库中获取 XML 文件名。 如果使用 `-doc-` 或未指定 `-doc` 选项，则编译器不会创建 XML 文件。  
  
 在源代码文件中，文档注释可先于以下定义：  
  
- 用户定义类型，例如[类](../../language-reference/statements/class-statement.md)或[接口](../../language-reference/statements/interface-statement.md)  
  
- 成员，例如字段、[事件](../../language-reference/statements/event-statement.md)、[属性](../../language-reference/statements/property-statement.md)、[函数](../../language-reference/statements/function-statement.md)或[子例程](../../language-reference/statements/sub-statement.md)。  
  
 若要将生成的 XML 文件与 Visual Studio [IntelliSense](/visualstudio/ide/using-intellisense) 功能配合使用，请将 XML 文件的文件名设为与要支持的程序集相同的名称。 确保 XML 文件与程序集位于同一目录中，以便在 Visual Studio 项目中引用此程序集时，也可以找到 .xml 文件。 IntelliSense 不需要 XML 文档文件来处理项目内或项目引用的项目中的代码。  
  
 除非使用 `-target:module` 进行编译，否则 XML 文件包含标记 `<assembly></assembly>`。 这些标记指定包含编译输出文件的程序集清单的文件的名称。  
  
 有关从代码中的注释生成文档的方法，请参阅 [XML 注释标记](../../language-reference/xmldoc/index.md)。  
  
|在 Visual Studio 集成开发环境中设置 -doc|  
|---|  
|1.在 **“解决方案资源管理器”** 中选择一个项目。 在“项目”菜单上，单击“属性”   。 <br />2.单击“编译”  选项卡。<br />3.在“生成 XML 文档文件”  框中设置值。|  
  
## <a name="example"></a>示例  

 有关示例，请参阅[使用 XML 来记录代码](../../programming-guide/program-structure/documenting-your-code-with-xml.md)。  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [使用 XML 记录代码](../../programming-guide/program-structure/documenting-your-code-with-xml.md)
