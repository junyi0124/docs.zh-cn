---
title: 如何显式实现两个接口的成员 - C# 编程指南
description: 了解如何在此 C# 示例中显式实现具有相同成员名称的两个接口，并为每个接口成员各提供一个单独的实现。
ms.date: 07/20/2015
helpviewer_keywords:
- inheritance [C#], explicitly implementing interface members
- interfaces [C#], explicitly implementing with inheritance
ms.assetid: 8b402ddc-dff9-4869-89cb-d718c764e68e
ms.openlocfilehash: 18b1f9cfb1690d35c0bca0a3c79c1b80ae5dd44d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91205082"
---
# <a name="how-to-explicitly-implement-members-of-two-interfaces-c-programming-guide"></a>如何显式实现两个接口的成员（C# 编程指南）

显式[接口](../../language-reference/keywords/interface.md)实现还允许程序员实现具有相同成员名称的两个接口，并为每个接口成员各提供一个单独的实现。 本示例同时以公制单位和英制单位显示框的尺寸。 Box [类](../../language-reference/keywords/class.md)实现 IEnglishDimensions 和 IMetricDimensions 两个接口，它们表示不同的度量系统。 两个接口有相同的成员名称 Length 和 Width。  
  
## <a name="example"></a>示例  

 [!code-csharp[csProgGuideInheritance#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#9)]  
  
## <a name="robust-programming"></a>可靠编程  

 如果希望默认度量采用英制单位，请正常实现 Length 和 Width 方法，并从 IMetricDimensions 接口显式实现 Length 和 Width 方法：  
  
 [!code-csharp[csProgGuideInheritance#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#10)]  
  
 这种情况下，可以从类实例访问英制单位，从接口实例访问公制单位：  
  
 [!code-csharp[csProgGuideInheritance#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#11)]  
  
## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [类和结构](../classes-and-structs/index.md)
- [接口](./index.md)
- [如何显式实现接口成员](./how-to-explicitly-implement-interface-members.md)
