---
title: 如何：使用 CodeDOM 创建类
description: 请查看详细示例，了解如何使用代码文档对象模型 (CodeDOM) 创建类。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Code Document Object Model, graphs
- Code Document Object Model, creating classes
- graphing with CodeDOM
- CodeDOM, creating classes
- CodeDOM, graphs
ms.assetid: 0ceb70fe-36e1-49bb-922b-e9f615c20a14
ms.openlocfilehash: 7c2cda2bb7cbdb93c27aef91c08f7c7227da7eed
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96235696"
---
# <a name="how-to-create-a-class-using-codedom"></a>如何：使用 CodeDOM 创建类

以下过程说明如何创建和编译 CodeDOM 图，此图会生成包含以下各项的类：两个字段、三个属性、一个方法、一个构造函数和一个入口点。  
  
1. 创建将使用 CodeDOM 代码生成类的源代码的控制台应用程序。  
  
     在此示例中，生成类命名为 `Sample`，生成的代码是名为 SampleCode 的文件中的一个名为 `CodeDOMCreatedClass` 的类。  
  
2. 在生成类中，初始化 CodeDOM 图，并使用 CodeDOM 方法定义生成类的成员、构造函数和入口点（`Main` 方法）。  
  
     在此示例中，生成类包含两个字段、三个属性、一个构造函数、一个方法和一个 `Main` 方法。  
  
3. 在生成类中，创建一个语言特定代码提供程序，并调用其 <xref:System.CodeDom.Compiler.CodeDomProvider.GenerateCodeFromCompileUnit%2A> 方法从该图生成代码。  
  
4. 编译并执行应用程序以生成代码。  
  
     在此示例中，生成的代码位于名为 SampleCode 的文件中。 编译并执行此代码以查看样本输出。  
  
### <a name="to-create-the-application-that-will-execute-the-codedom-code"></a>创建将执行 CodeDOM 代码的应用程序  
  
- 创建要包含 CodeDOM 代码的控制台应用程序类。 定义要在该类中使用的全局字段，以便引用程序集 (<xref:System.CodeDom.CodeCompileUnit>) 和类 (<xref:System.CodeDom.CodeTypeDeclaration>)，指定生成的源文件的名称并声明 `Main` 方法。  
  
     [!code-csharp[CodeDOM Class Sample Main#1](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample Main/CS/program.cs#1)]
     [!code-vb[CodeDOM Class Sample Main#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample Main/VB/program.vb#1)]  
  
### <a name="to-initialize-the-codedom-graph"></a>初始化 CodeDOM 图  
  
- 在控制台应用程序类的构造函数中，初始化程序集和类，并向 CodeDOM 图添加适当的声明。  
  
     [!code-csharp[CodeDOM Class Sample#2](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#2)]
     [!code-vb[CodeDOM Class Sample#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#2)]  
  
### <a name="to-add-members-to-the-codedom-graph"></a>向 CodeDOM 图添加成员  
  
- 通过向类的 <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> 属性添加 <xref:System.CodeDom.CodeMemberField> 对象，向 CodeDOM 图添加字段。  
  
     [!code-csharp[CodeDOM Class Sample#3](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#3)]
     [!code-vb[CodeDOM Class Sample#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#3)]  
  
- 通过向类的 <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> 属性添加 <xref:System.CodeDom.CodeMemberProperty> 对象，向 CodeDOM 图添加属性。  
  
     [!code-csharp[CodeDOM Class Sample#4](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#4)]
     [!code-vb[CodeDOM Class Sample#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#4)]  
  
- 通过向类的 <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> 属性添加 <xref:System.CodeDom.CodeMemberMethod> 对象，向 CodeDOM 图添加方法。  
  
     [!code-csharp[CodeDOM Class Sample#5](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#5)]
     [!code-vb[CodeDOM Class Sample#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#5)]  
  
- 通过向类的 <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> 属性添加 <xref:System.CodeDom.CodeConstructor> 对象，向 CodeDOM 图添加构造函数。  
  
     [!code-csharp[CodeDOM Class Sample#6](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#6)]
     [!code-vb[CodeDOM Class Sample#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#6)]  
  
- 通过向类的 <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> 属性添加 <xref:System.CodeDom.CodeEntryPointMethod> 对象，向 CodeDOM 图添加入口点。  
  
     [!code-csharp[CodeDOM Class Sample#7](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#7)]
     [!code-vb[CodeDOM Class Sample#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#7)]  
  
### <a name="to-generate-the-code-from-the-codedom-graph"></a>从 CodeDOM 图生成代码  
  
- 通过调用 <xref:System.CodeDom.Compiler.CodeDomProvider.GenerateCodeFromCompileUnit%2A> 方法，从 CodeDOM 图生成源代码。  
  
     [!code-csharp[CodeDOM Class Sample#8](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#8)]
     [!code-vb[CodeDOM Class Sample#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#8)]  
  
### <a name="to-create-the-graph-and-generate-the-code"></a>创建图并生成代码  
  
1. 将以上步骤中创建的方法添加到第一步中定义的 `Main` 方法中。  
  
     [!code-csharp[CodeDOM Class Sample#9](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#9)]
     [!code-vb[CodeDOM Class Sample#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#9)]  
  
2. 编译并执行生成类。  
  
## <a name="example"></a>示例  

 以下代码示例显示了上述步骤中的代码。  
  
 [!code-csharp[CodeDOM Class Sample#1](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#1)]
 [!code-vb[CodeDOM Class Sample#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#1)]  
  
 编译和执行以上示例后，会生成以下源代码。  
  
 [!code-csharp[CodeDOM Class Sample#99](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/SampleCode.cs#99)]
 [!code-vb[CodeDOM Class Sample#99](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/SampleCode.vb#99)]  
  
 生成的源代码经过编译和执行后会产生以下输出。  
  
```output
The object:  
 width = 5.3,  
 height = 6.9,  
 area = 36.57  
```  
  
## <a name="compiling-the-code"></a>编译代码  
  
- 需要 `FullTrust` 权限集才可成功执行此代码示例。  
  
## <a name="see-also"></a>请参阅

- [使用 CodeDOM](using-the-codedom.md)
- [从 CodeDOM 图生成和编译源代码](generating-and-compiling-source-code-from-a-codedom-graph.md)
