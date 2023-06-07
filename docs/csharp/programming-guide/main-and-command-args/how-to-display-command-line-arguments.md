---
title: 如何显示命令行参数 - C# 编程指南
description: 了解如何显示命令行参数。 查看代码示例和其他可用资源。
ms.date: 07/20/2015
helpviewer_keywords:
- command-line arguments [C#], displaying
ms.assetid: b8479f2d-9e05-4d38-82da-2e61246e5437
ms.openlocfilehash: 717e27c23724e63c03a38b028ef99dc6530b4745
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91195475"
---
# <a name="how-to-display-command-line-arguments-c-programming-guide"></a>如何显示命令行参数（C# 编程指南）

可通过 `Main` 的可选参数来访问在命令行处提供给可执行文件的参数。 参数以字符串数组的形式提供。 数组的每个元素都包含 1 个参数。 删除参数之间的空格。 例如，下面是对虚构可执行文件的命令行调用：  
  
|命令行上的输入|传递给 Main 的字符串数组|  
|----------------------------|-------------------------------------|  
|**executable.exe a b c**|"a"<br /><br /> "b"<br /><br /> “c”|  
|**executable.exe one two**|"one"<br /><br /> "two"|  
|**executable.exe "one two" three**|"one two"<br /><br /> "three"|  
  
> [!NOTE]
> 在 Visual Studio 中运行应用程序时，可在[“项目设计器”->“调试”页](/visualstudio/ide/reference/debug-page-project-designer)中指定命令行参数。  
  
## <a name="example"></a>示例  

 本示例显示了传递给命令行应用程序的命令行参数。 显示的输出对应于上表中的第一项。  
  
 [!code-csharp[csProgGuideMain#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class1.cs#9)]  
  
## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [在命令行上使用 csc.exe 生成](../../language-reference/compiler-options/command-line-building-with-csc-exe.md)
- [Main() 和命令行参数](./index.md)
- [Main() 返回值](./main-return-values.md)
