---
title: using 命名空间 - C# 编程指南
description: 了解如何在 C# 编程中使用命名空间，例如访问命名空间、命名空间别名，以及使用命名空间控制范围。
ms.date: 07/20/2015
f1_keywords:
- cs.names
helpviewer_keywords:
- fully qualified names [C#]
- namespaces [C#], how to use
ms.assetid: 1fe8bf39-addc-438a-bd9e-86410e32381d
ms.openlocfilehash: 86892f50e059c16ee9c133d7ba9f2716e11a8e56
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381692"
---
# <a name="using-namespaces-c-programming-guide"></a>using 命名空间（C# 编程指南）

在 C# 编程中，命名空间在两个方面被大量使用。 首先，.NET 类使用命名空间来组织它的众多类。 其次，在较大的编程项目中，声明自己的命名空间可以帮助控制类名称和方法名称的范围。  
  
## <a name="accessing-namespaces"></a>访问命名空间

 大多数 C# 应用程序以 `using` 指令开头。 本部分列出了应用程序将频繁使用的命名空间，使程序员避免在每次使用包含在命名空间中的方法时都要指定完全限定名称。  
  
 例如，通过包括行：  
  
 [!code-csharp[csProgGuide#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/using.cs#1)]  
  
 在程序的开始，程序员可以使用代码：  
  
 [!code-csharp[csProgGuide#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#31)]  
  
 而不是：  
  
 [!code-csharp[csProgGuide#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#30)]  
  
## <a name="namespace-aliases"></a>命名空间别名

 还可以使用 [`using` 指令](../../language-reference/keywords/using-directive.md)为命名空间创建别名。 使用[命名空间别名限定符 `::`](../../language-reference/operators/namespace-alias-qualifier.md) 访问已设置别名的命名空间的成员。 下面的示例演示如何创建和使用命名空间别名：
  
[!code-csharp[csProgGuideNamespaces#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#5)]
  
## <a name="using-namespaces-to-control-scope"></a>使用命名空间控制范围

 `namespace` 关键字用于声明范围。 在项目内创建范围有助于整理代码，并允许创建全局唯一类型。 在下面的示例中，名为 `SampleClass` 的类在两个命名空间中定义，其中一个嵌套在另一个之中。 [`.` 令牌](../../language-reference/operators/member-access-operators.md#member-access-expression-)用于区分调用的方法。  
  
 [!code-csharp[csProgGuideNamespaces#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#8)]  
  
## <a name="fully-qualified-names"></a>完全限定的名称

 命名空间和类型具有指示逻辑层次结构的完全限定名称所描述的唯一标题。 例如，语句 `A.B` 意味着 `A` 是命名空间或类型的名称，而 `B` 嵌套在其中。  
  
 以下示例中具有嵌套的类和命名空间。 完全限定名称表示为跟随每个实体的注释。  
  
 [!code-csharp[csProgGuideNamespaces#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#9)]  
  
 在前面的代码片段中：  
  
- 命名空间 `N1` 是全局命名空间的成员。 其完全限定名称为 `N1`。  
  
- 命名空间 `N2` 是 `N1` 的成员。 其完全限定名称为 `N1.N2`。  
  
- 类 `C1` 是 `N1` 的成员。 其完全限定名称为 `N1.C1`。  
  
- 类名 `C2` 在此代码中使用了两次。 但完全限定名称是唯一的。 `C2` 的第一个实例在 `C1` 中声明；因此，其完全限定名称是：`N1.C1.C2`。 `C2` 的第二个实例在命名空间 `N2` 中声明；因此，其完全限定名称是 `N1.N2.C2`。  
  
 使用前面的代码段，可向命名空间 `N1.N2` 添加新的类成员 `C3`，如下所示：  
  
 [!code-csharp[csProgGuideNamespaces#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#10)]  
  
 一般情况下，使用[命名空间别名限定符 `::`](../../language-reference/operators/namespace-alias-qualifier.md) 来引用命名空间别名，或使用 `global::` 来引用全局命名空间，以及使用 `.` 来限定类型或成员。  
  
 将 `::` 与引用类型而非引用命名空间的别名一起使用是错误的。 例如：  
  
 [!code-csharp[csProgGuideNamespaces#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces2.cs#11)]  
  
 [!code-csharp[csProgGuideNamespaces#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces2.cs#12)]  
  
 请记住，单词 `global` 不是预定义的别名；因此，`global.X` 不具有任何特殊含义。 仅当将其与 `::` 一起使用时，它才具有特殊意义。  
  
 定义名为 global 的别名时将生成编译器警告 CS0440，因为 `global::` 始终引用全局命名空间而非别名。 例如，以下行将生成该警告：  
  
 [!code-csharp[csProgGuideNamespaces#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces2.cs#13)]  
  
 将 `::` 与别名一起使用是一个不错的主意，可防止意外引入其他类型。 例如，请考虑以下示例：  
  
 [!code-csharp[csProgGuideNamespaces#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#14)]  
  
 [!code-csharp[csProgGuideNamespaces#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#15)]  
  
 这是可行的，但如果随后要引入名为 `Alias` 的类型，`Alias.` 将改为绑定到该类型。 使用 `Alias::Exception` 确保 `Alias` 被视为命名空间别名，而不被误解为类型。  

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [命名空间](./index.md)
- [成员访问表达式](../../language-reference/operators/member-access-operators.md#member-access-expression-)
- [:: 运算符](../../language-reference/operators/namespace-alias-qualifier.md)
- [外部别名](../../language-reference/keywords/extern-alias.md)
