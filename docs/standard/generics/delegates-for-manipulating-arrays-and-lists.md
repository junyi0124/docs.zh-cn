---
title: 用于操作数组和列表的泛型委托
ms.date: 03/30/2017
helpviewer_keywords:
- delegates [.NET], generic delegates
- chaining delegates
- arrays [.NET], generic delegates
- generic delegates [.NET]
- lists [.NET], generic delegates
- generics [.NET], delegates
ms.assetid: 416be383-cc61-4102-9b1b-88b51adb963e
ms.openlocfilehash: 5356671b4f1c48c125e49c35a2f8ddc5ea1ca26b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728506"
---
# <a name="generic-delegates-for-manipulating-arrays-and-lists"></a>用于操作数组和列表的泛型委托

此主题概述了用于转换的泛型委托、搜索谓词以及对数组或集合中的元素所采取的操作。  
  
## <a name="generic-delegates-for-manipulating-arrays-and-lists"></a>用于操作数组和列表的泛型委托  

 <xref:System.Action%601> 泛型委托表示对指定类型的元素执行某些操作的方法。 你可以创建一种对元素执行所需操作的方法，创建 <xref:System.Action%601> 委托的实例来表示该方法，然后将该数组和委托传递给 <xref:System.Array.ForEach%2A?displayProperty=nameWithType> 静态泛型方法。 数组的每个元素都可以调用该方法。  
  
 <xref:System.Collections.Generic.List%601> 泛型类还提供了 <xref:System.Collections.Generic.List%601.ForEach%2A> 方法，该方法使用 <xref:System.Action%601> 委托。 此方法不属泛型方法。  
  
> [!NOTE]
> 这使泛型类型和方法变得有趣。 <xref:System.Array.ForEach%2A?displayProperty=nameWithType> 方法必须是静态（在 Visual Basic 中为 `Shared`）和泛型的，因为 <xref:System.Array> 不是泛型类型；你可以对 <xref:System.Array.ForEach%2A?displayProperty=nameWithType> 指定一种类型以继续运行的唯一原因是该方法有自己的类型参数列表。 与之相反，非泛型 <xref:System.Collections.Generic.List%601.ForEach%2A?displayProperty=nameWithType> 方法属于泛型类 <xref:System.Collections.Generic.List%601>，因此它仅使用其类的类型参数。 该类为强类型，因此此方法可以作为实例方法。  
  
 <xref:System.Predicate%601> 泛型委托表示的是用于确定特定元素是否满足你定义的标准的方法。 你可以将其用于 <xref:System.Array> 的以下静态泛型方法来搜索一个或一组元素：<xref:System.Array.Exists%2A>、<xref:System.Array.Find%2A>、<xref:System.Array.FindAll%2A>、<xref:System.Array.FindIndex%2A>、<xref:System.Array.FindLast%2A>、<xref:System.Array.FindLastIndex%2A> 和<xref:System.Array.TrueForAll%2A>。  
  
 <xref:System.Predicate%601> 也适用于 <xref:System.Collections.Generic.List%601> 泛型类相应的非泛型实例方法。  
  
 <xref:System.Comparison%601> 泛型委托让你能为不具有本机排序顺序的数组或列表元素提供顺序排序或重写本机排序顺序。 创建执行比较的方法，创建一个 <xref:System.Comparison%601> 委托的实例，以表示你的方法，然后将该数组和委托传递给 <xref:System.Array.Sort%60%601%28%60%600%5B%5D%2CSystem.Comparison%7B%60%600%7D%29?displayProperty=nameWithType> 静态泛型方法。 <xref:System.Collections.Generic.List%601> 泛型类提供相应的实例方法重载，<xref:System.Collections.Generic.List%601.Sort%28System.Comparison%7B%600%7D%29?displayProperty=nameWithType>。  
  
 <xref:System.Converter%602>泛型委托让你能定义两个类型之间的转换，将一个类型的数组转换到另一个类型的数组，或者将一个类型的列表转换到另一个类型的列表。 创建将现有列表元素转换为新类型的方法，创建一个委托实例来表示该方法，并使用 <xref:System.Array.ConvertAll%2A?displayProperty=nameWithType> 泛型静态方法，从原始数组生成新类型数组；或使用 <xref:System.Collections.Generic.List%601.ConvertAll%60%601%28System.Converter%7B%600%2C%60%600%7D%29?displayProperty=nameWithType> 泛型实例方法，从原始列表生成新类型列表。  
  
### <a name="chaining-delegates"></a>链接委托  

 使用这些委托的许多方法返回数组或列表，然后传递到另一种方法。 例如，如果你想要选择某些数组元素，将这些元素转换为新类型，并将其保存在新的数组中，则可以将 <xref:System.Array.FindAll%2A> 泛型方法返回的数组传递到 <xref:System.Array.ConvertAll%2A> 泛型方法。 如果新的元素类型缺少自然排序顺序，你可以将 <xref:System.Array.ConvertAll%2A> 泛型方法返回的数组传递到 <xref:System.Array.Sort%60%601%28%60%600%5B%5D%2CSystem.Comparison%7B%60%600%7D%29> 泛型方法。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Collections.Generic?displayProperty=nameWithType>
- <xref:System.Collections.ObjectModel?displayProperty=nameWithType>
- [泛型](index.md)
- [.NET 中的泛型集合](collections.md)
- [泛型接口](interfaces.md)
- [协变和逆变](covariance-and-contravariance.md)
