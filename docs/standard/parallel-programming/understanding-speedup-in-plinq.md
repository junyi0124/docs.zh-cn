---
title: 了解 PLINQ 中的加速
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, performance tuning
ms.assetid: 53706c7e-397d-467a-98cd-c0d1fd63ba5e
ms.openlocfilehash: 64eb346ba57e9af9f5be0cc1b42398c4f539d4d4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95689896"
---
# <a name="understanding-speedup-in-plinq"></a>了解 PLINQ 中的加速

PLINQ 的主要用途是，在多核计算机上并行执行查询委托，以加速执行 LINQ to Objects 查询。 如果单独处理源集合中的每个元素，且各个代理之间不涉及共享状态，PLINQ 的性能最佳。 此类操作在 LINQ to Objects 和 PLINQ 中很常见，通常称为“适合并行”，因为它们可以轻松适应计划多个线程的工作。 不过，并非所有查询完全都由适合并行操作组成；在大多数情况下，查询涉及一些无法并行执行或减慢并行执行的运算符。 即使查询完全都由适合并行组成，PLINQ 仍必须对数据源进行分区，并计划线程工作，通常还需要在查询完成时合并结果。 所有这些操作都增加了并行执行的计算成本；增加并行执行而产生的这些成本称为“开销”。 为了实现 PLINQ 查询的最佳性能，目标是最大限度地增加适合并行执行的部分，并尽量减少需要开销的部分。 本文有助于确保编写的 PLINQ 查询尽可能高效，且仍能产生正确结果。  
  
## <a name="factors-that-impact-plinq-query-performance"></a>PLINQ 查询性能的影响因素  

 下面各部分列出了并行查询性能的一些最重要的影响因素。 这些都是一般性说明，本身并不足以用于在所有情况下预测查询性能。 和以往一样，请务必在具有一系列代表性配置和负载的计算机上度量特定查询的实际性能。  
  
1. 整体工作的计算成本。  
  
     为了实现加速，PLINQ 查询必须有足够多的适合并行操作来抵消开销。 工作可以表示为每个委托的计算成本乘以源集合中的元素数量。 假设操作可以并行执行，它的计算成本越高，加速的机会就越大。 例如，如果函数的执行时间为 1 毫秒，那么超过 1000 个元素的顺序查询需要 1 秒的时间才能执行此操作，而在四核计算机上，并行查询可能只需要 250 毫秒就能完成。 这就产生 750 毫秒的加速。 如果函数执行每个元素需要 1 秒，就会产生 750 秒的加速。 如果委托成本很高，PLINQ 可能会让速度显著提升，前提是源集合中只有几项。 相反，包含最简单的委托的小型源集合通常不适合执行 PLINQ。  
  
     在下面的示例中，queryA 可能很适合执行 PLINQ，前提是它的 Select 函数涉及很多工作。 queryB 可能不适合执行 PLINQ，因为 Select 语句中没有足够多的工作，并行开销会抵消大部分或全部加速。  
  
    ```vb  
    Dim queryA = From num In numberList.AsParallel()  
                 Select ExpensiveFunction(num); 'good for PLINQ  
  
    Dim queryB = From num In numberList.AsParallel()  
                 Where num Mod 2 > 0  
                 Select num; 'not as good for PLINQ  
    ```  
  
    ```csharp  
    var queryA = from num in numberList.AsParallel()  
                 select ExpensiveFunction(num); //good for PLINQ  
  
    var queryB = from num in numberList.AsParallel()  
                 where num % 2 > 0  
                 select num; //not as good for PLINQ  
    ```  
  
2. 系统上的逻辑内核数量（并行度）。  
  
     这一点是上一部分的必然结果，在具有更多内核的计算机上，适合并行查询运行得更快，这是因为可以在更多并发线程之间划分工作。 加速总量取决于查询整体工作的并行度百分比。 不过，不要认为所有查询在八核计算机上的运行速度都是在四核计算机上的两倍。 优化查询以实现最佳性能时，请务必在具有不同数量内核的计算机上度量实际结果。 这一点与第 1 点相关：需要更大的数据集，才能利用更多的计算资源。  
  
3. 操作的数量和种类。  
  
     如果有必要维护源序列中的元素顺序，PLINQ 提供 AsOrdered 运算符。 虽然排序有相关成本，但此成本通常还算低。 GroupBy 和 Join 操作同样也会产生开销。 如果允许按任意顺序处理源集合中的元素，并在准备就绪后立即将它们传递给下一个运算符，PLINQ 的性能最佳。 有关详细信息，请参阅 [PLINQ 中的顺序保留](order-preservation-in-plinq.md)。  
  
4. 查询执行形式。  
  
     若要通过调用 ToArray 或 ToList 存储查询结果，所有并行线程的结果都必须合并到一个数据结构中。 这就涉及不可避免的计算成本。 同样，如果使用 foreach（Visual Basic 中的 For Each）循环来循环访问结果，工作线程的结果必须串行化到枚举器线程。 不过，如果只想根据每个线程的结果执行某操作，可以使用 ForAll 方法对多个线程执行此操作。  
  
5. 合并选项类型。  
  
     PLINQ 可以配置为缓冲输出并在生成整个结果集后分块区生成或一次性全部生成，也可以配置为在各个结果生成时流式传输它们。 前一个导致总体执行时间减少，后一个导致所生成元素之间的延迟减少。  尽管合并选项不一定会对总体查询性能造成重大影响，但它们可能会影响感知性能，因为它们控制用户在看到结果前必须等待的时间。 有关详细信息，请参阅 [PLINQ 中的合并选项](merge-options-in-plinq.md)。  
  
6. 分区种类。  
  
     在某些情况下，对可索引源集合执行 PLINQ 查询可能会导致工作负载不平衡。 如果发生这种情况，可以创建自定义分区程序，从而提升查询性能。 有关详细信息，请参阅 [PLINQ 和 TPL 的自定义分区程序](custom-partitioners-for-plinq-and-tpl.md)。  
  
## <a name="when-plinq-chooses-sequential-mode"></a>如果 PLINQ 选择顺序模式  

 PLINQ 始终都会尝试至少像顺序运行查询一样快地执行查询。 虽然 PLINQ 没有考虑用户委托的计算成本或输入源大小，但它确实会查找特定查询“形状”。 具体来说，它会查找通常会减慢查询在并行模式下的执行速度的查询运算符或运算符组合。 如果找到此类形状，PLINQ 默认会回退到顺序模式。  
  
 不过，在度量特定查询的性能后，可以确定它在并行模式下的实际运行速度更快。 在这种情况下，可以通过 <xref:System.Linq.ParallelEnumerable.WithExecutionMode%2A> 方法使用 <xref:System.Linq.ParallelExecutionMode.ForceParallelism?displayProperty=nameWithType> 标志来指示 PLINQ 并行执行查询。 有关详细信息，请参阅[如何：在 PLINQ 中指定执行模式](how-to-specify-the-execution-mode-in-plinq.md)。  
  
 下面列出了 PLINQ 在顺序模式下默认执行的查询形状：  
  
- 在删除或重新排列了原始索引的排序或筛选运算符后面，包含 Select、已编制索引 Where、已编制索引 SelectMany 或 ElementAt 子句的查询。  
  
- 包含 Take、TakeWhile、Skip、SkipWhile 运算符且源序列中的索引不是原始顺序的查询。  
  
- 包含 Zip 或 SequenceEquals 的查询，除非其中一个数据源具有按原始顺序排列的索引，并且另一个数据源是可索引的（即，数组或 IList(T)）。  
  
- 包含 Concat 的查询，除非应用于可索引的数据源。  
  
- 包含 Reverse 的查询，除非应用于可索引的数据源。  
  
## <a name="see-also"></a>请参阅

- [并行 LINQ (PLINQ)](introduction-to-plinq.md)
