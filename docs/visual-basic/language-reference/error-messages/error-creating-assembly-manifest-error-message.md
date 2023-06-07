---
title: 创建程序集清单时出错：<error message>
ms.date: 07/20/2015
f1_keywords:
- bc30140
- vbc30140
helpviewer_keywords:
- BC30140
ms.assetid: 1beb5aa0-7b79-4c85-946b-5c2d0a41d1d2
ms.openlocfilehash: a772167966c4ec858197cc2032f4ae3d4e86070c
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163410"
---
# <a name="bc30140-error-creating-assembly-manifest-error-message"></a>BC30140：创建程序集清单时出错： \<error message>

Visual Basic 编译器调用程序集链接器 ( # A0 （也称为 Alink) ）以生成包含清单的程序集。 该链接器已报告在创建程序集的预发出阶段中出错。

 如果指定的密钥文件或密钥容器有问题，就可能发生错误。 若要对程序集进行完全签名，必须提供包含公钥和私钥信息的有效密钥文件。 若要延迟对程序集的签名，必须选择“仅延迟签名”复选框，并提供包含公钥信息的有效密钥文件****。 当程序集为延迟签名时，不需要使用私有密钥。 有关详细信息，请参阅[操作说明：使用强名称为程序集签名](../../../standard/assembly/sign-strong-name.md)。

 **错误 ID：** BC30140

## <a name="to-correct-this-error"></a>更正此错误

1. 检查引用的错误消息并参考 [Al.exe](../../../framework/tools/al-exe-assembly-linker.md)主题。 对于错误 AL1019 进一步的说明和建议

2. 如果仍然出现错误，则收集有关该情况的信息并通知 Microsoft 产品支持服务。

## <a name="see-also"></a>另请参阅

- [如何：使用强名称为程序集签名](../../../standard/assembly/sign-strong-name.md)
- [“项目设计器”-&gt;“签名”页](/visualstudio/ide/reference/signing-page-project-designer)
- [Al.exe](../../../framework/tools/al-exe-assembly-linker.md)
- [与我们交流](/visualstudio/ide/feedback-options)
