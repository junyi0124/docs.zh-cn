---
title: -errorreport
ms.date: 08/14/2018
helpviewer_keywords:
- -errorreport compiler option [Visual Basic]
- /errorreport compiler option [Visual Basic]
- errorreport compiler option [Visual Basic]
ms.assetid: a7fe83a2-a6d8-460c-8dad-79a8f433f501
ms.openlocfilehash: b6a1c8fce17e3e5a54366c2ff4dff4e6aa668f56
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408655"
---
# <a name="-errorreport"></a>-errorreport

指定 Visual Basic 编译器应报告内部编译器错误的方式。

## <a name="syntax"></a>语法

```console
-errorreport:{ prompt | queue | send | none }
```

## <a name="remarks"></a>备注

此选项为将 Visual Basic 内部编译器错误 (ICE) 报告给 Microsoft 的 Visual Basic 团队提供快捷方式。 默认情况下，编译器不向 Microsoft 发送任何信息。 不过，如果你遇到了内部编译器错误，则可以通过此选项将错误报告给 Microsoft。 该信息将帮助 Microsoft 工程师确认原因，并可能会帮助改进 Visual Basic 的下一个版本。

用户能否发送报告取决于计算机和用户策略权限。

下表概括了 `-errorreport` 选项的效果。

|选项|行为|
|---|---|
|`prompt`|如果出现了内部编译器错误，会出现一个对话框，以便于你查看编译器收集到的准确数据。 你可以确定错误报告中是否有任何敏感信息，并决定是否将它发送给 Microsoft。 若你决定发送它，并且计算机和用户策略设置允许此操作，编译器会将数据发送给 Microsoft。|
|`queue`|将错误报告排入队列。 使用管理员权限登录时，你可以报告自上次登录后出现的任何故障（每三天内提示你发送故障报告的次数不会超过一次）。 未指定 `-errorreport` 选项时，这是默认行为。|
|`send`|若出现内部编译器错误，并且计算机和用户策略设置允许此操作，编译器会将数据发送给 Microsoft。<br /><br /> 若 [Windows 错误报告](/windows/desktop/wer/windows-error-reporting)系统设置启用了报告，选项 `-errorreport:send` 会尝试自动将错误信息发送给 Microsoft。 |
|`none`|若出现内部编译器错误，不会收集它，也不会将其发送给 Microsoft。|

出现错误时，编译器会发送数据，其中包括通常会包含部分源代码的堆栈。 若将 `-errorreport` 与 [-bugreport](bugreport.md) 选项配合使用，将发送完整的源文件。

此选项最适合与 [-bugreport](bugreport.md) 选项配合使用，因为这样 Microsoft 工程师即可以轻松地重现错误。

> [!NOTE]
> `-errorreport` 选项在 Visual Studio 开发环境内无法使用；仅当从命令行编译时才可用。

## <a name="example"></a>示例

下面的代码尝试编译 `T2.vb`，若编译器遇到内部编译器错误，它会提示你向 Microsoft 发送错误报告。

```console
vbc -errorreport:prompt t2.vb
```

## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
- [-bugreport](bugreport.md)
