---
title: System.String 方法
ms.date: 03/30/2017
ms.assetid: ce307f14-87e6-4816-8694-8a4147f6b784
ms.openlocfilehash: 44d32ed1000ca49d9fc29ffcde4506b44fc975b6
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91155661"
---
# <a name="systemstring-methods"></a>System.String 方法

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 不支持以下 <xref:System.String> 方法。  
  
## <a name="unsupported-systemstring-methods-in-general"></a>一般情况下不支持的 System.String 方法  

 一般情况下不支持的 <xref:System.String> 方法：  
  
- 采用) 的区域性感知重载 (方法 `CultureInfo`  /  `StringComparison`  /  `IFormatProvider` 。  
  
- 带有或生成 `char` 数组的方法。  
  
## <a name="unsupported-systemstring-static-methods"></a>不支持的 System.String 静态方法  
  
|不支持的 System.String 静态方法|  
|----------------------------------------------|  
|<xref:System.String.Copy%28System.String%29?displayProperty=nameWithType>|  
|<xref:System.String.Compare%28System.String%2CSystem.String%2CSystem.Boolean%29?displayProperty=nameWithType>|  
|<xref:System.String.Compare%28System.String%2CSystem.String%2CSystem.Boolean%2CSystem.Globalization.CultureInfo%29?displayProperty=nameWithType>|  
|<xref:System.String.Compare%28System.String%2CSystem.Int32%2CSystem.String%2CSystem.Int32%2CSystem.Int32%29?displayProperty=nameWithType>|  
|<xref:System.String.Compare%28System.String%2CSystem.Int32%2CSystem.String%2CSystem.Int32%2CSystem.Int32%2CSystem.Boolean%29?displayProperty=nameWithType>|  
|<xref:System.String.Compare%28System.String%2CSystem.Int32%2CSystem.String%2CSystem.Int32%2CSystem.Int32%2CSystem.Boolean%2CSystem.Globalization.CultureInfo%29?displayProperty=nameWithType>|  
|<xref:System.String.CompareOrdinal%28System.String%2CSystem.String%29?displayProperty=nameWithType>|  
|<xref:System.String.CompareOrdinal%28System.String%2CSystem.Int32%2CSystem.String%2CSystem.Int32%2CSystem.Int32%29?displayProperty=nameWithType>|  
|<xref:System.String.Format%2A?displayProperty=nameWithType>|  
|<xref:System.String.Join%2A?displayProperty=nameWithType>|  
  
## <a name="unsupported-systemstring-non-static-methods"></a>不支持的 System.String 非静态方法  
  
|不支持的 System.String 非静态方法|  
|---------------------------------------------------|  
|<xref:System.String.IndexOfAny%28System.Char%5B%5D%29?displayProperty=nameWithType>|  
|<xref:System.String.Split%2A?displayProperty=nameWithType>|  
|<xref:System.String.ToCharArray?displayProperty=nameWithType>|  
|<xref:System.String.ToUpper%28System.Globalization.CultureInfo%29?displayProperty=nameWithType>|  
|<xref:System.String.TrimEnd%28System.Char%5B%5D%29?displayProperty=nameWithType>|  
|<xref:System.String.TrimStart%28System.Char%5B%5D%29?displayProperty=nameWithType>|  
  
## <a name="differences-from-net"></a>与 .NET 的差异  
  
- 查询不考虑可能在服务器上生效的 SQL Server 排序规则，因而默认情况下将提供区分区域性和大小写的比较。 此行为不同于 .NET Framework 默认的区分大小写的语义。  
  
- 当 `LastIndexOf` 返回0时，字符串为 `NULL` 或找到的位置为0。  
  
- 对长度固定的字符串（`CHAR`、`NCHAR`）执行串联或其他运算时，可能会返回意外结果，原因是这些类型会自动在数据库中应用空白。  
  
- 由于许多方法（如 `Replace`、`ToLower`、`ToUpper`）和字符索引器未实现对 `TEXT` 或 `NTEXT` 列以及 XML 的有效转换，因此，如果对它们进行正常转换，则会发生 `SqlExceptions`。 对这些类型而言，这种行为被视为可接受。 但是，所有字符串运算都必须符合 `VARCHAR`、`NVARCHAR`、`VARCHAR(max)` 和 `NVARCHAR(max)` 的公共语言运行库 (CLR) 语义。  
  
## <a name="see-also"></a>请参阅

- [数据类型和函数](data-types-and-functions.md)
