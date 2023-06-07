---
title: 使用 CodeDom
description: 使用代码文档对象模型 (CodeDOM) 来组合对象图，该模型提供表示多种常见源代码元素的类型。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- code compilers
- Code Document Object Model
- Code Document Object Model, graphs
- types, CodeDOM
- namespaces [.NET Framework], CodeDOM
- templated code generation
- dynamically representing source code
- source code models
- CodeDOM
- graphing with CodeDOM
- dynamic compilation
- code generators
- CodeDOM, graphs
ms.assetid: 0444ddf3-c3f6-44ed-a999-f710d9c3e0cf
ms.openlocfilehash: f2481aa0423615f5f5925bde7cae3ad170ca8533
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96259175"
---
# <a name="using-the-codedom"></a>使用 CodeDom

CodeDOM 提供表示多种常见源代码元素的类型。 可以设计一个程序，它使用 CodeDOM 元素生成源代码模型来组合对象图。 对于支持的编程语言，可使用 CodeDOM 代码生成器将此对象图呈现为源代码。 还可使用 CodeDOM 将源代码编译为二进制程序集。  
  
 CodeDOM 的常见用途包括：  
  
- 模板化代码生成：生成适用于 ASP.NET、XML Web 服务客户端代理、代码向导、设计器或其他代码发出机制的代码。  
  
- 动态编译：支持一种或多种语言的代码编译。  
  
## <a name="building-a-codedom-graph"></a>生成 CodeDOM 图  

 <xref:System.CodeDom> 命名空间提供用于表示源代码逻辑结构的类，独立于语言语法。  
  
### <a name="the-structure-of-a-codedom-graph"></a>CodeDOM 图的结构  

 CodeDOM 图的结构类似于一个容器树。 每个可编译 CodeDOM 图的顶端容器或根部容器是 <xref:System.CodeDom.CodeCompileUnit>。 源代码模型中的每个元素都必须通过图中 <xref:System.CodeDom.CodeObject> 的属性链接至该图。  
  
### <a name="building-a-source-code-model-for-a-sample-hello-world-program"></a>为示例 Hello World 程序生成源代码模型  

 下列演练提供如何生成 CodeDOM 对象图的示例，用以表示简单 Hello World 应用程序的代码。 有关此代码示例的完整源代码，请参阅 <xref:System.CodeDom.Compiler.CodeDomProvider?displayProperty=nameWithType> 主题。  
  
#### <a name="creating-a-compile-unit"></a>创建编译单元  

 CodeDOM 定义名为 <xref:System.CodeDom.CodeCompileUnit> 的对象，可引用 CodeDOM 对象图，该对象图塑造要编译的源代码的模型。 CodeCompileUnit 具有属性可用于存储对特性、命名空间和程序集的引用  。  
  
 派生自 <xref:System.CodeDom.Compiler.CodeDomProvider> 类的 CodeDom 提供程序包含处理 CodeCompileUnit 所引用的对象图的方法  。  
  
 若要为简单应用程序创建对象图，必须组合源代码模型，并通过 CodeCompileUnit 将其引用  。  
  
 可使用下例所示的语法来创建新的编译单元：  
  
 [!code-cpp[CodeDomExample#12](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#12)]
 [!code-csharp[CodeDomExample#12](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#12)]
 [!code-vb[CodeDomExample#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#12)]  
  
 <xref:System.CodeDom.CodeSnippetCompileUnit> 可以包含已采用目标语言的源代码的一部分，但无法呈现为另一语言。  
  
#### <a name="defining-a-namespace"></a>定义命名空间  

 若要定义命名空间，请创建 <xref:System.CodeDom.CodeNamespace>，并使用适当的构造函数或设置其 Name 属性来为其分配名称  。  
  
 [!code-cpp[CodeDomExample#13](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#13)]
 [!code-csharp[CodeDomExample#13](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#13)]
 [!code-vb[CodeDomExample#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#13)]  
  
#### <a name="importing-a-namespace"></a>导入命名空间  

 若要向命名空间添加命名空间导入指令，请添加 <xref:System.CodeDom.CodeNamespaceImport>，指示将命名空间导入 CodeNamespace.Imports 集合  。  
  
 下列代码添加导入，用于将 System 命名空间导入名为 `samples` 的 CodeNamespace 的 Imports 集合中    ：  
  
 [!code-cpp[CodeDomExample#14](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#14)]
 [!code-csharp[CodeDomExample#14](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#14)]
 [!code-vb[CodeDomExample#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#14)]  
  
#### <a name="linking-code-elements-into-the-object-graph"></a>将代码元素链接至对象图  

 形成 CodeDOM 图的所有代码元素都必须链接至作为树的根元素的 <xref:System.CodeDom.CodeCompileUnit>，方法是通过一系列直接引用自该图根对象属性的元素之间的引用。 将对象设置为容器对象的属性，以建立来自容器对象的引用。  
  
 下列语句将 `samples` CodeNamespace  添加至根 CodeCompileUnit  的 Namespaces  集合属性。  
  
 [!code-cpp[CodeDomExample#15](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#15)]
 [!code-csharp[CodeDomExample#15](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#15)]
 [!code-vb[CodeDomExample#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#15)]  
  
#### <a name="defining-a-type"></a>定义类型  

 若要使用 CodeDOM 声明类、结构、接口或枚举，请创建新的 <xref:System.CodeDom.CodeTypeDeclaration>，然后为其分配名称。 下列示例使用构造函数重载来设置 Name 属性对此进行了演示  ：  
  
 [!code-cpp[CodeDomExample#16](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#16)]
 [!code-csharp[CodeDomExample#16](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#16)]
 [!code-vb[CodeDomExample#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#16)]  
  
 若要向命名空间添加类型，请将代表要添加到命名空间的类型的 <xref:System.CodeDom.CodeTypeDeclaration> 添加到 CodeNamespace 的 Types 集合   。  
  
 下列示例演示如何向名为 `samples` 的 CodeNamespace 添加名为 `class1` 的类  ：  
  
 [!code-cpp[CodeDomExample#17](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#17)]
 [!code-csharp[CodeDomExample#17](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#17)]
 [!code-vb[CodeDomExample#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#17)]  
  
#### <a name="adding-class-members-to-a-class"></a>将类成员添加到类  

 <xref:System.CodeDom> 命名空间提供多种可用于表示类成员的元素。 每个类成员都可添加到 <xref:System.CodeDom.CodeTypeDeclaration> 的 Members 集合  。  
  
#### <a name="defining-a-code-entry-point-method-for-an-executable"></a>为可执行程序定义代码入口点方法  

 若正在为可执行程序生成代码，则务必创建 <xref:System.CodeDom.CodeEntryPointMethod> 来表示应开始程序执行的方法，以指示程序的入口点。  
  
 下列示例演示如何定义入口点方法，该方法包含 <xref:System.CodeDom.CodeMethodInvokeExpression> 来调用 System.Console.WriteLine 以打印“Hello World!”  ：  
  
 [!code-cpp[CodeDomExample#18](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#18)]
 [!code-csharp[CodeDomExample#18](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#18)]
 [!code-vb[CodeDomExample#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#18)]  
  
 下列语句向 `class1` 的 Members 集合添加名为 `Start` 的入口点方法  ：  
  
 [!code-cpp[CodeDomExample#19](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#19)]
 [!code-csharp[CodeDomExample#19](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#19)]
 [!code-vb[CodeDomExample#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#19)]  
  
 现在，名为 `compileUnit` 的 <xref:System.CodeDom.CodeCompileUnit> 包含用于简单 Hello World 程序的 CodeDOM 图。 有关从 CodeDOM 图生成和编译代码的信息，请参阅[从 CodeDOM 图生成源代码并编译程序](generating-and-compiling-source-code-from-a-codedom-graph.md)。  
  
### <a name="more-information-on-building-a-codedom-graph"></a>有关生成 CodeDOM 图的详细信息  

 CodeDOM 支持多种常见的代码元素，可用于支持公共语言运行时的编程语言。 CodeDOM 不是为了提供可表示所有可能的编程语言功能的元素。 可在 <xref:System.CodeDom.CodeSnippetExpression>、<xref:System.CodeDom.CodeSnippetStatement>、<xref:System.CodeDom.CodeSnippetTypeMember> 或 <xref:System.CodeDom.CodeSnippetCompileUnit> 中封装无法通过 CodeDOM 元素轻易表示的代码。 但是，CodeDOM 无法自动将代码片段转换为其他语言。  
  
 有关各种 CodeDOM 类型的文档，请参阅 <xref:System.CodeDom> 命名空间的参考文档。  
  
 有关用于查找表示特定代码元素类型的 CodeDOM 元素的快速图表，请参阅 [CodeDOM 快速参考](/previous-versions/dotnet/netframework-4.0/f1dfsbhc(v=vs.100))。
