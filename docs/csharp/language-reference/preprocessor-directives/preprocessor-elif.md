---
description: '#elif - C# 参考'
title: '#elif - C# 参考'
ms.date: 07/20/2015
f1_keywords:
- '#elif'
helpviewer_keywords:
- '#elif directive [C#]'
ms.assetid: 731d78df-08e0-4d51-b8c8-f193c27de13f
ms.openlocfilehash: 383c143792a39bb3abcd255804360ad5e2f8ef74
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91168694"
---
# <a name="elif-c-reference"></a>#elif（C# 参考）

`#elif` 可以创建复合条件指令。 如果之前的 [#if](./preprocessor-if.md) 和任何之前的可选 `#elif` 指令表达式的值为 `true`，则计算 `#elif` 表达式。 如果 `#elif` 表达式计算结果为 `true`，编译器将计算 `#elif` 和下一条件指令间的所有代码。 例如：  
  
```csharp
#define VC7  
//...  
#if debug  
    Console.WriteLine("Debug build");  
#elif VC7  
    Console.WriteLine("Visual Studio 7");  
#endif  
```  
  
 可以使用运算符 `==`（相等）、`!=`（不相等）、`&&`（和）以及`||`（或）计算多个符号。 还可以用括号对符号和运算符进行分组。  
  
## <a name="remarks"></a>备注  

 `#elif` 等效于使用：  
  
```csharp
#else  
#if  
```  
  
 使用 `#elif` 更简单，因为每个 `#if` 均需要 [#endif](./preprocessor-endif.md)，但 `#elif` 可在没有匹配的 `#endif` 的情况下使用。  
  
 有关如何使用 `#elif` 的示例，请参阅 [#if](./preprocessor-if.md)。  
  
## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 预处理器指令](./index.md)
