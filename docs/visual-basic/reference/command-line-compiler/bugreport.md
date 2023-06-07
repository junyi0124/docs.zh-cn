---
title: -bugreport
ms.date: 03/08/2018
helpviewer_keywords:
- -bugreport compiler option [Visual Basic]
- bugreport compiler option [Visual Basic]
- /bugreport compiler option [Visual Basic]
ms.assetid: e4325406-8dbd-4b48-b311-9ee0799e48bb
ms.openlocfilehash: 4b468b8ee4301693312e9545396f2b9f495f75d8
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90550906"
---
# <a name="-bugreport"></a>-bugreport

创建一个在提交 bug 报告时可以使用的文件。

## <a name="syntax"></a>语法

```console
-bugreport:file
```

## <a name="arguments"></a>自变量

|术语|定义|
|---|---|
|`file`|必需。 将包含 bug 报告的文件的名称。 如果文件名包含空格，则将它括在引号 (" ") 内。|

## <a name="remarks"></a>备注

将以下信息添加到 `file`：

- 编译期间所有源代码文件的副本。

- 编译中使用的编译器选项列表。

- 有关编译器、公共语言运行时和操作系统的版本信息。

- 编译器输出（如有）。

- 问题的说明（系统会提示你提供此信息）。

- 有关你认为应如何修复问题的说明（系统会提示你提供此信息）。

所有源代码文件的副本将添加到 `file` 中，因此你可能需要在尽可能短小的程序中重现（可疑）代码缺陷。

> [!IMPORTANT]
> `-bugreport` 选项将生成一个包含潜在敏感信息的文件。 其中包括当前时间、编译器版本、.NET Framework 版本、OS 版本、用户名、运行编译器时所用的命令行参数、所有源代码和任何引用程序集的二进制文件形式。 通过在 Web.config 文件中为 ASP.NET 应用程序的服务器端编译指定命令行选项，可以访问此选项。 为了防止出现这种情况，请修改 Machine.config 文件，禁止用户在服务器上进行编译。

如果将此选项与 `-errorreport:prompt`、`-errorreport:queue` 或 `-errorreport:send` 一起使用，并且应用程序遇到内部编译器错误，则 `file` 中的信息将被发送到 Microsoft Corporation。 该信息将帮助 Microsoft 工程师确认出现错误的原因，并可能会帮助改进 Visual Basic 的下一个版本。 默认情况下，不会向 Microsoft 发送任何信息。 但是，当你使用 `-errorreport:queue`（默认情况下已启用）编译应用程序时，应用程序将收集其错误报告。 然后，当计算机管理员登录时，错误报告系统会显示一个弹出窗口，使管理员能够将自登录后发生的任何错误报告转发给 Microsoft。

> [!NOTE]
> `-bugreport` 选项在 Visual Studio 开发环境内无法使用；仅当从命令行编译时才可用。

## <a name="example"></a>示例

下面的示例将编译 T2.vb  ，并将所有 bug 报告信息放在“Problem.txt”文件  中。

```console
vbc -bugreport:problem.txt t2.vb
```

## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [-debug (Visual Basic)](debug.md)
- [-errorreport](errorreport.md)
- [示例编译命令行](sample-compilation-command-lines.md)
- [securityPolicy 的 trustLevel 元素（ASP.NET 设置架构）](/previous-versions/dotnet/netframework-4.0/as399f0x(v=vs.100))
