---
title: " (代码分析的文档规则) "
description: 了解代码分析规则文档规则
ms.date: 09/16/2019
ms.topic: reference
f1_keywords:
- vs.codeanalysis.documentationrules
helpviewer_keywords:
- documentation rules
- managed code analysis rules, documentation rules
- warnings, documentation
author: mavasani
ms.author: mavasani
ms.openlocfilehash: 29d1734eec29bd00d456b4b00c48c2e8ef223441
ms.sourcegitcommit: 2e4adc490c1d2a705a0592b295d606b10b9f51f1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "96590258"
---
# <a name="documentation-rules"></a>文档规则

文档规则支持通过对外部可见 Api 使用正确的 [XML 文档注释](../../../csharp/codedoc.md) 来编写正确的库。

## <a name="in-this-section"></a>在本节中

| 规则 | 描述 |
| - | - |
| [CA1200:不要使用带前缀的 cref 标记](ca1200.md) | XML 文档标记中的 [cref](../../../csharp/programming-guide/xmldoc/cref-attribute.md) 特性表示 "代码引用"。 它指定标记的内部文本是一个代码元素，例如类型、方法或属性。 避免使用 `cref` 带有前缀的标记，因为这会阻止编译器验证引用。 它还可防止 Visual Studio 集成开发环境 (IDE) 在重构期间查找和更新这些符号引用。 |
