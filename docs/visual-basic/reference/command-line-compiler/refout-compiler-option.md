---
title: -refout
ms.date: 03/16/2018
f1_keywords:
- /refout
helpviewer_keywords:
- refout compiler option [Visual Basic]
- /refout compiler option [Visual Basic]
- -refout compiler option [Visual Basic]
ms.openlocfilehash: 3649a24a52cc6a448ea7cf4d850915adf02147fb
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348652"
---
# <a name="-refout-visual-basic"></a>-refout (Visual Basic)

-refout 选项指定应输出引用程序集的文件路径  。

[!INCLUDE[compiler-options](~/includes/compiler-options.md)]

## <a name="syntax"></a>语法

```console
-refout:filepath
```

## <a name="arguments"></a>自变量

`filepath`  
引用程序集的路径和文件名。 它通常位于主程序集的子文件夹中。 推荐约定（MSBuild 采用）是，将引用程序集放入与主程序集相关的“ref/”子文件夹中。 `filepath` 中的所有文件夹都必须存在；编译器不会创建它们。

## <a name="remarks"></a>备注

从 15.3 版开始，Visual Basic 支持 `-refout` 开关。

引用程序集是一种特殊类型的程序集，它只包含表示库的公共 API 外围应用所需的最少元数据量。 它们包括在生成工具中引用程序集时所需的所有成员的声明，但不包括所有成员实现以及对其 API 协定没有明显影响的私有成员的声明。 有关详细信息，请参阅 .NET 指南中的[引用程序集](../../../standard/assembly/reference-assemblies.md)。

`-refout` 和 [`-refonly`](refonly-compiler-option.md) 选项互斥。

## <a name="see-also"></a>请参阅

- [/refonly](refonly-compiler-option.md)
- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
