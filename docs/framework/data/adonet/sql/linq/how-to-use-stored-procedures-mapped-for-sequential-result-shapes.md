---
title: 如何：使用针对顺序结果形状映射的存储过程
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a73530de-5a4e-4d9c-8d66-abb19c225b11
ms.openlocfilehash: a53b2afcfce33c654b06b237cc55ad966c030568
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91184945"
---
# <a name="how-to-use-stored-procedures-mapped-for-sequential-result-shapes"></a>如何：使用针对顺序结果形状映射的存储过程

这种存储过程可以生成多个结果形状，但您知道结果的返回顺序。 请将此方案与您不知道返回顺序的方案作一个对比。 有关详细信息，请参阅 [如何：使用为多个结果形状映射的存储过程](how-to-use-stored-procedures-mapped-for-multiple-result-shapes.md)。  
  
## <a name="example"></a>示例  

 下面是一个按顺序返回多个结果形状的存储过程的 T-SQL。  
  
```sql
CREATE PROCEDURE MultipleResultTypesSequentially  
AS  
select * from products  
select * from customers  
```  
  
 [!code-csharp[DLinqSprox#6](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSprox/cs/northwind-sprox.cs#6)]
 [!code-vb[DLinqSprox#6](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSprox/vb/northwind-sprox.vb#6)]  
  
## <a name="example"></a>示例  

 你将使用类似于以下内容的代码来执行此存储过程。  
  
 [!code-csharp[DLinqSprox#7](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSprox/cs/Program.cs#7)]
 [!code-vb[DLinqSprox#7](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSprox/vb/Module1.vb#7)]  
  
## <a name="see-also"></a>请参阅

- [存储过程](stored-procedures.md)
