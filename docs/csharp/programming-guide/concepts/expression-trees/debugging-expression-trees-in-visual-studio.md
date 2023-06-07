---
title: 在 Visual Studio 中调试表达式树 (C#)
description: 了解 Visual Studio 中的 DebugView 属性。 了解如何使用此属性分析表达式树的结构和内容。
ms.date: 07/20/2015
ms.assetid: 1369fa25-0fbd-4b92-98d0-8df79c49c27a
ms.openlocfilehash: 5dcf56f96ecdbfdc3f4cb171fdb30b96456b59c9
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91154127"
---
# <a name="debugging-expression-trees-in-visual-studio-c"></a>在 Visual Studio 中调试表达式树 (C#)

可以在调试应用程序时分析表达式树的结构和内容。 要快速了解表达式树结构，可以使用 `DebugView` 属性，该属性[使用特殊语法](debugview-syntax.md)表示表达式树。 （请注意，`DebugView` 仅在调试模式下可用。）  

![VS 调试器中的表达式树的 DebugView 屏幕截图。](media/debugging-expression-trees-in-visual-studio/debugview-expression-tree.png)

由于 `DebugView` 是一个字符串，因此可以使用[内置文本可视化工具](/visualstudio/debugger/view-strings-visualizer#open-a-string-visualizer)在多行中进行查看，方法是从 `DebugView` 标签旁边的放大镜图标中选择“文本可视化工具”。

 ![应用于 DebugView 结果的文本可视化工具的屏幕截图。](media/debugging-expression-trees-in-visual-studio/string-visualizer-debugview.png)

或者，可以为表达式树安装和使用[自定义可视化工具](/visualstudio/debugger/create-custom-visualizers-of-data)，例如：

- [可读表达式](https://github.com/agileobjects/ReadableExpressions)（[MIT 许可证](https://github.com/agileobjects/ReadableExpressions/blob/master/LICENSE.md)，可以从 [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=vs-publisher-1232914.ReadableExpressionsVisualizers) 中获取）使用各种呈现选项将表达式树呈现为可设置主题的 C# 代码：

  ![可读表达式可视化工具的屏幕截图。](media/debugging-expression-trees-in-visual-studio/readable-expressions-visualizer.png)

- [表达式树可视化工具](https://github.com/zspitz/ExpressionTreeVisualizer/blob/master/README.md)（[MIT 许可证](https://github.com/zspitz/ExpressionTreeVisualizer/blob/master/LICENSE)）提供表达式树及其各个节点的树视图：

  ![表达式树可视化工具的屏幕截图。](media/debugging-expression-trees-in-visual-studio/expression-tree-visualizer.png)

### <a name="to-open-a-visualizer-for-an-expression-tree"></a>打开表达式树的可视化工具  
  
1. 单击“数据提示”、“监视”窗口、“自动”窗口或“本地”窗口中表达式树旁边显示的放大镜图标   。  

    显示可用的可视化工具列表：

    ![显示从 Visual Studio 中打开可视化工具的屏幕截图。](media/debugging-expression-trees-in-visual-studio/expression-tree-visualizers.png)

2. 单击要使用的可视化工具。  
  
## <a name="see-also"></a>请参阅

- [表达式树 (C#)](./index.md)
- [在 Visual Studio 中进行调试](/visualstudio/debugger/debugger-feature-tour)
- [创建自定义可视化工具](/visualstudio/debugger/create-custom-visualizers-of-data)
- [`DebugView` 语法](debugview-syntax.md)
