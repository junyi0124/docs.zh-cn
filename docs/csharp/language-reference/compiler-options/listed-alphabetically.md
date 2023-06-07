---
description: 按字母顺序列出的 C# 编译器选项
title: 按字母顺序列出的 C# 编译器选项
ms.date: 06/04/2020
helpviewer_keywords:
- compiler options [C#], listed alphabetically
- C# language, compiler options listed alphabetically
- Visual C# compiler, options listed alphabetically
- Visual C#, compiler options listed alphabetically
ms.assetid: 43535ea0-ca47-4a15-b528-615087a86092
ms.openlocfilehash: 6e1351eb9989abeaadebcb755ea9268fcff40a11
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89125327"
---
# <a name="c-compiler-options-listed-alphabetically"></a>按字母顺序列出的 C# 编译器选项

下列编译器选项按字母顺序排序。 有关按类别排序的列表，请参 [按类别列出的 C# 编译器选项](listed-by-category.md)。

|选项|目标|
|------------|-------------|
|[@](response-file-compiler-option.md)|有关更多选项，请阅读响应文件。|
|[-?](help-compiler-option.md)|向 stdout 显示用法消息。|
|-additionalfile|命名其他文件，这些文件不会直接影响代码生成，但可能由分析器用于生成错误或警告。|
|[-addmodule](addmodule-compiler-option.md)|将指定的模块链接到此程序集中|
|-analyzer|从此程序集（缩写形式：-a）运行分析器|
|[/appconfig](appconfig-compiler-option.md)|在程序集绑定时指定 app.config 的位置。|
|[-baseaddress](baseaddress-compiler-option.md)|指定要生成的库的基址。|
|[-bugreport](bugreport-compiler-option.md)|创建“Bug 报告”文件。 如果与 -errorreport:prompt 或 -errorreport:send 一起使用，则发送任何崩溃信息时都将随附此文件。|
|[/checked](checked-compiler-option.md)|使编译器生成溢出检查。|
|-checksumalgorithm：\<alg>|指定用于计算 PDB 中存储的源文件校验和的算法。  受支持的值为:SHA256（默认值）或 SHA1。<br>由于与 SHA1 冲突，Microsoft 建议使用 SHA256。 |
|[-codepage](codepage-compiler-option.md)|指定在打开源文件时使用的代码页。|
|[-debug](debug-compiler-option.md)|发出调试信息。|
|[-define](define-compiler-option.md)|定义条件编译符号。|
|[-delaysign](delaysign-compiler-option.md)|仅使用强名称密钥公共部分对程序集进行延迟签名。|
|[-deterministic](deterministic-compiler-option.md)|如果输入相同，则会导致编译器输出的程序集其二进制内容在整个编译中相同。|
|[-doc](doc-compiler-option.md)|指定要生成的 XML 文档文件。|
|-embed|在 PDB 中嵌入所有源文件。|
|-embed：\<file list>|在 PDB 中嵌入特定文件。|
|-errorendlocation|每个错误结尾位置的输出行和列。|
|-errorlog：\<file>|指定要记录所有编译器和分析器诊断的文件。|
|[-errorreport](errorreport-compiler-option.md)|指定如何处理内部编译器错误；prompt、send 或 none。 默认值为 none。|
|[-filealign](filealign-compiler-option.md)|指定用于输出文件节的对齐方式。|
|[/fullpaths](fullpaths-compiler-option.md)|使编译器生成完全限定的路径。|
|[-help](help-compiler-option.md)|向 stdout 显示用法消息。|
|[-highentropyva](highentropyva-compiler-option.md)|指定支持高熵 ASLR。|
|-incremental|启用增量编译 [已过时]。|
|[-keycontainer](keycontainer-compiler-option.md)|指定强名称密钥容器。|
|[-keyfile](keyfile-compiler-option.md)|指定强名称密钥文件。|
|[-langversion：\<string>](langversion-compiler-option.md)|指定语言版本：默认、ISO-1、ISO-2、3、4、5、6、7、7.1、7.2、7.3 或最新版本 |
|[/lib](lib-compiler-option.md)|指定要在其中搜索引用的附加目录。|
|[-link](link-compiler-option.md)|使指定程序集中的 COM 类型信息对项目可用。|
|[-linkresource](linkresource-compiler-option.md)|将指定的资源链接到此程序集。|
|[-main](main-compiler-option.md)|指定包含入口点的类型（忽略所有其他可能的入口点）。|
|[-moduleassemblyname](moduleassemblyname-compiler-option.md)|指定一个程序集，.netmodule 可以访问其非公共类型。|
|-modulename：\<string>|指定源模块的名称|
|[-noconfig](noconfig-compiler-option.md)|指示编译器不自动包含 CSC.RSP 文件。|
|[-nologo](nologo-compiler-option.md)|取消编译器版权消息。|
|[-nostdlib](nostdlib-compiler-option.md)|指示编译器不引用标准库 (mscorlib.dll)。|
|[-nowarn](nowarn-compiler-option.md)|禁用特定的警告消息|
|[-nowin32manifest](nowin32manifest-compiler-option.md)|指示编译器不在可执行文件中嵌入应用程序清单。|
|[-nullable](nullable-compiler-option.md)|指定可为空上下文选项。|
|[-optimize](optimize-compiler-option.md)|启用/禁用优化。|
|[-out](out-compiler-option.md)|指定输出文件名（默认值：包含主类的文件或第一个文件的基名称）。|
|-parallel[+&#124;-]|指定是否使用并发生成 (+)。|
|[-pathmap](pathmap-compiler-option.md)|指定编译器输出的源路径名的映射。|
|[/pdb](pdb-compiler-option.md)|指定 .pdb 文件的文件名和位置。|
|[-platform](platform-compiler-option.md)|限定此代码可以在其上运行的平台：x86、Itanium、x64 、anycpu 或 anycpu32bitpreferred。 默认值为 anycpu。|
|[/preferreduilang](preferreduilang-compiler-option.md)|指定要用于编译器输出的语言。|
|[-publicsign](publicsign-compiler-option.md)|应用公钥而不签名程序集，但在程序集中设置位，来表示程序集已签名。|
|[-recurse](recurse-compiler-option.md)|根据通配符规范，包括当前目录及子目录下的所有文件。|
|[-reference](reference-compiler-option.md)|从指定的程序集文件引用元数据。|
|[/refout](refout-compiler-option.md)|除主程序集之外，还生成引用程序集。|
|[/refonly](refonly-compiler-option.md)|生成引用程序集，而不生成主程序集。|
|-reportanalyzer|报告其他分析器信息，如执行时间。|
|[-resource](resource-compiler-option.md)|嵌入指定的资源。|
|-ruleset：\<file>|指定可禁用特定诊断的规则集文件。|
|[-subsystemversion](subsystemversion-compiler-option.md)|指定可执行文件可以使用的子系统的最低版本。|
|[-target](target-compiler-option.md)|使用下列选项之一指定输出文件的格式：[-target:appcontainerexe](target-appcontainerexe-compiler-option.md)、[-target:exe](target-exe-compiler-option.md)、[-target:library](target-library-compiler-option.md)、[-target:module](target-module-compiler-option.md)、[-target:winexe](target-winexe-compiler-option.md)、[-target:winmdobj](target-winmdobj-compiler-option.md)。|
|[unsafe](unsafe-compiler-option.md)|允许[不安全](../keywords/unsafe.md)代码。|
|[-utf8output](utf8output-compiler-option.md)|以 UTF-8 编码格式输出编译器消息。|
|-version|显示编译器的版本号并退出。|
|[/warn](warn-compiler-option.md)|设置警告等级 (0-4)。|
|[-warnaserror](warnaserror-compiler-option.md)|将特定警告报告为错误。|
|[-win32icon](win32icon-compiler-option.md)|对输出使用此图标。|
|[-win32manifest](win32manifest-compiler-option.md)|指定自定义 win32 清单文件。|
|[/win32res:](win32res-compiler-option.md)|指定 win32 资源文件 (.res)。|

## <a name="see-also"></a>请参阅

- [C# 编译器选项](index.md)
- [按类别列出的 C# 编译器选项](listed-by-category.md)
- [如何为 Visual Studio 命令行设置环境变量](how-to-set-environment-variables-for-the-visual-studio-command-line.md)
- [\<compiler> 元素](../../../framework/configure-apps/file-schema/compiler/compiler-element.md)
