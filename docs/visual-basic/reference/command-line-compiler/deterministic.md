---
title: -deterministic
ms.date: 04/11/2018
helpviewer_keywords:
- deterministic compiler option [Visual Basic]
- -deterministic compiler option [Visual Basic]
- -deterministic compiler option [Visual Basic]
ms.openlocfilehash: 0cf9aceef54998f269e9e377fe5d0a48492969c0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408681"
---
# <a name="-deterministic"></a>-deterministic

如果输入相同，则会导致编译器生成的程序集其逐字节输出在整个编译期间中相同。

## <a name="syntax"></a>语法

```console
-deterministic
```

## <a name="remarks"></a>备注

默认情况下，一组给定输入的编译器输出是唯一的，因为编译器会添加时间戳和随意数字生成的 GUID。 使用 `-deterministic` 选项生成确定性的程序集，只要输入保持不变，该程序集的二进制内容在整个编译中都是相同的  。

出于确定性目的，编译器会考虑以下输入：

- 命令行参数序列。
- 编译器 .rsp 响应文件的内容。
- 所用编译器的精确版本及其引用的程序集。
- 当前目录路径。
- 直接或间接地显式传递到编译器的所有文件的二进制内容，包括：
  - 源文件
  - 引用的程序集
  - 引用的模块
  - 资源
  - 强名称密钥文件
  - @ 响应文件
  - 分析器
  - 规则集
  - 分析器可能使用的其他文件
- 当前区域性（针对生成诊断和异常消息的语言）。
- 在未指定编码情况下使用的默认编码（或当前代码页）。
- 编译器搜索路径（例如，由`-lib` 或 `-recurse` 指定）上文件是否存在及其内容。
- 运行编译器的 CLR 平台。
- `%LIBPATH%` 的值，该值会影响分析器的依赖项加载。

当源公开可用时，可使用确定性编译来确定是否从可信源编译二进制内容。 它还可有效用于连续生成系统，确定是否需要执行依赖于二进制内容更改的生成步骤。

## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
