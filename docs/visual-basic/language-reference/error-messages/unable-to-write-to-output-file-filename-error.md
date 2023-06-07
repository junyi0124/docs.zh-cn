---
title: 无法写入输出文件“<filename>”：<error>
ms.date: 07/20/2015
f1_keywords:
- vbc31019
- bc31019
helpviewer_keywords:
- BC31019
ms.assetid: 0845b245-11bb-46fd-95ca-f6cef3c318ef
ms.openlocfilehash: 5f4add95da7c996513ffb291a7794ea0e345ac94
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161538"
---
# <a name="bc31019-unable-to-write-to-output-file-filename-error"></a>BC31019：无法写入输出文件 " \<filename> "： \<error>

创建文件时出现问题。

 无法打开输出文件以进行写入。 文件（或包含该文件的文件夹）可能由另一个进程打开以供独占使用，或者可能设置了只读特性。

 文件以独占形式打开的常见情形是：

- 应用程序已经在运行并使用它的文件。 若要解决此问题，请确保应用程序没有运行。

- 其他应用程序已经打开了该文件。 若要解决此问题，请确保其他应用程序没有访问这些文件。 是哪个应用程序在访问你的文件并不总是很明显；在这种情况下，重新启动计算机可能是终止该应用程序的最简单的方式。

 即使是项目输出文件中只有一个被标记为只读，也将会引发此异常。

 **错误 ID：** BC31019

## <a name="to-correct-this-error"></a>更正此错误

1. 再次编译此程序以查看错误是否重复出现。

2. 如果错误仍然存在，请保存你的工作并重启 Visual Studio。

3. 如果仍出现错误，请重新启动计算机。

4. 如果错误重复出现，请重新安装 Visual Basic。

5. 如果在重新安装之后依然出现错误，请通知 Microsoft 产品支持服务。

### <a name="to-check-file-attributes-in-file-explorer"></a>在“文件资源管理器”中检查文件特性

1. 打开你感兴趣的文件夹。

2. 单击 " **视图** " 图标，然后选择 " **详细信息**"。

3. 右键单击列标题，然后从下拉列表中选择 " **属性** "。

### <a name="to-change-the-attributes-of-a-file-or-folder"></a>更改文件或文件夹的特性

1. 在 **文件资源管理器**中，右键单击文件或文件夹，然后选择 " **属性**"。

2. 在 "**常规**" 选项卡的 "**属性**" 部分中，清除 "**只读**" 复选框。

3. 按“确定”。

## <a name="see-also"></a>另请参阅

- [与我们交流](/visualstudio/ide/feedback-options)
