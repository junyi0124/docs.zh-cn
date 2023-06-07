---
title: 新建 .NET 中的字符串
description: 了解如何在 .NET 中使用赋值、类构造函数或 System.String 方法创建字符串，这些方法结合了多个字符串、字符串数组或对象。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- CopyTo method
- Join method
- Format method
- Concat method
- strings [.NET], creating
- Insert method
ms.assetid: 06fdf123-2fac-4459-8904-eb48ab908a30
ms.openlocfilehash: a8fec6c08d44212d1b7cdd5a34c9a27b0fc19746
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683870"
---
# <a name="creating-new-strings-in-net"></a>新建 .NET 中的字符串

.NET 允许通过简单赋值创建字符串，并且还重载一个类构造函数，以支持使用一些不同参数来创建字符串。 .NET 还在 <xref:System.String?displayProperty=nameWithType> 类中提供了多个方法，可通过合并多个字符串、字符串数组或对象来新建字符串对象。  
  
## <a name="creating-strings-using-assignment"></a>通过赋值创建字符串  

 新建 <xref:System.String> 对象的最简单方法是，将字符串文本分配给 <xref:System.String> 对象。  
  
## <a name="creating-strings-using-a-class-constructor"></a>使用类构造函数创建字符串  

 可以重载 <xref:System.String> 类构造函数，通过字符数组创建字符串。 还可以通过将特定字符重复指定次数来创建新字符串。  
  
## <a name="methods-that-return-strings"></a>返回字符串的方法  

 下表列出了返回新字符串对象的几个有用方法。  
  
|方法名称|使用|  
|-----------------|---------|  
|<xref:System.String.Format%2A?displayProperty=nameWithType>|从一组输入对象生成格式化的字符串。|  
|<xref:System.String.Concat%2A?displayProperty=nameWithType>|从两个或更多个字符串生成字符串。|  
|<xref:System.String.Join%2A?displayProperty=nameWithType>|通过合并字符串数组生成新字符串。|  
|<xref:System.String.Insert%2A?displayProperty=nameWithType>|通过将一个字符串插入现有字符串的指定索引处生成新字符串。|  
|<xref:System.String.CopyTo%2A?displayProperty=nameWithType>|将一个字符串中的指定字符复制到一个字符数组中的指定位置。|  
  
### <a name="format"></a>格式  

 可以使用 String.Format 方法，创建格式化字符串，并连接表示多个对象的字符串。 此方法自动将传递给它的任何对象转换为字符串。 例如，如果应用必须向用户显示 Int32 值和 DateTime 值，可以使用 Format 方法，轻松构造表示这些值的字符串。 有关此方法使用的格式化约定的信息，请参阅有关[复合格式化](composite-formatting.md)的部分。  
  
 下面的示例使用 Format 方法，创建使用整数变量的字符串。  
  
 [!code-csharp[Strings.Creating#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#1)]
 [!code-vb[Strings.Creating#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#1)]  
  
 在此示例中，<xref:System.DateTime.Now%2A?displayProperty=nameWithType> 按照与当前线程关联的区域性指定的方式，显示当前日期和时间。  
  
### <a name="concat"></a>Concat  

 使用 String.Concat 方法，可以通过两个或多个现有对象轻松新建字符串对象。 它提供了一种与语言无关的方法来连接字符串。 此方法接受派生自 System.Object 的任何类。 下面的示例使用两个现有字符串对象和一个分隔符创建一个字符串。  
  
 [!code-csharp[Strings.Creating#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#2)]
 [!code-vb[Strings.Creating#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#2)]  
  
### <a name="join"></a>联接  

 String.Join 方法通过字符串数组和分隔符字符串新建字符串。 如果想要将多个字符串连接在一起，构成一个可能由逗号分隔的列表，则此方法非常有用。  
  
 下面的示例使用空格来连接字符串数组。  
  
 [!code-csharp[Strings.Creating#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#3)]
 [!code-vb[Strings.Creating#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#3)]  
  
### <a name="insert"></a>Insert  

 String.Insert 方法通过将一个字符串插入另一个字符串中的指定位置来新建字符串。 此方法使用从零开始的索引。 下面的示例将一个字符串插入 `MyString` 的第五个索引位置，并用此值创建新字符串。  
  
 [!code-csharp[Strings.Creating#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#4)]
 [!code-vb[Strings.Creating#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#4)]  
  
### <a name="copyto"></a>CopyTo  

 String.CopyTo 方法将字符串的某些部分复制到字符数组中。 可以同时指定字符串的开始索引和要复制的字符数。 此方法采用源索引、字符数组、目标索引和要复制的字符数。 所有索引都从零开始。  
  
 下面的示例使用 CopyTo 方法，将字符串对象中的“Hello”字词字符复制到字符数组的第一个索引位置。  
  
 [!code-csharp[Strings.Creating#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.Creating/cs/Example.cs#5)]
 [!code-vb[Strings.Creating#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.Creating/vb/Example.vb#5)]  
  
## <a name="see-also"></a>请参阅

- [基本字符串操作](basic-string-operations.md)
- [复合格式设置](composite-formatting.md)
