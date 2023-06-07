---
title: 如何使用 My 命名空间 - C# 编程指南
description: 了解如何使用“My”命名空间。 “My”命名空间使访问多个 .NET 类变得轻松直观。
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, My namespace access
ms.assetid: e7152414-0ea5-4c8e-bf02-c8d5bbe45ff4
ms.openlocfilehash: 5310b911cc0abf0e82c4dc8efd45eb45ffb94c9d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91176222"
---
# <a name="how-to-use-the-my-namespace-c-programming-guide"></a>如何使用 My 命名空间（C# 编程指南）

<xref:Microsoft.VisualBasic.MyServices> 命名空间（在 Visual Basic 中为 `My`）使访问多个 .NET 类变得轻松直观，让你能够编写与计算机、应用程序、设置、资源等交互的代码。 虽然最初设计用于 Visual Basic，但 `MyServices` 命名空间仍可用于 C# 应用程序。  
  
 有关使用 Visual Basic 的 `MyServices` 命名空间的详细信息，请参阅[使用 My 开发](../../../visual-basic/developing-apps/development-with-my/index.md)。  
  
## <a name="add-a-reference"></a>添加引用

 可以在解决方案中使用 `MyServices` 类之前，必须添加对 Visual Basic 库的引用。  
  
### <a name="add-a-reference-to-the-visual-basic-library"></a>添加对 Visual Basic 库的引用  
  
1. 在解决方案资源管理器中，右键单击“引用”节点并选择“添加引用”  。  
  
2. 出现“引用”对话框时，向下滚动列表，然后选择“Microsoft.VisualBasic.dll”。  
  
     同时建议将以下行包括在程序开头的 `using` 部分。  
  
     [!code-csharp[csProgGuideNamespaces#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces3.cs#18)]  
  
## <a name="example"></a>示例  

 此示例调用 `MyServices` 命名空间中包含的各种静态方法。 若要编译此代码，必须向项目添加对 Microsoft.VisualBasic.DLL 的引用。  
  
 [!code-csharp[csProgGuideNamespaces#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces3.cs#19)]  
  
 并不是 `MyServices` 命名空间中的所有类均可从 C# 应用程序中调用：例如，<xref:Microsoft.VisualBasic.MyServices.FileSystemProxy> 类不兼容。 在此特定情况下，可以改为使用属于 <xref:Microsoft.VisualBasic.FileIO.FileSystem> 的静态方法，这些方法也包含在 VisualBasic.dll 中。 例如，下面介绍了如何使用此类方法来复制目录：  
  
 [!code-csharp[csProgGuideNamespaces#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces3.cs#20)]  
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [命名空间](./index.md)
- [使用命名空间](./using-namespaces.md)
