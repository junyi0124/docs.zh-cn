---
description: '#warning - C# 参考'
title: '#warning - C# 参考'
ms.date: 07/20/2015
f1_keywords:
- '#warning'
helpviewer_keywords:
- '#warning directive [C#]'
ms.assetid: e6fb496d-bb8b-4018-baf6-5b60a0c8902b
ms.openlocfilehash: 9ade723bdca17597dcd56240f506e60f2debf6be
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91186414"
---
# <a name="warning-c-reference"></a>#warning（C# 参考）

`#warning` 允许你从代码中的特定位置生成 [ CS1030](../../misc/cs1030.md) 第一级编译器警告。 例如：  
  
```csharp
#warning Deprecated code in this method.  
```  
  
## <a name="remarks"></a>备注

 `#warning` 常用于条件指令中。 还可使用 [#error](./preprocessor-error.md) 生成用户定义错误。  
  
## <a name="example"></a>示例  

```csharp
// preprocessor_warning.cs  
// CS1030 expected  
#define DEBUG  
class MainClass
{  
    static void Main()
    {  
#if DEBUG  
#warning DEBUG is defined  
#endif  
    }  
}  
```  

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 预处理器指令](./index.md)
