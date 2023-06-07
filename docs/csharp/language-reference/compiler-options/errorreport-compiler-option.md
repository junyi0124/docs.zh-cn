---
description: -errorreport（C# 编译器选项）
title: -errorreport（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /errorreport
helpviewer_keywords:
- -errorreport compiler option [C#]
- errorreport compiler option [C#]
- /errorreport compiler option [C#]
ms.assetid: bd0e7493-b79d-4369-9c3f-ba26ebdfbedf
ms.openlocfilehash: 6238ac392ff99d18d9cc7ea07e23b08ff235c14f
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91173225"
---
# <a name="-errorreport-c-compiler-options"></a>-errorreport（C# 编译器选项）

此选项提供向 Microsoft 报告 C# 内部编译错误的简便方法。

> [!NOTE]
> 在 Windows Vista 和 Windows Server 2008 上，为 Visual Studio 制定的错误报告设置不会替代通过 Windows 错误报告 (WER) 制定的设置。 WER 设置始终优先于 Visual Studio 错误报告设置。

## <a name="syntax"></a>语法

```console
-errorreport:{ none | prompt | queue | send }
```

## <a name="arguments"></a>自变量

 **无**  
 不收集有关内部编译器错误的报告，或不向 Microsoft 发送报告。

 **prompt** 当你收到内部编译器错误时，提示你发送报告。 **提示** 是在开发环境中编译应用程序时的默认值。

 **queue** 将错误报告排入队列。 使用管理凭据登录时，可以报告自上次登录以来的任何故障。 系统最多每 3 天 1 次提醒你发送故障报告。 **排队** 是在命令行编译应用程序时的默认值。

 **send** 自动向 Microsoft 发送内部编译器错误报告。 若要启用此选项，必须首先同意 Microsoft 数据收集策略。 首次在计算机上指定 -errorreport:send 时，编译器消息将引导你访问包含 Microsoft 数据收集策略的网站。

## <a name="remarks"></a>注解

 当编译器无法处理源代码文件时，会导致内部编译器错误 (ICE)。 出现 ICE 时，编译器不会生成可用于修复代码的输出文件或任何有用的诊断。

 在早期版本中，收到 ICE 时，我们欢迎你与 Microsoft 产品支持服务联系以报告问题。 通过使用 -errorreport，可向 Visual C# 团队提供 ICE 信息。 你的错误报告可以帮助改进未来的编译器版本。

 用户能否发送报告取决于计算机和用户策略权限。

### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项

1. 打开项目的“属性”页。 有关详细信息，请参阅 [“项目设计器”->“生成”页 (C#)](/visualstudio/ide/reference/build-page-project-designer-csharp)。

2. 单击“生成”属性页。

3. 单击“高级”按钮。

4. 修改“内部编译器错误报告”属性。

 有关如何以编程方式设置此编译器选项的信息，请参阅 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.ErrorReport%2A>。

## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
