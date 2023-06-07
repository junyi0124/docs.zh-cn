---
title: 正则表达式中的线程安全
ms.date: 03/30/2017
helpviewer_keywords:
- .NET regular expressions, threads
- regular expressions, threads
- searching with regular expressions, threads
- parsing text with regular expressions, threads
- pattern-matching with regular expressions, threads
ms.assetid: 7c4a167b-5236-4cde-a2ca-58646230730f
ms.openlocfilehash: a10b5d01d308af3c808404608e6be1d77e6be8e0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734161"
---
# <a name="thread-safety-in-regular-expressions"></a>正则表达式中的线程安全

<xref:System.Text.RegularExpressions.Regex> 类本身是线程安全且不可变的（只读）。 也就是说，可以在任何线程上创建 **Regex** 对象并在线程间共享；可以从任何线程调用匹配方法并且始终不会更改全局状态。  
  
 不过，应对一个线程使用 Regex 返回的结果对象（Match 和 MatchCollection）。 尽管其中许多对象在逻辑上是不可变的，但其实现可以延迟某些结果的计算以提高性能，因此，调用方必须序列化对这些对象的访问。  
  
 如果需要在多个线程上共享 **Regex** 结果对象，则通过调用对象的同步方法，可以将这些对象转换成线程安全的实例。 除枚举器外，所有正则表达式类都是线程安全的或者可以通过同步方法转换成线程安全对象。  
  
 枚举器是唯一例外。 应用程序必须序列化对集合枚举器的调用。 规则为，如果可以在多个线程上同时枚举一个集合，则应该同步枚举器所遍历集合的根对象上的枚举器方法。  
  
## <a name="see-also"></a>另请参阅

- [.NET 正则表达式](regular-expressions.md)
