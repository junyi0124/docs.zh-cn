---
title: 空间函数
ms.date: 03/30/2017
ms.assetid: 90cb177d-88a0-45be-97e8-3b306283c6e0
ms.openlocfilehash: 7d0979b5166c847244cbeec97acf4fa4f745a259
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91169578"
---
# <a name="spatial-functions"></a>空间函数

空间类型没有文本格式。 但是，您可以使用规范实体实体框架函数，这些函数可使用已知文本格式的字符串来调用。 例如，以下函数调用会生成一个几何点：  
  
```sql  
GeometryFromText('POINT (43 -73)')  
```  
  
 <xref:System.Data.Common.CommandTrees.ExpressionBuilder.Spatial.SpatialEdmFunctions>方法具有所有空间规范实体框架方法。 请单击相关方法以查看应向函数传递哪些参数。
