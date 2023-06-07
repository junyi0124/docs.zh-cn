---
title: -refonly
ms.date: 03/16/2018
f1_keywords:
- -refonly
helpviewer_keywords:
- /refonly compiler option [Visual Basic]
- -refonly compiler option [Visual Basic]
- refonly compiler option [Visual Basic]
ms.openlocfilehash: b906178abf8d159083d95e41448596d512e857de
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348580"
---
# <a name="-refonly-visual-basic"></a>-refonly (Visual Basic)

-refonly  选项指示编译的主输出应为引用程序集（而不是实现程序集）。 `-refonly` 参数以无提示方式禁用输出 PDB，因为无法执行引用程序集。

[!INCLUDE[compiler-options](~/includes/compiler-options.md)]

## <a name="syntax"></a>语法

```console
-refonly
```

## <a name="remarks"></a>备注

从 15.3 版开始，Visual Basic 支持 `-refonly` 开关。

引用程序集是一种特殊类型的程序集，它只包含表示库的公共 API 外围应用所需的最少元数据量。 它们包括在生成工具中引用程序集时所需的所有成员的声明，但不包括所有成员实现以及对其 API 协定没有明显影响的私有成员的声明。 有关详细信息，请参阅 .NET 指南中的[引用程序集](../../../standard/assembly/reference-assemblies.md)。

`-refonly` 和 [`-refout`](refout-compiler-option.md) 选项互斥。

## <a name="see-also"></a>请参阅

- [/refout](refout-compiler-option.md)
- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
