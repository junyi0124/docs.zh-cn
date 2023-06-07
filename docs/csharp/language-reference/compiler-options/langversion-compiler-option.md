---
description: -langversion（C# 编译器选项）
title: -langversion（C# 编译器选项）
ms.date: 05/20/2020
f1_keywords:
- /langversion
helpviewer_keywords:
- /langversion compiler option [C#]
- -langversion compiler option [C#]
- langversion compiler option [C#]
ms.custom: updateeachrelease
ms.assetid: 3fb00b05-a0ff-4782-b313-13a4c0f62d94
ms.openlocfilehash: b0e966bcc87303c0a7c2199fbfac743b22481424
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89125470"
---
# <a name="-langversion-c-compiler-options"></a>-langversion（C# 编译器选项）

使编译器仅接受包含在所选 C# 语言规范中的语法。

## <a name="syntax"></a>语法

```console
-langversion:option
```

## <a name="arguments"></a>自变量

`option`

以下为有效值：

[!INCLUDE [lang-versions-table](../includes/langversion-table.md)]

默认语言版本依赖于应用程序的目标框架以及所安装的 SDK 或 Visual Studio 的版本。 有关这些规则的定义，请参见[配置语言版本](../configure-language-version.md#defaults)一文。

## <a name="remarks"></a>备注

C# 应用程序引用的元数据不受 -langversion 编译器选项约束。

每个版本的 C# 编译器都包含语言规范的扩展，因此 -langversion 不提供早期版本编译器的同等功能。

此外，虽然 C# 版本更新通常与主要的 .NET Framework 版本一致，但新的语法和功能不一定绑定到该特定的 Framework 版本。 虽然新功能肯定需要与 C# 修订版一起发布的新编译器更新，但每项具体功能都有自己的最小 .NET API 或公共语言运行时要求，这些要求通过包括 NuGet 包或其他库允许功能在下层框架上运行。

无论使用哪一项 -langversion 设置，请使用当前版本的公共语言运行时来创建 .exe 或 .dll。 友元程序集和 [-moduleassemblyname（C# 编译器选项）](./moduleassemblyname-compiler-option.md)是一个例外，它们在 -langversion:ISO-1 下工作。

如需了解指定 C# 语言版本的其他方式，请参阅[选择 C# 语言版本](../configure-language-version.md)一文。

有关如何以编程方式设置此编译器选项的信息，请参阅 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.LanguageVersion%2A>。

## <a name="c-language-specification"></a>C# 语言规范

| Version          | 链接                       | 描述                                                             |
|------------------|----------------------------|-------------------------------------------------------------------------|
| C# 7.0 和更高版本 |                            | 当前不可用                                                 |
| C# 6.0           | [链接][csharp-6]           | C# 语言规范版本 6 - 非官方草稿：.NET Foundation |
| C# 5.0           | [下载 PDF][csharp-5]   | 标准 ECMA-334 第 5 版                                           |
| C# 3.0           | [下载 DOC][csharp-3]   | C# 语言规范版本 3.0：Microsoft Corporation            |
| C# 2.0           | [下载 PDF][csharp-2]   | 标准 ECMA-334 第 4 版                                           |
| C# 1.2           | [下载 DOC][csharp-1.2] | C# 语言规范版本 1.2：Microsoft Corporation            |
| C# 1.0           | [下载 DOC][csharp-1]   | C# 语言规范版本 1.0：Microsoft Corporation            |

[csharp-6]: /dotnet/csharp/language-reference/language-specification/introduction
[csharp-5]: https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-334.pdf
[csharp-3]: https://download.microsoft.com/download/3/8/8/388e7205-bc10-4226-b2a8-75351c669b09/CSharp%20Language%20Specification.doc
[csharp-2]: https://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-334%204th%20edition%20June%202006.pdf
[csharp-1.2]: https://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-334%202nd%20edition%20December%202002.pdf
[csharp-1]: https://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-334%201st%20edition%20December%202001.pdf

## <a name="minimum-sdk-version-needed-to-support-all-language-features"></a>支持所有语言功能所需的最低 SDK 版本

下表列出了支持相应语言版本的 C# 编译器的 SDK 的最低版本：

| C# 版本 | 最低 SDK 版本                                                                  |
|------------|--------------------------------------------------------------------------------------|
| C# 8.0     | Microsoft Visual Studio/生成工具 2019，版本 16.3 或 .NET Core 3.0 SDK         |
| C# 7.3     | Microsoft Visual Studio/生成工具 2017，版本 15.7                               |
| C# 7.2     | Microsoft Visual Studio/生成工具 2017，版本 15.5                               |
| C# 7.1     | Microsoft Visual Studio/生成工具 2017，版本 15.3                               |
| C# 7.0     | Microsoft Visual Studio/生成工具 2017                                             |
| C# 6       | Microsoft Visual Studio/生成工具 2015                                             |
| C# 5       | Microsoft Visual Studio/生成工具 2012 或捆绑的 .NET Framework 4.5 编译器      |
| C# 4       | Microsoft Visual Studio/生成工具 2010 或捆绑的 .NET Framework 4.0 编译器      |
| C# 3       | Microsoft Visual Studio/生成工具 2008 或捆绑的 .NET Framework 3.5 编译器      |
| C# 2       | Microsoft Visual Studio/生成工具 2005 或捆绑的 .Net Framework 2.0 编译器      |
| C# 1.0/1.2 | Microsoft Visual Studio/生成工具 .NET 2002 或捆绑的 .NET Framework 1.0 编译器 |

## <a name="see-also"></a>请参阅

- [C# 编译器选项](index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
