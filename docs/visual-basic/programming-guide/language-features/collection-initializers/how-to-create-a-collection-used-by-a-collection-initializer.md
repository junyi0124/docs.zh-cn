---
title: 如何：创建供集合初始化程序使用的集合
ms.date: 07/20/2015
helpviewer_keywords:
- collection initializers [Visual Basic]
ms.assetid: c858db10-424d-47e0-92cd-e08087cc5ebc
ms.openlocfilehash: b332ffb44ebc20ce8431e586fc380b8c6a29967d
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91086369"
---
# <a name="how-to-create-a-collection-used-by-a-collection-initializer-visual-basic"></a>如何：创建集合初始值设定项所使用的集合 (Visual Basic)

使用集合初始值设定项创建集合时，Visual Basic 编译器会搜索集合类型的一个 `Add` 方法，该方法的方法的参数 `Add` 与集合初始值设定项中的值的类型相匹配。 此 `Add` 方法用于在集合中填充集合初始值设定项中的值。  
  
## <a name="example"></a>示例  

 下面的示例演示一个 `OrderCollection` 集合，该集合包含一个公共 `Add` 方法，集合初始值设定项可以使用该方法来添加类型的对象 `Order` 。 `Add`方法使您可以使用简化的集合初始值设定项语法。  
  
 [!code-vb[VbVbalrCollectionInitializersHowTo2#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializersHowTo2/VB/Module1.vb#4)]  
  
 [!code-vb[VbVbalrCollectionInitializersHowTo2#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializersHowTo2/VB/Module1.vb#1)]  
  
 [!code-vb[VbVbalrCollectionInitializersHowTo2#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializersHowTo2/VB/Module1.vb#2)]  
  
 [!code-vb[VbVbalrCollectionInitializersHowTo2#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializersHowTo2/VB/Module1.vb#3)]  
  
## <a name="see-also"></a>请参阅

- [集合初始值设定项](index.md)
- [如何：创建供集合初始化程序使用的 Add 扩展方法](how-to-create-an-add-extension-method-used-by-a-collection-initializer.md)
