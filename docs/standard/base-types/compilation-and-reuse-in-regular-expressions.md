---
title: 正则表达式中的编译和重复使用
ms.date: 03/30/2017
helpviewer_keywords:
- parsing text with regular expressions, compilation
- searching with regular expressions, compilation
- .NET regular expressions, engines
- .NET regular expressions, compilation
- regular expressions, compilation
- compilation, regular expressions
- pattern-matching with regular expressions, compilation
- regular expressions, engines
ms.assetid: 182ec76d-5a01-4d73-996c-0b0d14fcea18
ms.openlocfilehash: b0d3ac619e8d9548fffcb41b23d2ebd6663915e9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723202"
---
# <a name="compilation-and-reuse-in-regular-expressions"></a>正则表达式中的编译和重复使用

通过了解正则表达式引擎编译表达式的方式以及正则表达式的缓存方式，可以优化大量使用正则表达式的应用程序的性能。 本主题介绍编译和缓存。  
  
## <a name="compiled-regular-expressions"></a>已编译的正则表达式  

 默认情况下，正则表达式引擎将正则表达式编译成内部指令序列（这些指令序列是不同于 Microsoft 中间语言 (MSIL) 的高级代码）。 当引擎执行正则表达式时，会解释内部代码。  
  
 如果 <xref:System.Text.RegularExpressions.Regex> 对象是通过 <xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType> 选项构造而成，它会将正则表达式编译为显式 MSIL 代码，而不是高级正则表达式内部指令。 这样，.NET 的实时 (JIT) 编译器便可以将表达式转换为本机代码以获得更高的性能。  构造 <xref:System.Text.RegularExpressions.Regex> 对象的成本可能会更高，但执行其匹配项的开销可能会小得多。

 替换方法是，使用预编译正则表达式。 可以使用 <xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A> 方法，将所有表达式都编译到可重用的 DLL 中。 这样一来，就无需在运行时编译，同时还仍受益于已编译正则表达式的速度优势。  
  
## <a name="the-regular-expressions-cache"></a>正则表达式缓存  

 为了提高性能，正则表达式引擎为已编译的正则表达式维护了一个应用程序范围的缓存。 该缓存只存储静态方法调用中使用的正则表达式模式。 （不缓存提供给实例方法的正则表达式模式。）这样，在每次使用正则表达式时，就无需将正则表达式重新分析成高级字节代码。  
  
 缓存正则表达式数上限由 `static`（Visual Basic 中的 `Shared`）<xref:System.Text.RegularExpressions.Regex.CacheSize%2A?displayProperty=nameWithType> 属性的值决定。 默认情况下，正则表达式引擎最多可缓存 15 个已编译的正则表达式。 如果已编译正则表达式的数目超过缓存大小，则丢弃最早使用的正则表达式并缓存新的正则表达式。  
  
 应用程序可通过以下两种方式之一来重用正则表达式：  
  
- 使用 <xref:System.Text.RegularExpressions.Regex> 对象的静态方法定义正则表达式。 如果要使用的正则表达式模式已由其他静态方法调用定义，则正则表达式引擎将尝试从缓存中检索该模式。 如果它在缓存中不可用，则引擎将编译正则表达式并将其添加到缓存中。
  
- 重用现有 <xref:System.Text.RegularExpressions.Regex> 对象（只要需要使用正则表达式模式）。  
  
 鉴于对象实例化和正则表达式编译产生的开销，因此创建并迅速销毁大量 <xref:System.Text.RegularExpressions.Regex> 对象的进程成本非常高。 对于使用大量不同正则表达式的应用，可以调用静态方法 `Regex`，并尽量增加正则表达式缓存大小，从而优化性能。  
  
## <a name="see-also"></a>请参阅

- [.NET 正则表达式](regular-expressions.md)
