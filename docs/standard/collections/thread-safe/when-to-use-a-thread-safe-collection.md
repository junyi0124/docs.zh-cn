---
title: 何时使用线程安全集合
description: 了解何时在 .NET 中使用线程安全集合。 有五种专门为支持多线程添加和删除操作而设计的集合类型。
ms.date: 03/30/2017
helpviewer_keywords:
- thread-safe collections, when to upgrade
ms.assetid: a9babe97-e457-4ff3-b528-a1bc940d5320
ms.openlocfilehash: ab1d4d436ce833af94e7eaba35943e499a047a05
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725061"
---
# <a name="when-to-use-a-thread-safe-collection"></a>何时使用线程安全集合

.NET Framework 4 引入了五种专为支持多线程添加和删除操作而设计的集合类型。 为了实现线程安全，这些类型使用多种高效的锁定和免锁定同步机制。 同步会增加操作的开销。 开销数取决于所用的同步类型、执行的操作类型和其他因素，例如尝试并行访问该集合的线程数。  
  
 在某些方案中，同步开销可忽略不计，使多线程类型的执行速度和缩放水平远远超过其受外部锁保护的非线程安全同等类型。 在其他方案中，开销可能会导致线程安全类型的执行速度和缩放水平与该类型外部锁定的非线程安全版本相同，甚至更差。  
  
 以下部分提供有关何时使用线程安全集合与其非线程安全同等集合（其读写操作受用户提供的锁定保护）的通用指南。 由于性能可能因多种因素而异，所以本指南并不针对某特定情况且不一定对所有情况都有效。 如果性能非常重要，那么确定要使用的集合类型的最佳方式是基于典型计算机配置和负载衡量性能。 本文档使用以下术语：  
  
 纯生成方-使用方方案\
 任何给定线程要么添加元素，要么删除元素，两种操作不能同时执行。  
  
 混合生成方-使用方方案\
 任何给定线程可同时添加和删除元素。  
  
 加速\
 相对于同一方案中其他类型更快速的算法性能。  
  
 *可伸缩性*\
 与计算机内核数相称的性能提升。 一种可伸缩的算法，相比两个内核，八个内核上的执行速度更快。  
  
## <a name="concurrentqueuet-vs-queuet"></a>ConcurrentQueue (T) 与 Queue(T)  

 在纯制造者-使用者方案中，每个元素的处理时间都非常短（几条指令），而相比带有外部锁的 <xref:System.Collections.Generic.Queue%601?displayProperty=nameWithType>，<xref:System.Collections.Concurrent.ConcurrentQueue%601?displayProperty=nameWithType> 可提供适度的性能优势。 在此方案中，当某一专用线程排队，而另一专用线程取消排队时，<xref:System.Collections.Concurrent.ConcurrentQueue%601> 的性能最佳。 如果不强制执行此规则，那么 <xref:System.Collections.Generic.Queue%601> 在多内核计算机上的执行速度甚至可能稍快于 <xref:System.Collections.Concurrent.ConcurrentQueue%601>。  
  
 处理时间大约为 500 FLOPS（浮点运算）或更长时，该双线程规则不适用于 <xref:System.Collections.Concurrent.ConcurrentQueue%601>，这将具有很好的可伸缩性。 <xref:System.Collections.Generic.Queue%601> 在此情况下无法正常伸缩。  
  
 在混合制造者-使用者方案中，处理时间非常短时，带外部锁的 <xref:System.Collections.Generic.Queue%601> 的伸缩性优于 <xref:System.Collections.Concurrent.ConcurrentQueue%601>。 但是，处理时间大约为 500 FLOPS 或更长时，<xref:System.Collections.Concurrent.ConcurrentQueue%601> 的伸缩性更佳。  
  
## <a name="concurrentstack-vs-stack"></a>ConcurrentStack 与堆栈  

 在纯制造者-使用者方案中，当处理时间非常短时，<xref:System.Collections.Concurrent.ConcurrentStack%601?displayProperty=nameWithType> 和带外部锁的 <xref:System.Collections.Generic.Stack%601?displayProperty=nameWithType> 在使用一个专用推送线程和一个专用弹出线程时的执行性能可能大致相同。 但是，随着线程数的增加，这两种类型的执行性能会因争用增加而降低，并且 <xref:System.Collections.Generic.Stack%601> 的执行性能可能优于 <xref:System.Collections.Concurrent.ConcurrentStack%601>。 处理时间大约为 500 FLOPS 或更长时，这两种类型的伸缩速率大致相同。  
  
 在混合制造者-使用者方案中，对于小型和大型工作负荷，<xref:System.Collections.Concurrent.ConcurrentStack%601> 的速度更快。  
  
 使用 <xref:System.Collections.Concurrent.ConcurrentStack%601.PushRange%2A> 和 <xref:System.Collections.Concurrent.ConcurrentStack%601.TryPopRange%2A> 可能会大大加快访问速度。  
  
## <a name="concurrentdictionary-vs-dictionary"></a>ConcurrentDictionary 与词典  

 通常，在从多个线程中并行添加和更新键或值的任何方案中，会使用 <xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=nameWithType>。 在涉及频繁更新和相对较少读取操作的方案中，<xref:System.Collections.Concurrent.ConcurrentDictionary%602> 通常具备一些优势。 在涉及许多读取和更新操作的方案中，<xref:System.Collections.Concurrent.ConcurrentDictionary%602> 通常在具备任意数量内核的计算机上运行速度更快。  
  
 在涉及频繁更新的方案中，可以提高 <xref:System.Collections.Concurrent.ConcurrentDictionary%602> 中的并发度，然后进行衡量，查看含有多个内核的计算机的性能是否有所提升。 如果更改并发级别，请尽可能避免全局操作。  
  
 如果仅读取键或值，<xref:System.Collections.Generic.Dictionary%602> 的速度会更快，因为如果字典未被任何线程修改，则无需同步。  
  
## <a name="concurrentbag"></a>ConcurrentBag  

 在纯制造者-使用者方案中，<xref:System.Collections.Concurrent.ConcurrentBag%601?displayProperty=nameWithType> 的执行速度可能慢于其他并发集合类型。  
  
 在混合制造者-使用者方案中，对于大型和小型工作负荷，相比其他任何并发集合类型，往往 <xref:System.Collections.Concurrent.ConcurrentBag%601> 的执行速度更快且伸缩性更佳。  
  
## <a name="blockingcollection"></a>BlockingCollection  

 需要限制和阻止语义时，<xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType> 的执行速度可能优于任何自定义实现。 它还支持诸多取消、枚举和异常处理操作。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [线程安全集合](index.md)
- [并行编程](../../parallel-programming/index.md)
