---
title: 层次结构支持
ms.date: 03/30/2017
ms.assetid: 19bb2794-b4e7-402e-8307-1d1517381a08
ms.openlocfilehash: 7673e69458d5c1af854747c43ba463332a9cd588
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91158248"
---
# <a name="inheritance-support"></a>层次结构支持

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 支持 *单表映射*。 换言之，整个继承层次结构存储在单个数据库表中。 该表包含整个层次结构的所有可能数据列的平展联合。  (联合是指将两个表组合成一个表，该表中有两个原始表中存在的行。每行 ) 的列中的 null 值不适用于行所表示的实例类型。  
  
 单表映射策略是最简单的继承表示形式，为许多不同类别的查询提供了良好的性能特征。  
  
 若要在 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 中实现这种映射，必须在继承层次结构的根类中指定属性 (Attribute) 和属性 (Attribute) 的属性 (Property)。 有关详细信息，请参阅 [如何：映射继承层次结构](how-to-map-inheritance-hierarchies.md)。  
  
 使用 Visual Studio 的开发人员还可以使用对象关系设计器来映射继承层次结构。  
  
## <a name="see-also"></a>请参阅

- [背景信息](background-information.md)
