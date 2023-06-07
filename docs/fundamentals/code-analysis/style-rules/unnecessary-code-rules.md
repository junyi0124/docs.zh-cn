---
title: 不必要的代码规则
description: 了解代码分析不必要的代码规则
ms.date: 09/30/2020
ms.topic: reference
author: gewarren
ms.author: gewarren
dev_langs:
- CSharp
- VB
ms.openlocfilehash: 16c4ba0e4bee2388736bf9813a3e8290d8d4da32
ms.sourcegitcommit: 48466b8fb7332ececff5dc388f19f6b3ff503dd4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "96590995"
---
# <a name="unnecessary-code-rules"></a>不必要的代码规则

不必要的代码规则标识不必要且可以重构或删除的基本代码部分。 不必要的代码的存在表示存在以下问题之一：

- 可读性：不必要降低可读性的代码。 例如， [IDE0001](ide0001.md) 标志不必要的类型名称限定。
- 可维护性：重构后不再使用的代码，不必要地进行维护。 例如， [IDE0051](ide0051.md) 标志未使用的私有字段、属性、事件和方法。
- 性能：不必要的计算，不会产生负面影响，并会导致不必要的性能开销。 例如， [IDE0059](ide0059.md) 标记用于计算值的表达式没有任何副作用的未使用的值赋值。
- 功能：代码中的功能问题导致所需的代码呈现为冗余。 例如， [IDE0060](ide0060.md) 将标记未使用的参数，其中方法意外忽略了输入参数。

本部分中的规则考虑以下不必要的代码规则：

- [简化名称 (IDE0001) ](ide0001.md)
- [简化成员访问 (IDE0002) ](ide0002.md)
- [删除不必要的强制转换 (IDE0004) ](ide0004.md)
- [删除不必要的导入 (IDE0005) ](ide0005.md)
- [ (IDE0035) 删除无法访问的代码 ](ide0035.md)
- [删除未使用的私有成员 (IDE0051) ](ide0051.md)
- [删除未读的私有成员 (IDE0052) ](ide0052.md)
- [删除未使用的表达式值 (IDE0058) ](ide0058.md)
- [删除不必要的值赋值 (IDE0059) ](ide0059.md)
- [删除未使用的参数 (IDE0060) ](ide0060.md)
- [删除不必要的抑制 (IDE0079) ](ide0079.md)
- [删除不必要的抑制运算符 (IDE0080) ](ide0080.md) -仅限 c #。
- 仅 Visual Basic [)  (IDE0081 删除 "ByVal"](ide0081.md) 。
- [删除不必要的相等运算符 (IDE0100) ](ide0100.md)
- [删除不必要的丢弃 (IDE0110 仅) ](ide0110.md) -c #。

其中的某些规则包含配置规则行为的选项：

- [csharp_style_unused_value_expression_statement_preference (IDE0058) ](ide0058.md#csharp_style_unused_value_expression_statement_preference)
- [visual_basic_style_unused_value_expression_statement_preference (IDE0058) ](ide0058.md#visual_basic_style_unused_value_expression_statement_preference)
- [csharp_style_unused_value_assignment_preference (IDE0059) ](ide0059.md#csharp_style_unused_value_assignment_preference)
- [visual_basic_style_unused_value_assignment_preference (IDE0059) ](ide0059.md#visual_basic_style_unused_value_assignment_preference)
- [dotnet_code_quality_unused_parameters (IDE0060) ](ide0060.md#dotnet_code_quality_unused_parameters)
- [dotnet_remove_unnecessary_suppression_exclusions (IDE0079) ](ide0079.md#dotnet_remove_unnecessary_suppression_exclusions)

## <a name="see-also"></a>另请参阅

- [代码样式语言规则](language-rules.md)
- [代码样式规则参考](index.md)
