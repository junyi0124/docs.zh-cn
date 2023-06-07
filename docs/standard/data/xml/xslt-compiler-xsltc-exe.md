---
title: XSLT 编译器 (xsltc.exe)
ms.date: 03/30/2017
ms.assetid: 672a5ac8-8305-4d28-ba10-11089c2c0924
ms.openlocfilehash: 89e2291cb4eafe9ca9e5001061b960f348fe4719
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720823"
---
# <a name="xslt-compiler-xsltcexe"></a>XSLT 编译器 (xsltc.exe)

XSLT 编译器 (xsltc.exe) 编译 XSLT 样式表并生成一个程序集。 然后可以将已编译的样式表直接传递到 <xref:System.Xml.Xsl.XslCompiledTransform.Load%28System.Type%29?displayProperty=nameWithType> 方法中。 不能用 xsltc.exe 生成签名的程序集。  
  
 xsltc.exe 工具包含在 Visual Studio 中。 有关详细信息，请参阅 [Visual Studio 下载](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs)。  
  
## <a name="syntax"></a>语法  
  
```console  
xsltc [options] [/class:<name>] <sourceFile> [[/class:<name>] <sourceFile>...]  
```  
  
## <a name="argument"></a>参数  
  
|参数|描述|  
|--------------|-----------------|  
|`sourceFile`|指定样式表的名称。 样式表必须是本地文件或者位于 Intranet 上。|  
  
## <a name="options"></a>选项  
  
|选项|描述|  
|------------|-----------------|  
|`/c[lass]:` `name`|指定下面样式表的类名称。 类名称可以是完全限定的名称。<br /><br /> 类名称默认为样式表的名称。 例如，如果编译样式表 customers.xsl，则默认类名称为 customers。|  
|`/debug[`+&#124;-`]`|指定是否生成调试信息。<br /><br /> 指定 `+` 或 `/debug` 将导致编译器生成调试信息并将此信息放在程序数据库 (PDB) 文件中。 生成的 PDB 文件的名称为 `assemblyName`.pdb。<br /><br /> 指定 `-`（在不指定 `/debug` 时生效）将导致不创建任何调试信息。 生成发布程序集。 **注意：** 在调试模式下进行编译会显著影响 XSLT 性能。|  
|`/help`|显示该工具的命令语法和选项。|  
|`/nologo`|禁止显示编译器版权消息。|  
|`/platform:` `string`|指定程序集可以在其上运行的平台。 下面说明有效的平台值：<br /><br /> `x86` 将程序集编译成可由 32 位、x86 兼容的公共语言运行库运行<br /><br /> `x64` 将程序集编译成可由 64 位公共语言运行库在支持 AMD64 或 EM64T 指令集的计算机上运行。<br /><br /> Itanium 将程序集编译成可由 64 位公共语言运行时在采用 Itanium 处理器的计算机上运行。<br /><br /> `anycpu` 将程序集编译成可在任何平台上运行。 这是默认设置。|  
|`/out:` `assemblyName`|指定输出的程序集的名称。 程序集名称默认为主样式表的名称；如果有多个样式表，则为第一个样式表的名称。<br /><br /> 如果样式表包含脚本，脚本将保存到单独的程序集。 脚本程序集名称从主程序集名称生成。 例如，如果您为程序集名称指定了 CustOrders.dll，则第一个脚本程序集命名为 CustOrders_Script1.dll。|  
|`/settings:` `document+-, script+-, DTD+-,`|指定是否允许在样式表中使用 `document()` 函数、XSLT 脚本或文档类型定义 (DTD)。<br /><br /> 默认行为禁用对 DTD、`document()` 函数和脚本的支持。|  
|`@` `file`|允许您指定包含编译器选项的文件。|  
|`?`|显示该工具的命令语法和选项。|  
  
## <a name="remarks"></a>备注  

 XSLT 解决方案可由多个样式表模块组成。 xsltc.exe 工具从样式表生成程序集。 然后可以将程序集传递到 <xref:System.Xml.Xsl.XslCompiledTransform.Load%28System.Type%29?displayProperty=nameWithType> 方法中。 这有助于在一些 XSLT 部署方案中降低性能成本。  
  
> [!NOTE]
> 您还必须包括已编译的程序集作为应用程序中的引用。  
  
 xsltc.exe 工具不验证类（`/class:` 名称）或程序集（`/out:`assemblyName）名称   。 如果名称无效，公共语言运行库将引发错误。  
  
## <a name="examples"></a>示例  

 下面的命令编译样式表并创建一个名为 booksort.dll 的程序集。  
  
```console  
xsltc booksort.xsl  
```  
  
 下面的命令编译样式表并创建名称分别为 booksort.dll 和 booksort.pdb 的程序集和 PDB 文件。  
  
```console  
xsltc booksort.xsl /debug  
```  
  
 下面的命令编译包含 msxsl:script 元素的样式表并创建两个名为 calc.dll 和 calc_Script1.dll 的程序集。  
  
```console  
xsltc /settings:script+ calc.xsl  
```  
  
 下面的命令启用 DTD 处理和脚本支持并创建两个名为 myTest.dll 和 myTest_Script1.dll 的程序集。  
  
```console  
xsltc /settings:DTD+,script+ /out:myTest calc.xsl  
```  
  
 下面的命令编译两个样式表模块并创建单个名为 booksort.dll 的程序集。  
  
```console  
xsltc booksort.xsl output.xsl  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.Xsl.XslCompiledTransform>
- [如何：通过使用程序集执行 XSLT 转换](how-to-perform-an-xslt-transformation-by-using-an-assembly.md)
- [XSLT 转换](xslt-transformations.md)
