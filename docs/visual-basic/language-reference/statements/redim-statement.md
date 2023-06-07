---
title: ReDim 语句
ms.date: 07/20/2015
f1_keywords:
- vb.ReDim
- vb.Preserve
helpviewer_keywords:
- fixed-length strings [Visual Basic], declaring
- ReDim statement [Visual Basic], syntax
- dynamic arrays [Visual Basic], ReDim statement
- arrays [Visual Basic], reallocating
- arrays [Visual Basic], reinitializing
- arrays [Visual Basic], dimensions
- scalars, and arrays
- scalars
- declarations [Visual Basic], dynamic arrays
- variables [Visual Basic], scalar
- ReDim statement [Visual Basic]
- data types [Visual Basic], assigning
- As keyword [Visual Basic], in ReDim statement
- To keyword [Visual Basic], ReDim statement
- arrays [Visual Basic], declaring
- Preserve keyword [Visual Basic], ReDim statement
- storage [Visual Basic], allocating
- arrays [Visual Basic], and scalars
- declaration statements [Visual Basic]
- scalar variables [Visual Basic]
ms.assetid: ad1c5e07-dcd7-4ae1-a79e-ad3f2dcc2083
ms.openlocfilehash: 17bc806f2e92c61f1dd7425de40b1a68f926a583
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90872023"
---
# <a name="redim-statement-visual-basic"></a>ReDim 语句 (Visual Basic)

为数组变量重新分配存储空间。  
  
## <a name="syntax"></a>语法  
  
```vb  
ReDim [ Preserve ] name(boundlist) [ ,  name(boundlist) [, ... ] ]  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|----------|----------------|  
|`Preserve`|可选。 修饰符，当仅更改最后一个维度的大小时，用来保留现有数组中的数据。|  
|`name`|必需。 数组变量的名称。 请参阅 [Declared Element Names](../../programming-guide/language-features/declared-elements/declared-element-names.md)。|  
|`boundlist`|必需。 列出重新定义的数组各个维度的边界。|  
  
## <a name="remarks"></a>备注  

 可以使用 `ReDim` 语句来更改某个已声明数组的一个或多个维度的大小。 如果数组较大，并且你不再需要它的某些元素，`ReDim`可通过减少数组大小来释放内存。 另一方面，如果数组需要更多元素，也可使用 `ReDim` 进行添加。  
  
 `ReDim` 语句仅供数组使用。 它对标量（仅包含单个值的变量）、集合或结构无效。 请注意，如果将变量声明为 `Array` 类型，则 `ReDim` 语句将没有足够的类型信息来创建新数组。  
  
 仅可在过程级别使用 `ReDim` 。 因此，变量的声明上下文必须是过程；而不能是源文件、命名空间、接口、类、结构、模块或块。 有关详细信息，请参阅[声明上下文和默认访问级别](declaration-contexts-and-default-access-levels.md)。  
  
## <a name="rules"></a>规则  
  
- **多个变量。** 可以在同一声明语句中调整多个数组变量的大小， `name` 并 `boundlist` 为每个变量指定和部分。 以逗号分隔多个变量。  
  
- **数组界限。** 中的每个条目 `boundlist` 都可以指定该维度的下限和上限。 下边界始终为 0（零）。 上边界是该维度可能的最大索引值，而不是维度的长度（即上边界加 1）。 每个维度的索引都可能在 0 到其上边界值之间变动。  
  
     `boundlist` 中的维数必须与数组的原始维数（秩）相匹配。  
  
- **数据类型。** `ReDim`语句无法更改数组变量或其元素的数据类型。  
  
- **起始.** `ReDim`语句不能为数组元素提供新的初始化值。  
  
- **级别.** `ReDim` 语句无法更改数组的秩（维数）。  
  
- **使用 Preserve 重设大小。** 如果使用 `Preserve` ，则只能调整数组最后一个维度的大小。 对于其他每个维度，必须指定现有数组的边界。  
  
     例如，如果数组只有一个维度，则可以重设该维度的大小并依然保留数组的所有内容，因为你更改的是最后一个也是唯一的维度。 但是，如果数组具有两个或更多维度，则使用 `Preserve` 只能重设最后一个维度的大小。  
  
- **属性.** 可以 `ReDim` 在保存值数组的属性上使用。  
  
## <a name="behavior"></a>行为  
  
- **数组替换。** `ReDim` 释放现有数组，并创建一个具有相同秩的新数组。 新数组将替换数组变量中已释放的数组。  
  
- **在不使用 Preserve 的情况下初始化。** 如果不指定，则 `Preserve` `ReDim` 通过使用其数据类型的默认值来初始化新数组的元素。  
  
- **使用 Preserve 初始化。** 如果指定 `Preserve` ，则 Visual Basic 会将元素从现有数组复制到新数组。  
  
## <a name="example"></a>示例  

 下面的示例将增加某个动态数组最后一个维度的大小（不会丢失数组中的任何现有数据），然后减小该大小（会有部分数据丢失）。 最后，它会将大小重新减小到其原始值，并重新初始化所有数组元素。  
  
 [!code-vb[VbVbalrStatements#52](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#52)]  
  
 `Dim` 语句创建具有三个维度的新数组。 为每个维度声明的边界为 10，因此每个维度的数组索引的范围介于 0 到 10 之间。 在下面的讨论中，三个维度分别被称为层、行和列。  
  
 第一个 `ReDim` 创建一个新数组，该数组将替换变量 `intArray` 中的现有数组。 `ReDim` 将所有元素从现有数组复制到新数组中。 它还向每一层中每一行的末尾添加 10 列，并将这些新列中的元素初始化为 0（数组的元素类型 `Integer` 的默认值）。  
  
 第二个 `ReDim` 创建另一个新数组并复制所有适合的元素。 但是，每一层的每一行的末尾丢失了 5 列。 如果不再使用这些列，这不是问题。 减小大型数组的大小可以释放不再需要的内存。  
  
 第三个 `ReDim` 创建另一个新数组，同时从每一层中每一行的末尾删除另外 5 列。 这一次它不会复制任何现有元素。 此语句将数组恢复为其原始大小。 由于该语句不包括 `Preserve` 修饰符，因此它将所有数组元素设置为其原始默认值。  
  
 有关其他示例，请参阅 [数组](../../programming-guide/language-features/arrays/index.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IndexOutOfRangeException>
- [Const 语句](const-statement.md)
- [Dim 语句](dim-statement.md)
- [Erase 语句](erase-statement.md)
- [没](../nothing.md)
- [数组](../../programming-guide/language-features/arrays/index.md)
