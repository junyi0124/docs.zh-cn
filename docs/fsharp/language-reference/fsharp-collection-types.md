---
title: 集合类型
description: '了解 F # 集合类型以及它们与集合类型 .NET 有何不同。'
ms.date: 08/14/2020
ms.openlocfilehash: 0b5be8f656d6728fe382b1944bda0a410a94d226
ms.sourcegitcommit: fe8877e564deb68d77fa4b79f55584ac8d7e8997
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2020
ms.locfileid: "90720330"
---
# <a name="f-collection-types"></a>F # 集合类型

通过查看本主题，可以确定哪个 F # 集合类型最适合特定需求。 这些集合类型与 .NET 中的集合类型（如命名空间中的集合类型）不同，因为 `System.Collections.Generic` F # 集合类型是通过函数编程透视而不是面向对象的透视来设计的。 更具体地说，只有数组集合包含可变元素。 因此，在修改集合时，将创建已修改集合的实例，而不是更改原始集合。

集合类型在存储对象的数据结构类型上也有所不同。 数据结构（如哈希表、链接列表和数组）具有不同的性能特征和一组不同的可用操作。

## <a name="table-of-collection-types"></a>集合类型表

下表显示 F # 集合类型。

|类型|说明|相关链接|
|----|-----------|-------------|
|[列表](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-fsharplist-1.html)|同一类型的有序、不可变的元素系列。 作为链接列表实现。|[列表](lists.md)<br /><br />[List 模块](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html)|
|数组 |固定大小的、从零开始的可变连续数据元素集合，这些元素属于同一类型。|[数组](arrays.md)<br /><br />[Array 模块](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html)<br /><br />[Array2D 模块](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-array2dmodule.html)<br /><br />[Array3D 模块](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-array3dmodule.html)|
|[序列](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seq-1.html)|全部为一种类型的元素的逻辑系列。 如果有大量的有序数据集合，但不一定要使用所有元素，则序列会特别有用。 单个序列元素仅在需要时进行计算，因此，如果不是所有元素都使用，则序列可以比列表更好。 序列由 `seq<'T>` 类型表示，该类型是的别名 `IEnumerable<T>` 。 因此，实现的任何 .NET Framework 类型 `System.Collections.Generic.IEnumerable<'T>` 都可用作序列。|[序列](sequences.md)<br /><br />[Seq 模块](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html)|
|[Map](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-fsharpmap-2.html)|元素的不可变字典。 通过键访问元素。|[地图模块](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-mapmodule.html)|
|[设置](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-fsharpset-1.html)|基于二进制树的不可变集，其中，比较是 F # 结构比较函数，这可能会 `System.IComparable` 对键值使用接口的实现。|[设置模块](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-setmodule.html)|

### <a name="table-of-functions"></a>函数表

本节比较 F # 集合类型上可用的函数。 给定函数的计算复杂性，其中 N 是第一个集合的大小，M 是第二个集合的大小（如果有）。 短划线 (-) 指示此函数在集合上不可用。 由于序列被延迟计算，因此函数（如） `Seq.distinct` 可能是 O (1) ，因为它仍会在枚举时影响序列性能。

|函数|数组|列出|序列|映射|Set|说明|
|--------|-----|----|--------|---|---|-----------|
|append|O (N) |O (N) |O (N) |-|-|返回一个新集合，其中包含第一个集合的元素，后跟第二个集合的元素。|
|add|-|-|-|O (日志 (N) # A3|O (日志 (N) # A3|返回添加了元素的新集合。|
|average|O (N) |O (N) |O (N) |-|-|返回集合中元素的平均值。|
|averageBy|O (N) |O (N) |O (N) |-|-|返回应用于每个元素的所提供函数的结果平均值。|
|array.blit|O (N) |-|-|-|-|复制数组的一部分。|
|cache|-|-|O (N) |-|-|计算并存储序列的元素。|
|强制转换|-|-|O (N) |-|-|将元素转换为指定类型。|
|choose|O (N) |O (N) |O (N) |-|-|将给定函数应用于 `f` 列表的每个元素 `x` 。 返回一个列表，其中包含该函数返回的每个元素的结果 `Some(f(x))` 。|
|collect|O (N) |O (N) |O (N) |-|-|将给定函数应用于集合的每个元素，连接所有结果并返回组合列表。|
|Seq.comparewith|-|-|O (N) |-|-|使用给定的比较函数，按元素比较两个序列。|
|concat|O (N) |O (N) |O (N) |-|-|将给定枚举枚举合并为单个串联的枚举。|
|contains|-|-|-|-|O (日志 (N) # A3|如果集包含指定的元素，则返回 true。|
|containsKey|-|-|-|O (日志 (N) # A3|-|测试元素是否在映射的域中。|
|count|-|-|-|-|O (N) |返回集合中元素的数目。|
|countBy|-|-|O (N) |-|-|将键生成函数应用于序列的每个元素，并返回一个序列，该序列将生成唯一键以及它们在原始序列中出现的次数。|
|copy|O (N) |-|O (N) |-|-|复制集合。|
|create|O (N) |-|-|-|-|创建一个整体元素数组，这些元素最初都是给定的值。|
|delay|-|-|O(1)|-|-|返回一个序列，该序列是根据序列的给定延迟规范生成的。|
|差异|-|-|-|-|O (M \* 日志 (N) # A3|返回一个新集，其中包含从第一个集中删除第二个集的元素。|
|distinct|||O(1)\*|||根据泛型哈希和项的相等比较，返回不包含重复项的序列。 如果某个元素在序列中出现多次，则将放弃后面的匹配项。|
|Seq.distinctby|||O(1)\*|||根据给定的键生成函数返回的键的泛型哈希和相等比较，返回不包含重复项的序列。 如果某个元素在序列中出现多次，则将放弃后面的匹配项。|
|empty|O(1)|O(1)|O(1)|O(1)|O(1)|创建一个空集合。|
|exists|O (N) |O (N) |O (N) |O (日志 (N) # A3|O (日志 (N) # A3|测试序列中是否有任何元素满足给定谓词。|
|array.exists2|O (分钟 (N，M) # A3|-|O (分钟 (N，M) # A3|||测试输入序列的任何对应元素对是否都满足给定谓词。|
|fill|O (N) |||||将数组的一系列元素设置为给定值。|
|filter|O (N) |O (N) |O (N) |O (N) |O (N) |返回一个新集合，其中仅包含给定谓词为其返回的集合的元素 `true` 。|
|find|O (N) |O (N) |O (N) |O (日志 (N) # A3|-|返回给定函数为其返回的第一个元素 `true` 。 `System.Collections.Generic.KeyNotFoundException`如果此类元素不存在，则返回。|
|findIndex|O (N) |O (N) |O (N) |-|-|返回满足给定谓词的数组中第一个元素的索引。 `System.Collections.Generic.KeyNotFoundException`如果任何元素都不满足谓词，则引发。|
|Map.findkey|-|-|-|O (日志 (N) # A3|-|对集合中的每个映射计算函数，并返回函数返回的第一个映射的键 `true` 。 如果此类元素不存在，则此函数将引发 `System.Collections.Generic.KeyNotFoundException` 。|
|折叠|O (N) |O (N) |O (N) |O (N) |O (N) |将函数应用于集合的每个元素，并在整个计算中使用一个累加器参数。 如果输入函数为 f，并且元素为 i0 .。。在中，此函数计算 ( (f s i0) ... ) 。|
|list.fold2|O (N) |O (N) |-|-|-|将函数应用于两个集合的对应元素，并在计算中使用一个累加器参数线程。 集合必须具有相同的大小。 如果输入函数为 f，并且元素为 i0 .。。iN and j0 .。。jN，此函数计算 jN 中的 f ( ... (f s i0 j0) ... ) 。|
|List.foldback|O (N) |O (N) |-|O (N) |O (N) |将函数应用于集合的每个元素，并在整个计算中使用一个累加器参数。 如果输入函数为 f，并且元素为 i0 .。。在中，此函数计算) # A3 中的 f i0 ( ... (f。|
|Array.foldback2|O (N) |O (N) |-|-|-|将函数应用于两个集合的对应元素，并在计算中使用一个累加器参数线程。 集合必须具有相同的大小。 如果输入函数为 f，并且元素为 i0 .。。iN and j0 .。。jN，此函数在 jN s) # A3 中计算 f i0 j0 ( ... (f。|
|forall|O (N) |O (N) |O (N) |O (N) |O (N) |测试集合的所有元素是否都满足给定谓词。|
|array.forall2|O (N) |O (N) |O (N) |-|-|测试集合的所有对应元素是否都满足给定谓词配对。|
|获取/第 n|O(1)|O (N) |O (N) |-|-|根据给定的索引，返回集合中的一个元素。|
|head|-|O(1)|O(1)|-|-|返回集合中的第一个元素。|
|init|O (N) |O (N) |O(1)|-|-|创建给定维度的集合和用于计算元素的生成器函数。|
|Seq.initinfinite|-|-|O(1)|-|-|生成一个序列，当进行迭代时，该序列将通过调用给定函数返回连续的元素。|
|intersect|-|-|-|-|O (日志 (N) \* 日志 (M) # A5|计算两个集的交集。|
|Set.intersectmany|-|-|-|-|O (N1 \* N2 ... ) |计算集序列的交集。 序列不能为空。|
|isEmpty|O(1)|O(1)|O(1)|O(1)|-|`true`如果集合为空，则返回。|
|Set.ispropersubset|-|-|-|-|O (M \* 日志 (N) # A3|`true`如果第一个集的所有元素都位于第二个集合中，则返回; 如果第一组中至少有一个元素不在第一个集合中，则返回。|
|Set.ispropersuperset|-|-|-|-|O (M \* 日志 (N) # A3|`true`如果第二个集的所有元素都在第一个集中，并且第一组中至少有一个元素不在第二个集中，则返回。|
|Set.issubset|-|-|-|-|O (M \* 日志 (N) # A3|`true`如果第一个集合的所有元素均在第二个集中，则返回。|
|Set.issuperset|-|-|-|-|O (M \* 日志 (N) # A3|`true`如果第二个集的所有元素都在第一个集中，则返回。|
|iter|O (N) |O (N) |O (N) |O (N) |O (N) |将给定函数应用于集合的每个元素。|
|array.iteri|O (N) |O (N) |O (N) |-|-|将给定函数应用于集合的每个元素。 传递给函数的整数指示元素的索引。|
|list.iteri2|O (N) |O (N) |-|-|-|将给定函数应用于从两个数组中的匹配索引处提取的元素对。 传递给函数的整数指示元素的索引。 这两个数组的长度必须相同。|
|array.iter2|O (N) |O (N) |O (N) |-|-|将给定函数应用于从两个数组中的匹配索引处提取的元素对。 这两个数组的长度必须相同。|
|last|O(1)|O (N) |O (N) |-|-|返回适用集合中的最后一项。|
|length|O(1)|O (N) |O (N) |-|-|返回集合中元素的数目。|
|map|O (N) |O (N) |O(1)|-|-|生成一个集合，其元素是将给定函数应用于数组的每个元素的结果。|
|array.map2|O (N) |O (N) |O(1)|-|-|生成一个集合，其元素是将给定函数应用于两个集合的对应元素成对的结果。 两个输入数组的长度必须相同。|
|list.map3|-|O (N) |-|-|-|生成一个集合，其元素是将给定函数同时应用于三个集合的对应元素的结果。|
|mapi|O (N) |O (N) |O (N) |-|-|生成一个数组，其元素是将给定函数应用于数组的每个元素的结果。 传递给函数的整数索引指示所转换的元素的索引。|
|list.mapi2|O (N) |O (N) |-|-|-|生成一个集合，其元素是将给定函数应用于两个集合的对应元素的结果，同时传递元素的索引。 两个输入数组的长度必须相同。|
|max|O (N) |O (N) |O (N) |-|-|使用 [max](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-operators.html#max) 运算符，返回集合中的最大元素。|
|maxBy|O (N) |O (N) |O (N) |-|-|返回集合中的最大元素，并将 [max](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-operators.html#max) 与函数结果一起使用。|
|Set.maxelement|-|-|-|-|O (日志 (N) # A3|根据用于集的排序返回集合中的最大元素。|
|分钟|O (N) |O (N) |O (N) |-|-|使用 [min](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-operators.html#min) 运算符，返回集合中的最小元素。|
|minBy|O (N) |O (N) |O (N) |-|-|通过对函数结果使用 [min](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-operators.html#min) 运算符，返回集合中的最小元素。|
|Set.minelement|-|-|-|-|O (日志 (N) # A3|根据用于集的排序返回集合中的最小元素。|
|List.ofarray|-|O (N) |O(1)|O (N) |O (N) |创建一个集合，该集合包含与给定数组相同的元素。|
|Array.oflist|O (N) |-|O(1)|O (N) |O (N) |创建一个集合，该集合包含与给定列表相同的元素。|
|List.ofseq|O (N) |O (N) |-|O (N) |O (N) |创建一个集合，该集合包含与给定序列相同的元素。|
|配对|-|-|O (N) |-|-|返回输入序列中的每个元素及其前置元素的序列，第一个元素除外，该元素仅作为第二个元素的前置元素返回。|
|partition|O (N) |O (N) |-|O (N) |O (N) |将集合拆分成两个集合。 第一个集合包含给定谓词为其返回的元素 `true` ，第二个集合包含给定谓词为其返回的元素 `false` 。|
|permute|O (N) |O (N) |-|-|-|返回一个数组，其中所有元素都根据指定的置换。|
|选取|O (N) |O (N) |O (N) |O (日志 (N) # A3|-|将给定函数应用于连续的元素，并返回函数返回的第一个结果。 如果函数从不返回 Some， `System.Collections.Generic.KeyNotFoundException` 则会引发。|
|readonly|-|-|O (N) |-|-|创建委托给给定序列对象的序列对象。 此操作可确保类型强制转换无法重新发现和改变原始序列。 例如，如果给定数组，则返回的序列将返回数组的元素，但不能将返回的序列对象强制转换为数组。|
|reduce|O (N) |O (N) |O (N) |-|-|将函数应用于集合的每个元素，并在整个计算中使用一个累加器参数。 此函数首先将函数应用于前两个元素，将此结果传递到函数中以及第三个元素，依此类推。 函数返回最终结果。|
|Array.reduceback|O (N) |O (N) |-|-|-|将函数应用于集合的每个元素，并在整个计算中使用一个累加器参数。 如果输入函数为 f，并且元素为 i0 .。。在中，此函数在) # A3 中计算 ( ... (f iN-1。|
|remove|-|-|-|O (日志 (N) # A3|O (日志 (N) # A3|从映射的域中删除一个元素。 如果该元素不存在，则不会引发异常。|
|再现|-|O (N) |-|-|-|创建一个具有指定长度的列表，每个元素设置为给定值。|
|rev|O (N) |O (N) |-|-|-|按相反顺序返回元素的新列表。|
|检测|O (N) |O (N) |O (N) |-|-|将函数应用于集合的每个元素，并在整个计算中使用一个累加器参数。 此操作将函数应用于第二个参数和列表的第一个元素。 然后，该操作会将此结果与第二个元素等一起传递到函数中。 最后，此操作返回中间结果和最终结果的列表。|
|Array.scanback|O (N) |O (N) |-|-|-|类似于 List.foldback 操作，但同时返回中间结果和最终结果。|
|singleton|-|-|O(1)|-|O(1)|返回仅生成一个项的序列。|
|set|O(1)|-|-|-|-|将数组的元素设置为指定值。|
|skip|-|-|O (N) |-|-|返回一个序列，该序列跳过基础序列的 N 个元素，然后生成序列的其余元素。|
|skipWhile|-|-|O (N) |-|-|返回一个序列，该序列在循环访问时跳过基础序列的元素，同时给定谓词返回 `true` ，然后生成序列的其余元素。|
|sort|O (N \* 日志 (n) # A3 average<br /><br />O (N ^ 2) 最坏情况|O (N \* 日志 (n) # A3|O (N \* 日志 (n) # A3|-|-|按元素值对集合进行排序。 使用 [compare](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-operators.html#compare)比较元素。|
|sortBy|O (N \* 日志 (n) # A3 average<br /><br />O (N ^ 2) 最坏情况|O (N \* 日志 (n) # A3|O (N \* 日志 (n) # A3|-|-|使用给定的投影提供的键对给定列表进行排序。 使用 [compare](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-operators.html#compare)比较键。|
|Array.sortinplace|O (N \* 日志 (n) # A3 average<br /><br />O (N ^ 2) 最坏情况|-|-|-|-|使用给定的比较函数就地改变数组的元素，从而对数组的元素进行排序。 使用 [compare](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-operators.html#compare)比较元素。|
|Array.sortinplaceby|O (N \* 日志 (n) # A3 average<br /><br />O (N ^ 2) 最坏情况|-|-|-|-|通过就地改变数组的元素并对键使用给定的投影，对数组的元素进行排序。 使用 [compare](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-operators.html#compare)比较元素。|
|Array.sortinplacewith|O (N \* 日志 (n) # A3 average<br /><br />O (N ^ 2) 最坏情况|-|-|-|-|通过就地改变数组的元素并使用给定的比较函数作为顺序对数组的元素进行排序。|
|Array.sortwith|O (N \* 日志 (n) # A3 average<br /><br />O (N ^ 2) 最坏情况|O (N \* 日志 (n) # A3|-|-|-|使用给定的比较函数作为顺序并返回一个新集合，对集合中的元素进行排序。|
|sub|O (N) |-|-|-|-|生成一个数组，该数组包含由起始索引和长度指定的给定子范围。|
|sum|O (N) |O (N) |O (N) |-|-|返回集合中的元素的和。|
|sumBy|O (N) |O (N) |O (N) |-|-|返回通过将函数应用于集合的每个元素而生成的结果的总和。|
|侧|-|O(1)|-|-|-|返回没有其第一个元素的列表。|
|take|-|-|O (N) |-|-|返回序列中的元素，直到达到指定的计数。|
|takeWhile|-|-|O(1)|-|-|返回一个序列，该序列在循环访问时生成基础序列的元素，同时给定谓词返回 `true` ，然后不返回更多元素。|
|toArray|-|O (N) |O (N) |O (N) |O (N) |从给定集合创建数组。|
|System.linq.enumerable.tolist|O (N) |-|O (N) |O (N) |O (N) |从给定集合创建一个列表。|
|List.toseq|O(1)|O(1)|-|O(1)|O(1)|从给定集合创建一个序列。|
|truncate|-|-|O(1)|-|-|返回一个序列，该序列在枚举后返回不超过 N 个元素。|
|tryFind|O (N) |O (N) |O (N) |O (日志 (N) # A3|-|搜索满足给定谓词的元素。|
|Array.tryfindindex|O (N) |O (N) |O (N) |-|-|搜索满足给定谓词的第一个元素，并返回匹配元素的索引， `None` 如果此类元素不存在，则为。|
|Map.tryfindkey|-|-|-|O (日志 (N) # A3|-|返回集合中满足给定谓词的第一个映射的键， `None` 如果此类元素不存在，则返回。|
|Array.trypick|O (N) |O (N) |O (N) |O (日志 (N) # A3|-|将给定函数应用于连续的元素，并返回函数为某些值返回的第一个结果 `Some` 。 如果此类元素不存在，则该操作返回 `None` 。|
|展开|-|-|O (N) |-|-|返回一个序列，该序列包含给定计算生成的元素。|
|union|-|-|-|-|O (M \* 日志 (N) # A3|计算两个集的并集。|
|Set.unionmany|-|-|-|-|O (N1 \* N2 ... ) |计算集序列的并集。|
|unzip|O (N) |O (N) |O (N) |-|-|将一个对的列表拆分为两个列表。|
|list.unzip3|O (N) |O (N) |O (N) |-|-|将三元组列表拆分为三个列表。|
|开|-|-|O (N) |-|-|返回一个序列，该序列生成包含从输入序列中提取的元素的滑动窗口。 每个窗口都作为一个全新的数组返回。|
|zip|O (N) |O (N) |O (N) |-|-|将两个集合组合为一对的列表。 这两个列表的长度必须相等。|
|list.zip3|O (N) |O (N) |O (N) |-|-|将这三个集合合并为一个三元组列表。 列表的长度必须相等。|

## <a name="see-also"></a>请参阅

- [F# 类型](fsharp-types.md)
- [F# 语言参考](index.md)
