---
title: 如何显式实现接口成员 - C# 编程指南
description: 了解如何在此 C# 示例中显式实现接口成员。 通过接口实例访问这些成员。
ms.date: 07/20/2015
helpviewer_keywords:
- interfaces [C#], explicitly implementing
ms.assetid: 514cde76-f981-474e-8b40-9493619f899c
ms.openlocfilehash: a9c019cdcf6e229199d980a2d1913df7c72a2169
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91157390"
---
# <a name="how-to-explicitly-implement-interface-members-c-programming-guide"></a>如何显式实现接口成员（C# 编程指南）

本示例声明一个[接口](../../language-reference/keywords/interface.md)`IDimensions` 和一个类 `Box`，显式实现了接口成员 `GetLength` 和 `GetWidth`。 通过接口实例 `dimensions` 访问这些成员。  
  
## <a name="example"></a>示例  

 [!code-csharp[csProgGuideInheritance#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#8)]  
  
## <a name="robust-programming"></a>可靠编程  
  
- 请注意，注释掉了 `Main` 方法中以下行，因为它们将产生编译错误。 显式实现的接口成员不能从[类](../../language-reference/keywords/class.md)实例访问：  
  
     [!code-csharp[csProgGuideInheritance#45](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#45)]  
  
- 另请注意 `Main` 方法中的以下行成功输出了框的尺寸，因为这些方法是从接口实例调用的：  
  
     [!code-csharp[csProgGuideInheritance#46](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#46)]  
  
## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [类和结构](../classes-and-structs/index.md)
- [接口](./index.md)
- [如何显式实现两个接口的成员](./how-to-explicitly-implement-members-of-two-interfaces.md)
