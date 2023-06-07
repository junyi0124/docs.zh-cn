---
title: 如何在 C# 中定义常量
description: 了解如何定义 C# 中的常量，这些常量是在编译时设置其值的字段。 使用常量可以为特殊值提供有意义的名称。
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, constants
- constants [C#]
ms.topic: how-to
ms.custom: contperf-fy21q2
ms.assetid: 43f511be-346c-4b8a-995e-aded94542ece
ms.openlocfilehash: 972deaa4616c15c00e83e26891c4473eae7bfcf8
ms.sourcegitcommit: d0990c1c1ab2f81908360f47eafa8db9aa165137
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97513050"
---
# <a name="how-to-define-constants-in-c"></a>如何在 C\# 中定义常量

常量是在编译时设置其值并且永远不能更改其值的字段。 使用常量可以为特殊值提供有意义的名称，而不是数字文本（“幻数”）。  
  
> [!NOTE]
> 在 C# 中，不能以 C 和 C++ 中通常采用的方式使用 [#define](../../language-reference/preprocessor-directives/preprocessor-define.md) 预处理器指令定义常量。  
  
 若要定义整型类型（`int`、`byte` 等）的常量值，请使用枚举类型。 有关详细信息，请参阅[枚举](../../language-reference/builtin-types/enum.md)。  
  
 若要定义非整型常量，一种方法是将它们分组到一个名为 `Constants` 的静态类。 这要求对常量的所有引用都在其前面加上该类名，如下例所示。  
  
## <a name="example"></a>示例  

 [!code-csharp[csProgGuideObjects#89](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#89)]  
  
 使用类名限定符有助于确保你和使用该常量的其他人了解它是常量并且不能修改。  
  
## <a name="see-also"></a>请参阅

- [类和结构](./index.md)
