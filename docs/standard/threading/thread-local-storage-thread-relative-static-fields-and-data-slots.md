---
title: 线程本地存储区：线程相关的静态字段和数据槽
ms.date: 03/30/2017
helpviewer_keywords:
- threading [.NET], local storage
- threading [.NET], thread-relative static fields
- local thread storage
- TLS
ms.assetid: c633a4dc-a790-4ed1-96b5-f72bd968b284
ms.openlocfilehash: b45c83887d278589cc1704ec1398ec99e27550ad
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727518"
---
# <a name="thread-local-storage-thread-relative-static-fields-and-data-slots"></a>线程本地存储区：线程相关的静态字段和数据槽

托管线程本地存储 (TLS) 可用于存储对线程和应用程序域唯一的数据。 .NET 提供了下面两种托管 TLS 使用方式：线程相对静态字段和数据槽。  
  
- 如果可以在编译时预测确切需求，请使用线程相对静态字段（Visual Basic 中的线程相对 `Shared` 字段）。 线程相对静态字段的性能最佳。 它们还支持编译时类型检查。  
  
- 如果只能在运行时发现实际需求，请使用数据槽。 数据槽使用起来比线程相对静态字段更慢、更加棘手。由于数据存储为类型 <xref:System.Object>，因此使用前必须将它强制转换为正确类型。  
  
 在非托管 C++ 中，使用 `TlsAlloc` 动态分配槽，使用 `__declspec(thread)` 声明应在线程相对存储中分配变量。 线程相对静态字段和数据槽提供了此行为的托管版本。  
  
可以使用 <xref:System.Threading.ThreadLocal%601?displayProperty=nameWithType> 类来创建在首次使用对象时迟缓初始化的线程本地对象。 若要了解详细信息，请参阅[迟缓初始化](../../framework/performance/lazy-initialization.md)  
  
## <a name="uniqueness-of-data-in-managed-tls"></a>托管 TLS 中数据的唯一性  

 无论使用线程相对静态字段，还是使用数据槽，托管 TLS 中的数据都是对线程和应用域唯一的数据。  
  
- 在应用域内，一个线程无法修改另一个线程的数据，即使两个线程使用相同的字段或槽，也不例外。  
  
- 如果线程访问多个应用域中的相同字段或槽，每个应用域中都会保留单独的值。  
  
 例如，如果线程设置线程相对静态字段值，并在进入另一个应用域后检索此字段的值，那么在第二个应用域中检索到的值与第一个应用域中的值不同。 对第二个应用域中的字段设置新值不会影响第一个应用域中的字段值。  
  
 同样，如果线程在两个不同的应用域中有同名数据槽，第一个应用域中的数据不受第二个应用域中数据的影响。  
  
## <a name="thread-relative-static-fields"></a>线程相对静态字段  

 如果确定一条数据始终对线程和应用域是唯一的，请向静态字段应用 <xref:System.ThreadStaticAttribute> 属性。 此字段的使用方法与其他任何静态字段一样。 此字段中的数据对使用它的每个线程都是唯一的。  
  
 线程相对静态字段的性能优于数据槽，并支持编译时类型检查。  
  
 请注意，任何类构造函数代码都会在访问此字段的首个上下文中的第一个线程上运行。 在同一应用域中的其他所有线程或上下文中，如果字段是引用类型，便会初始化为 `null`（Visual Basic 中的 `Nothing`）；如果字段是值类型，便会初始化为默认值。 因此，不得依赖类构造函数来初始化线程相对静态字段。 相反，请避免初始化线程相对静态字段，而是假设它们初始化为 `null` (`Nothing`) 或默认值。  
  
## <a name="data-slots"></a>数据槽  

.NET 提供了对于线程和应用程序域都是唯一的动态数据槽。 数据槽分为下列两种类型：命名槽和未命名槽。 两种类型都是使用 <xref:System.LocalDataStoreSlot> 结构实现。  
  
- 若要创建命名数据槽，请使用 <xref:System.Threading.Thread.AllocateNamedDataSlot%2A?displayProperty=nameWithType> 或 <xref:System.Threading.Thread.GetNamedDataSlot%2A?displayProperty=nameWithType> 方法。 若要获取对现有命名槽的引用，请将它的名称传递给 <xref:System.Threading.Thread.GetNamedDataSlot%2A> 方法。  
  
- 若要创建未命名数据槽，请使用 <xref:System.Threading.Thread.AllocateDataSlot%2A?displayProperty=nameWithType> 方法。  
  
 对于命名槽和未命名槽，请使用 <xref:System.Threading.Thread.SetData%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Thread.GetData%2A?displayProperty=nameWithType> 方法设置和检索槽中的信息。 这些静态方法始终处理当前正在执行它们的线程的数据。  
  
 命名槽非常便捷，因为可以在需要时检索槽，具体操作是将它的名称传递给 <xref:System.Threading.Thread.GetNamedDataSlot%2A> 方法，而不用维护对未命名槽的引用。 不过，如果另一个组件对线程相对存储使用相同的名称，并且线程同时执行你的组件和另一个组件的代码，这两个组件可能会相互损坏数据。 （此方案假定这两个组件都在同一个应用域中运行，并不旨在共享相同的数据。）  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ContextStaticAttribute>
- <xref:System.Threading.Thread.GetNamedDataSlot%2A?displayProperty=nameWithType>
- <xref:System.ThreadStaticAttribute>
- <xref:System.Runtime.Remoting.Messaging.CallContext>
- [线程处理](index.md)
