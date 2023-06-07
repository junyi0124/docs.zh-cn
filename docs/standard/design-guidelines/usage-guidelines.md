---
title: 使用准则
ms.date: 10/22/2008
helpviewer_keywords:
- class library design guidelines [.NET Framework], usage guidelines
ms.assetid: 42215ffa-a099-4a26-b14e-fb2bdb6f95b7
ms.openlocfilehash: d6ea7c7b9ada95e3d0c425aaea18be6cdbb4ce35
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94828499"
---
# <a name="usage-guidelines"></a>使用准则

本部分包含有关在可公开访问的 Api 中使用常见类型的准则。 它处理内置框架类型的直接使用 (例如，序列化特性) 和重载公共运算符。
  
<xref:System.IDisposable?displayProperty=nameWithType>此部分未介绍接口，但会在 " [Dispose 模式](../garbage-collection/implementing-dispose.md)" 部分中讨论。

> [!NOTE]
> 有关其他通用内置 .NET Framework 类型的指南和其他信息，请参阅以下主题： <xref:System.DateTime?displayProperty=nameWithType> 、 <xref:System.DateTimeOffset?displayProperty=nameWithType> 、 <xref:System.ICloneable?displayProperty=nameWithType> 、 <xref:System.IComparable%601?displayProperty=nameWithType> 、 <xref:System.IEquatable%601?displayProperty=nameWithType> 、 <xref:System.Nullable%601?displayProperty=nameWithType> <xref:System.Object?displayProperty=nameWithType> <xref:System.Uri?displayProperty=nameWithType> 、和。

## <a name="in-this-section"></a>在此部分中

[数组](arrays.md)  
[特性](attributes.md)  
[集合](guidelines-for-collections.md)  
[序列化](serialization.md)  
[System.Xml 用法](system-xml-usage.md)  
[相等运算符](equality-operators.md)  

*部分©2005，2009 Microsoft Corporation。保留所有权利。*

*经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*
  
## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
