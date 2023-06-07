---
description: -pathmap（C# 编译器选项）
title: -pathmap（C# 编译器选项）
ms.date: 05/16/2018
f1_keywords:
- /pathmap
helpviewer_keywords:
- -pathmap compiler option [C#]
- pathmap compiler option [C#]
- /pathmap compiler option [C#]
ms.openlocfilehash: 707a37c6946cfcaf429552f0aeece6b87f3ad71d
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "89125002"
---
# <a name="-pathmap-c-compiler-options"></a>-pathmap（C# 编译器选项）

-pathmap 编译器选项指定如何将物理路径映射到编译器输出的源路径名称。

## <a name="syntax"></a>语法

```console
-pathmap:path1=sourcePath1,path2=sourcePath2
```

## <a name="arguments"></a>自变量

 `path1` 当前环境中源文件的完整路径

 `sourcePath1` 在任何输出文件中将源路径替换为 `path1`。

要指定多个映射的源路径，请用逗号分隔每个路径。

## <a name="remarks"></a>注解

编译器将源路径写入其输出，原因如下：

1. 将 <xref:System.Runtime.CompilerServices.CallerFilePathAttribute> 应用于可选参数时，会将源路径替换为参数。
1. PDB 文件中嵌入的源路径。
1. PDB 文件的路径嵌入到 PE（可移植的可执行文件）文件中。

此选项将编译器在其上运行的计算机上的每个物理路径映射到应写入输出文件的相应路径。

## <a name="example"></a>示例

在目录 C:\\work\\tests 中编译 `t.cs` 并将该目录映射到输出中的 \publish：

```console
csc -pathmap:C:\work\tests=\publish t.cs
```

## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
