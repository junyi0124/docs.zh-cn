---
description: -refout（C# 编译器选项）
title: -refout（C# 编译器选项）
ms.date: 08/08/2017
f1_keywords:
- /refout
helpviewer_keywords:
- refout compiler option [C#]
- /refout compiler option [C#]
- -refout compiler option [C#]
ms.openlocfilehash: 424782e4607fea63130e95ab09a671c75fe1404d
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89128709"
---
# <a name="-refout-c-compiler-options"></a>-refout（C# 编译器选项）

-refout 选项指定应输出引用程序集的文件路径  。 这在 Emit API 中转换为 `metadataPeStream`。 此选项对应于 MSBuild 的 [ProduceReferenceAssembly](/visualstudio/msbuild/common-msbuild-project-properties) 项目属性。

## <a name="syntax"></a>语法

```console
-refout:filepath
```

## <a name="arguments"></a>自变量

 `filepath` - 引用程序集的文件路径。 通常情况下，应与主程序集的路径匹配。 推荐约定（MSBuild 采用）是，将引用程序集放入与主程序集相关的“ref/”子文件夹中。

## <a name="remarks"></a>备注

引用程序集是一种特殊类型的程序集，它只包含表示库的公共 API 外围应用所需的最少元数据量。 它们包括在生成工具中引用程序集时所需的所有成员的声明，但不包括所有成员实现以及对其 API 协定没有明显影响的私有成员的声明。 有关详细信息，请参阅 .NET 指南中的[引用程序集](../../../standard/assembly/reference-assemblies.md)。

`-refout` 和 [`-refonly`](refonly-compiler-option.md) 选项互斥。

## <a name="see-also"></a>请参阅

- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
