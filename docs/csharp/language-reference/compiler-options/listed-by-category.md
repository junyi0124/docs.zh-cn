---
description: 按类别列出的 C# 编译器选项
title: 按类别列出的 C# 编译器选项
ms.date: 06/04/2020
helpviewer_keywords:
- Visual C# compiler, options listed by category
- compiler options [C#], listed by category
- Visual C#, compiler options listed by category
ms.assetid: 96437ecc-6502-4cd3-b070-e9386a298e83
ms.openlocfilehash: a1b65db6efb60d27ab82ab47854c0399b9476729
ms.sourcegitcommit: e7acba36517134238065e4d50bb4a1cfe47ebd06
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2020
ms.locfileid: "89466074"
---
# <a name="c-compiler-options-listed-by-category"></a>按类别列出的 C# 编译器选项

下列编译器选项按类别排序。 关于按字母顺序排列的列表，请参阅[按字母顺序列出的 C# 编译器选项](listed-alphabetically.md)。

## <a name="optimization"></a>优化

|选项|目标|
|------------|-------------|
|[-filealign](filealign-compiler-option.md)|指定输出文件中各节的大小。|
|[-optimize](optimize-compiler-option.md)|启用/禁用优化。|

## <a name="output-files"></a>输出文件

|选项|目标|
|------------|-------------|
|[-deterministic](deterministic-compiler-option.md)|如果输入相同，则会导致编译器输出的程序集其二进制内容在整个编译中相同。|
|[-doc](doc-compiler-option.md)|指定要将已处理的文档注释写入到的 XML 文件。|
|[-out](out-compiler-option.md)|指定输出文件。|
|[-pathmap](pathmap-compiler-option.md)|指定编译器输出的源路径名的映射|
|[/pdb](pdb-compiler-option.md)|指定 .pdb 文件的文件名和位置。|
|[-platform](platform-compiler-option.md)|指定输出平台。|
|[/preferreduilang](preferreduilang-compiler-option.md)|指定编译器输出的语言。|
|[/refout](refout-compiler-option.md)|除主程序集之外，还生成引用程序集。|
|[/refonly](refonly-compiler-option.md)|生成引用程序集，而不生成主程序集。|
|[-target](target-compiler-option.md)|使用下列选项之一指定输出文件的格式：[-target:appcontainerexe](target-appcontainerexe-compiler-option.md)、[-target:exe](target-exe-compiler-option.md)、[-target:library](target-library-compiler-option.md)、[-target:module](target-module-compiler-option.md)、[-target:winexe](target-winexe-compiler-option.md) 或 [-target:winmdobj](target-winmdobj-compiler-option.md)。|
|-modulename：\<string>|指定源模块的名称|

## <a name="net-assemblies"></a>.NET 程序集

|选项|目标|
|------------|-------------|
|[-addmodule](addmodule-compiler-option.md)|指定一个或多个模块作为此程序集的一部分。|
|[-delaysign](delaysign-compiler-option.md)|指示编译器添加公钥，但将此程序集保留为未签名状态。|
|[-keycontainer](keycontainer-compiler-option.md)|指定加密密钥容器的名称。|
|[-keyfile](keyfile-compiler-option.md)|指定包含加密密钥的文件名。|
|[/lib](lib-compiler-option.md)|指定通过 [-reference](reference-compiler-option.md) 的方式引用的程序集的位置。|
|[-nostdlib](nostdlib-compiler-option.md)|指示编译器不导入标准库 (mscorlib.dll)。|
|[-publicsign](publicsign-compiler-option.md)|应用公钥而不签名程序集，但在程序集中设置位，来表示程序集已签名。|
|[-reference](reference-compiler-option.md)|从包含程序集的文件导入元数据。|
|-analyzer|从此程序集（缩写形式：/a）运行分析器|
|-additionalfile|命名其他文件，这些文件不会直接影响代码生成，但可能由分析器用于生成错误或警告。|
|-embed|在 PDB 中嵌入所有源文件。|
|-embed：\<file list>|在 PDB 中嵌入特定文件。|

## <a name="debuggingerror-checking"></a>调试/错误检查

|选项|目标|
|------------|-------------|
|[-bugreport](bugreport-compiler-option.md)|创建一个文件，其中包含可以轻松报告 bug 的信息。|
|[/checked](checked-compiler-option.md)|指定溢出数据类型边界的整数算法是否将导致运行时异常。|
|[-debug](debug-compiler-option.md)|指示编译器发出调试信息。|
|[-errorreport](errorreport-compiler-option.md)|设置错误报告行为。|
|[/fullpaths](fullpaths-compiler-option.md)|指定编译器输出中文件的绝对路径。|
|[-nowarn](nowarn-compiler-option.md)|取消编译器对指定警告的生成。|
|[-nullable](nullable-compiler-option.md)|指定可为空上下文选项。|
|[/warn](warn-compiler-option.md)|设置警告等级。|
|[-warnaserror](warnaserror-compiler-option.md)|将警告提升为错误。|
|-ruleset：\<file>|指定可禁用特定诊断的规则集文件。|

## <a name="preprocessor"></a>预处理器

|选项|目标|
|------------|-------------|
|[-define](define-compiler-option.md)|定义预处理器符号。|

## <a name="resources"></a>资源

|选项|目标|
|------------|-------------|
|[-link](link-compiler-option.md)|使指定程序集中的 COM 类型信息对项目可用。|
|[-linkresource](linkresource-compiler-option.md)|创建指向托管资源的链接。|
|[-resource](resource-compiler-option.md)|将 .NET 资源嵌入到输出文件中。|
|[-win32icon](win32icon-compiler-option.md)|指定要插入到输出文件的 .ico 文件。|
|[/win32res:](win32res-compiler-option.md)|指定要插入到输出文件的 Win32 资源。|

## <a name="miscellaneous"></a>杂项

|选项|目标|
|------------|-------------|
|[@](response-file-compiler-option.md)|指定响应文件。|
|[-?](help-compiler-option.md)|列出到 stdout 的编译器选项。|
|[-baseaddress](baseaddress-compiler-option.md)|指定要加载 DLL 的首选基址。|
|[-codepage](codepage-compiler-option.md)|指定要用于编译中所有源代码文件的代码页。|
|[-help](help-compiler-option.md)|列出到 stdout 的编译器选项。|
|[-highentropyva](highentropyva-compiler-option.md)|指定可执行文件支持地址空间布局随机化 (ASLR)。|
|[-langversion](langversion-compiler-option.md)|指定语言版本：默认、ISO-1、ISO-2、3、4、5、6、7、7.1、7.2、7.3 或最新版本 |
|[-main](main-compiler-option.md)|指定 Main 方法的位置。|
|[-noconfig](noconfig-compiler-option.md)|指示编译器不使用 csc.rsp 进行编译。|
|[-nologo](nologo-compiler-option.md)|禁止显示编译器横幅信息。|
|[-recurse](recurse-compiler-option.md)|搜索要编译的源文件的子目录。|
|[-subsystemversion](subsystemversion-compiler-option.md)|指定可执行文件可以使用的子系统的最低版本。|
|[unsafe](unsafe-compiler-option.md)|启用使用 [unsafe](../keywords/unsafe.md) 关键字的代码编译。|
|[-utf8output](utf8output-compiler-option.md)|显示使用 UTF-8 编码的编译器输出。|
|-parallel[+&#124;-]|指定是否使用并发生成 (+)。|
|-checksumalgorithm：\<alg>|指定用于计算 PDB 中存储的源文件校验和的算法。  受支持的值为:SHA256（默认值）或 SHA1。<br>由于与 SHA1 冲突，Microsoft 建议使用 SHA256。|

## <a name="obsolete-options"></a>已过时的选项

|选项|目标|
|---|---|
|-incremental|启用增量编译。|

## <a name="see-also"></a>请参阅

- [C# 编译器选项](index.md)
- [按字母顺序列出的 C# 编译器选项](listed-alphabetically.md)
- [如何为 Visual Studio 命令行设置环境变量](how-to-set-environment-variables-for-the-visual-studio-command-line.md)
