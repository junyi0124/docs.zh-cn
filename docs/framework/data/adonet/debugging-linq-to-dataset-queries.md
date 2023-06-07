---
title: 调试 LINQ to DataSet 查询
ms.date: 03/30/2017
ms.assetid: f4c54015-8ce2-4c5c-8d18-7038144cc66d
ms.openlocfilehash: 638198ef4c78b84dd12c3d39f83bf8a015e566c1
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183190"
---
# <a name="debugging-linq-to-dataset-queries"></a>调试 LINQ to DataSet 查询

Visual Studio 支持调试 LINQ to DataSet 代码。 不过，调试 LINQ to DataSet 代码和非 LINQ to DataSet 托管代码之间存在一些差异。 大多数调试功能都使用 LINQ to DataSet 语句，包括单步执行、设置断点以及查看调试器窗口中显示的结果。 但是，中延迟的查询执行具有一些副作用，你应该在调试 LINQ to DataSet 代码时考虑这些副作用，并且在使用 "编辑并继续" 时会有一些限制。 本主题讨论与非 LINQ to DataSet 托管代码 LINQ to DataSet 唯一的调试方面。  
  
## <a name="viewing-results"></a>查看结果  

 您可以使用 "数据提示"、"监视窗口" 和 "快速监视" 对话框查看 LINQ to DataSet 语句的结果。 使用源窗口，可以将指针暂时停留在源窗口中的查询上，这样数据提示就会出现。 可以复制 LINQ to DataSet 变量，并将其粘贴到监视窗口或 "快速监视" 对话框中。 在 LINQ to DataSet 中，不会在创建或声明查询时对其进行计算，而只在执行查询时进行计算。 这称为 " *延迟执行*"。 因此，查询变量直到计算时才有值。 有关详细信息，请参阅 [LINQ to DataSet 中的查询](queries-in-linq-to-dataset.md)。  
  
 调试程序必须计算查询才能显示查询结果。 当您在调试器中查看 LINQ to DataSet 查询结果时，将发生这种隐式计算，并且它有一些应考虑的效果。 查询的每次计算都需要时间。 展开结果节点需要时间。 对于某些查询，重复计算可能引起明显的性能损失。 计算查询也会有副作用，这些副作用会更改数据值或程序的状态。 不是所有查询都具有副作用。 要确定查询能否安全计算而没有副作用，必须了解实现查询的代码。 有关详细信息，请参阅 [副作用和表达式](/previous-versions/visualstudio/visual-studio-2013/a7a250bs(v=vs.120))。  
  
## <a name="edit-and-continue"></a>编辑并继续  

 "编辑并继续" 不支持 LINQ to DataSet 查询的更改。 如果在调试会话过程中添加、删除或更改 LINQ to DataSet 语句，则会出现一个对话框，告知您 "编辑并继续" 不支持该更改。 此时，可以撤消更改，或停止调试会话并对编辑的代码重新启动新会话。  
  
 此外，"编辑并继续" 不支持更改 LINQ to DataSet 语句中使用的变量的类型或值。 同样，可以撤消更改，或停止并重新启动调试会话。  
  
 在 Visual Studio 的 Visual c # 中，不能对包含 LINQ to DataSet 查询的方法中的任何代码使用 "编辑并继续"。  
  
 在 Visual Studio Visual Basic 中，可以对非 LINQ to DataSet 代码使用 "编辑并继续"，即使是在包含 LINQ to DataSet 查询的方法中也是如此。 您可以在 LINQ to DataSet 语句之前添加或移除代码，即使这些更改会影响 LINQ to DataSet 查询的行号。 非 LINQ to DataSet 代码的 Visual Basic 调试体验与引入 LINQ to DataSet 之前相同。 不过，除非停止调试以应用更改，否则不能更改、添加或删除 LINQ to DataSet 查询。  
  
## <a name="see-also"></a>请参阅

- [调试托管代码](/visualstudio/debugger/debugging-managed-code)
- [编程指南](programming-guide-linq-to-dataset.md)
