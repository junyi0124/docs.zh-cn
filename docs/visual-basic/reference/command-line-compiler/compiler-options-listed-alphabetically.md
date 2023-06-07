---
title: 按字母顺序列出的编译器选项
ms.date: 04/12/2018
helpviewer_keywords:
- Visual Basic compiler, options
ms.assetid: e67febba-bacf-4e1f-a143-c141e063f90e
ms.openlocfilehash: d0dbe785edf7a9aa029d4be08a9b854cf1fb2b79
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085290"
---
# <a name="visual-basic-compiler-options-listed-alphabetically"></a>按字母顺序列出的 Visual Basic 编译器选项

Visual Basic 命令行编译器用作一种替代方法，用于在 Visual Studio 集成开发环境 (IDE) 中编译程序。 以下是按字母顺序排序的 Visual Basic 命令行编译器选项的列表。  

[!INCLUDE[compiler-options](~/includes/compiler-options.md)]
  
|选项|目标|  
|------------|-------------|  
|[@（指定响应文件）](specify-response-file.md)|指定响应文件。|  
|[-?](help.md)|显示编译器选项。 此命令等同于指定 `-help` 选项。 未进行编译。|  
|`-additionalfile`|命名其他文件，这些文件不会直接影响代码生成，但可能由分析器用于生成错误或警告。|  
|[-addmodule](addmodule.md)|使编译器让指定文件中的所有类型信息可供当前正在编译的项目使用。|  
|`-analyzer`|从此程序集（缩写形式：-a）运行分析器|  
|[-baseaddress](baseaddress.md)|指定的 DLL 的基址。|  
|[-bugreport](bugreport.md)|创建一个文件，其中包含可以轻松报告 bug 的信息。|  
|`-checksumalgorithm:<alg>`|指定用于计算 PDB 中存储的源文件校验和的算法。  受支持的值为:SHA1（默认值）或 SHA256。 <br>由于与 SHA1 冲突，Microsoft 建议使用 SHA256 或更高版本。|  
|[-codepage](codepage.md)|指定要用于编译中所有源代码文件的代码页。|  
|[-debug](debug.md)|生成调试信息。|  
|[-define](define.md)|定义条件编译的符号。|  
|[-delaysign](delaysign.md)|指定程序集是完全签名的还是部分签名的。|  
|[-deterministic](deterministic.md)|如果输入相同，则会导致编译器输出的程序集其二进制内容在整个编译中相同。|
|[-doc](doc.md)|将文档注释处理到一个 XML 文件中。|  
|[-errorreport](errorreport.md)|指定 Visual Basic 编译器应报告内部编译器错误的方式。|  
|[-filealign](filealign.md)|指定输出文件各节的对齐位置。|  
|[-help](help.md)|显示编译器选项。 此命令等同于指定 `-?` 选项。 未进行编译。|  
|[-highentropyva](highentropyva.md)|指示特定的可执行文件是否支持高熵地址空间布局随机化 (ASLR)。|  
|[-imports](imports.md)|从指定的程序集导入命名空间。|  
|[-keycontainer](keycontainer.md)|指定密钥对的密钥容器名称从而为程序集赋予强名称。|  
|[-keyfile](keyfile.md)|指定包含密钥或密钥对的文件从而为程序集赋予强名称。|  
|[-langversion](langversion.md)|指定语言版本：9&#124;9.0&#124;10&#124;10.0&#124;11&#124;11.0。|  
|[-libpath](libpath.md)|指定通过 [-reference](reference.md) 选项引用的程序集的位置。|  
|[-linkresource](linkresource.md)|创建指向托管资源的链接。|  
|[-main](main.md)|指定包含启动时要使用的 `Sub Main` 过程的类。|  
|[-moduleassemblyname](moduleassemblyname.md)|指定模块所属程序集的名称。|  
|`-modulename:<string>`|指定源模块的名称|  
|[-netcf](netcf.md)|将编译器设置为以 .NET Compact Framework 为目标。|  
|[-noconfig](noconfig.md)|禁止使用 Vbc.rsp 进行编译。|  
|[-nologo](nologo.md)|禁止显示编译器横幅信息。|  
|[-nostdlib](nostdlib.md)|导致编译器不引用标准库。|  
|[-nowarn](nowarn.md)|禁止编译器生成警告的能力。|  
|[-nowin32manifest](nowin32manifest.md)|指示编译器不在可执行文件中嵌入任何应用程序清单。|  
|[-optimize](optimize.md)|启用/禁用代码优化。|  
|[-optioncompare](optioncompare.md)|指定字符串比较是否应为二进制，或是否应使用特定于区域设置的文本语义。|  
|[-optionexplicit](optionexplicit.md)|强制执行显式声明变量。|  
|[-optioninfer](optioninfer.md)|允许在变量声明中使用局部类型推理。|  
|[-optionstrict](optionstrict.md)|强制执行严格的语言语义。|  
|[-out](out.md)|指定输出目录。|  
|<code>-parallel[+&#124;-]</code>|指定是否使用并发生成 (+)。|  
|[-platform](platform.md)|为输出文件指定编译器面向的处理器平台。|  
|`-preferreduilang`|指定首选输出语言名称。|  
|[-quiet](quiet.md)|阻止编译器显示与语法相关的错误和警告的代码。|  
|[-recurse](recurse.md)|搜索要编译的源文件的子目录。|  
|[-reference](reference.md)|从程序集导入元数据。|  
|[/refonly](refonly-compiler-option.md)|仅输出引用程序集。|
|[/refout](refout-compiler-option.md)|指定引用程序集的输出路径。|
|[-removeintchecks](removeintchecks.md)|禁用整数溢出检查。|  
|[-resource](resource.md)|将托管资源嵌入程序集。|  
|[-rootnamespace](rootnamespace.md)|指定所有类型声明的命名空间。|  
|`-ruleset:<file>`|指定可禁用特定诊断的规则集文件。|  
|[-sdkpath](sdkpath.md)|指定 Mscorlib.dll 和 Microsoft.VisualBasic.dll 的位置。|  
|[-subsystemversion](subsystemversion.md)|指定生成的可执行文件可以使用的子系统的最低版本。|  
|[-target](target.md)|指定输出文件的格式。|  
|[-utf8output](utf8output.md)|显示使用 UTF-8 编码的编译器输出。|  
|[-vbruntime](vbruntime.md)|指定编译器应在不引用 Visual Basic 运行库的情况下进行编译，或在引用特定运行库的情况下进行编译。|  
|[-verbose](verbose.md)|在编译期间输出其他信息。|  
|[-warnaserror](warnaserror.md)|将警告提升为错误。|  
|[-win32icon](win32icon.md)|将 .ico 文件插入到输出文件。|  
|[-win32manifest](win32manifest.md)|标识用户定义的 Win32 应用程序清单文件要嵌入到项目的可移植可执行 (PE) 文件。|  
|[-win32resource](win32resource.md)|将 Win32 资源插入到输出文件。|  
  
## <a name="see-also"></a>请参阅

- [按类别列出的 Visual Basic 编译器选项](compiler-options-listed-by-category.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
