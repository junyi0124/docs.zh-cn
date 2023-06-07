---
title: 不支持的功能
ms.date: 03/30/2017
ms.assetid: e480cfb5-697e-42c8-bed5-9264c945c4f9
ms.openlocfilehash: d4fe3d91b80197d962989cd2d3bc9bb2df6e3ffe
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91164150"
---
# <a name="unsupported-functionality"></a>不支持的功能

在 LINQ to SQL 中，无法通过转换现有的公共语言运行库 (CLR) 和 .NET Framework 构造公开以下 SQL 功能：  
  
- `STDDEV`  
  
- `LIKE`  
  
     尽管通过直接转换，不支持 `LIKE`，但是 <xref:System.Data.Linq.SqlClient.SqlMethods> 类中仍存在相似的功能。 有关详细信息，请参阅 <xref:System.Data.Linq.SqlClient.SqlMethods.Like%2A?displayProperty=nameWithType>。  
  
- `DATEDIFF`  
  
     LINQ to SQL 具有对 `DATEDIFF` 的有限支持。 <xref:System.Data.Linq.SqlClient.SqlMethods> 类中存在相似的功能。  
  
- `ROUND`  
  
     LINQ to SQL 具有对 `ROUND` 的有限支持。 有关详细信息，请参阅 system.exception [方法](system-math-methods.md)。  
  
## <a name="see-also"></a>请参阅

- [数据类型和函数](data-types-and-functions.md)
