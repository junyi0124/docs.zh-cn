---
title: 使用 .NET Compiler Platform SDK 语法模型
description: 此概述介绍了用于理解和操作语法节点的类型。
ms.date: 10/15/2017
ms.custom: mvc
ms.openlocfilehash: fdb13095c2b91e54d58988a51a51b05652e57ea6
ms.sourcegitcommit: 488aced39b5f374bc0a139a4993616a54d15baf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/12/2020
ms.locfileid: "83208390"
---
# <a name="work-with-syntax"></a>使用语法

“语法树”是一种由编译器 API 公开的基础数据结构。 这些树表示源代码的词法和语法结构。 它们有两个重要用途：

- 支持使用工具（如 IDE、加载项、代码分析工具和重构）查看和处理用户项目中源代码的语法结构。
- 支持使用工具（如重构和 IDE）以自然的方式创建、修改和重新排列源代码，而无需直接编辑文本。 通过创建和操作语法树，可轻松使用工具创建和重新排列源代码。

## <a name="syntax-trees"></a>语法树

语法树是用于编译、代码分析、绑定、重构、IDE 功能和代码生成的主要结构。 要理解任意部分的源代码，都必须先加以识别，然后将其分类为众多已知结构化语言元素之一。

语法树具有三个关键特性。 第一个特性是语法树完全保真地保留所有源代码信息。 完全保真意味着语法树包含可在源文本中找到的每份信息、每个语法结构、每个词法标记，以及它们之间的所有其他内容，包括空格、注释和预处理器指令。 例如，源中提及的所有文本都完全按照键入的形式表示。 语法树还可在程序不完整或格式错误时，通过表示出已跳过或缺少的标记，来捕获源代码中的错误。

语法树的第二个特性是它们可以生成从中进行分析的确切文本。 可从任何语法节点获取以该节点为根的子树的文本表示形式。 此功能意味着语法树可以用作一种构造和编辑源文本的方法。 创建树即会隐式创建等效文本，编辑语法树会创建新的树，而不会更改现有树，通过这些操作，可高效编辑文本。

语法树的第三个特性是它们是恒定不变的，也是线程安全的。 获取的树是代码当前状态的快照，不会更改。 这可让多个用户同时在不同线程中与同一语法树进行交互，而不会锁定或重复。 由于语法树恒定不变，并且不可直接对其进行修改，因此工厂方法可通过创建树的另一个快照来帮助创建和修改语法树。 语法树可高效重用基础节点，因此几乎无需使用额外的内存便可快速重新生成新版本。

语法树实际上是一个树形数据结构，其中非终端结构化元素是其他元素的父元素。 每个语法树都由节点、标记和琐碎内容组成。

## <a name="syntax-nodes"></a>语法节点

语法节点是语法树的一个主要元素。 这些节点表示声明、语句、子句和表达式等语法构造。 语法节点的每个类别都由派生自 <xref:Microsoft.CodeAnalysis.SyntaxNode?displayProperty=nameWithType> 的单独类表示。 节点类集是不可扩展的。

所有语法节点都是语法树中的非终端节点，这意味着这些节点始终有其他节点和标记作为子元素。 作为另一个节点的子级，每个节点都具有可通过 <xref:Microsoft.CodeAnalysis.SyntaxNode.Parent?displayProperty=nameWithType> 属性访问的父节点。 由于节点和树恒定不变，因此节点的父节点永远不会更改。 树的根以 null 为父级。

每个节点都包含 <xref:Microsoft.CodeAnalysis.SyntaxNode.ChildNodes?displayProperty=nameWithType> 方法，可根据子节点在源文本中的位置按顺序返回子节点列表。 此列表中不包含标记。 每个节点还包含用于检查子代的方法（例如，<xref:Microsoft.CodeAnalysis.SyntaxNode.DescendantNodes%2A>、<xref:Microsoft.CodeAnalysis.SyntaxNode.DescendantTokens%2A> 或 <xref:Microsoft.CodeAnalysis.SyntaxNode.DescendantTrivia%2A>），这些子代表示以该节点为根的子树中存在的所有节点、标记或琐碎内容的列表。

此外，每个语法节点子类通过强类型属性公开所有相同的子级。 例如，<xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax> 节点类具有三个特定于二元运算符的其他属性：<xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.Left>、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.OperatorToken> 和 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.Right>。 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.Right> 和 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.ExpressionSyntax> 的类型为 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.Left>，<xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.OperatorToken> 的类型为 <xref:Microsoft.CodeAnalysis.SyntaxToken>。

某些语法节点具有可选子级。 例如，<xref:Microsoft.CodeAnalysis.CSharp.Syntax.IfStatementSyntax> 具有可选的 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.ElseClauseSyntax>。 如果没有子级，则该属性返回 null。

## <a name="syntax-tokens"></a>语法标记

语法标记是语言语法的终端，表示代码的最小语法片段。 它们从不作为其他节点或标记的父级。 语法标记包含关键字、标识符、文本和标点。

为了提高效率，<xref:Microsoft.CodeAnalysis.SyntaxToken> 类型为 CLR 值类型。 因此，与语法节点不同，所有类型的标记都采用同一结构，但包含各种属性，这些属性的意义取决于表示的标记类型。

例如，整数文本标记表示数字值。 除了原始源文本和标记范围外，文本标记还包含 <xref:Microsoft.CodeAnalysis.SyntaxToken.Value> 属性，用于告知精确解码的整数值。 此属性类型化为 <xref:System.Object>，因为它可能属于多个基元类型之一。

<xref:Microsoft.CodeAnalysis.SyntaxToken.ValueText> 属性与 <xref:Microsoft.CodeAnalysis.SyntaxToken.Value> 属性告知相同的信息；但前者始终类型化为 <xref:System.String>。 C# 源文本中的标识符可能包括 Unicode 转义字符，但转义序列本身的语法不被视为标识符名称的一部分。 因此，虽然标记跨越的原始文本包含转义序列，但 <xref:Microsoft.CodeAnalysis.SyntaxToken.ValueText> 属性却不包含转义序列。 而是包括转义识别的 Unicode 字符。 例如，如果源文本包含写为 `\u03C0` 的标识符，则此标记的 <xref:Microsoft.CodeAnalysis.SyntaxToken.ValueText> 属性返回 `π`。

## <a name="syntax-trivia"></a>语法琐碎内容

语法琐碎内容表示对正常理解代码基本上没有意义的源文本部分，例如空格、注释和预处理器指令。 与语法标记类似，琐碎内容为值类型。 单个 <xref:Microsoft.CodeAnalysis.SyntaxTrivia?displayProperty=nameWithType> 类型用于描述各种类型的琐碎内容。

由于琐碎内容不是正常语言语法的一部分，并且可以出现在任意两个标记之间的任意位置，因此它们不会作为节点的子级包含在语法树中。 但由于它们对于实现重构等功能和完全保真地保留源文本非常重要，因此还是包含在语法树内。

可通过检查标记的 <xref:Microsoft.CodeAnalysis.SyntaxToken.LeadingTrivia?displayProperty=nameWithType> 或 <xref:Microsoft.CodeAnalysis.SyntaxToken.TrailingTrivia?displayProperty=nameWithType> 集合来访问琐碎内容。 分析源文本时，琐碎内容序列与标记关联。 通常情况下，一个标记拥有其后位于同一行中下一个标记之前的任意琐碎内容。 该行之后的任意琐碎内容与下一个标记关联。 源文件中的第一个标记可获取所有初始琐碎内容，而最后一个琐碎内容序列附加到文件尾标记，否则其宽度为零。

与语法节点和语法标记不同，语法琐碎内容没有父级。 但由于它们是树的一部分，且每个琐碎内容都与一个标记关联，因此可使用 <xref:Microsoft.CodeAnalysis.SyntaxTrivia.Token?displayProperty=nameWithType> 属性访问关联的标记。

## <a name="spans"></a>范围

每个节点、标记或琐碎内容都知道其在源文本内的位置和包含的字符数。 文本位置表示为一个 32 位整数，是一个从零开始的 `char` 索引。 <xref:Microsoft.CodeAnalysis.Text.TextSpan> 对象表示开始位置和字符计数，都表示为整数。 如果 <xref:Microsoft.CodeAnalysis.Text.TextSpan> 的长度为零，则其表示两个字符之间的位置。

每个节点具有两个 <xref:Microsoft.CodeAnalysis.Text.TextSpan> 属性：<xref:Microsoft.CodeAnalysis.SyntaxNode.Span%2A> 和 <xref:Microsoft.CodeAnalysis.SyntaxNode.FullSpan%2A>。

<xref:Microsoft.CodeAnalysis.SyntaxNode.Span%2A> 属性表示从节点子树中第一个标记的开头到最后一个标记末尾的文本范围。 此范围不包括任何前导或尾随琐碎内容。

<xref:Microsoft.CodeAnalysis.SyntaxNode.FullSpan%2A> 属性表示的文本范围包括节点的正常范围，加上任何前导或尾随琐碎内容的范围。

例如：

``` csharp
      if (x > 3)
      {
||        // this is bad
          |throw new Exception("Not right.");|  // better exception?||
      }
```

块内语句节点的范围由单个竖线 (|) 指示。 它包含字符 `throw new Exception("Not right.");`。 完整的范围由双竖线 (||) 指示。 它包含的字符与前导和尾随琐碎内容的相关范围和字符相同。

## <a name="kinds"></a>种类

每个节点、标记或琐碎内容都具有 <xref:System.Int32?displayProperty=nameWithType> 类型的 <xref:Microsoft.CodeAnalysis.SyntaxNode.RawKind?displayProperty=nameWithType> 属性，标识所表示的确切语法元素。 此值可强制转换为特定语言的枚举。 每种语言（C# 或 Visual Basic）都具有单个 `SyntaxKind` 枚举（分别为 <xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind?displayProperty=nameWithType> 和 <xref:Microsoft.CodeAnalysis.VisualBasic.SyntaxKind?displayProperty=nameWithType>），列出了语法中所有可能的节点、标记和琐碎内容。 可通过访问 <xref:Microsoft.CodeAnalysis.CSharp.CSharpExtensions.Kind%2A?displayProperty=nameWithType> 或 <xref:Microsoft.CodeAnalysis.VisualBasic.VisualBasicExtensions.Kind%2A?displayProperty=nameWithType> 扩展方法自动完成此转换。

<xref:Microsoft.CodeAnalysis.SyntaxToken.RawKind> 属性可轻松消除共享同一节点类的语法节点类型的歧义。 对于标记和琐碎内容，此属性是区分不同元素类型的唯一方法。

例如，一个 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax> 类具有 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.Left>、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.OperatorToken> 和 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.Right> 作为子级。 <xref:Microsoft.CodeAnalysis.CSharp.CSharpExtensions.Kind%2A> 属性可辨别它是 <xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind.AddExpression>、<xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind.SubtractExpression> 还是 <xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind.MultiplyExpression> 类型的语法节点。

## <a name="errors"></a>错误

即使源文本中包含语法错误，也会公开可与源双向转换的完整语法树。 当分析程序遇到不符合语言定义语法的代码时，使用两种方法之一来创建语法树：

- 如果分析程序需要特定种类的标记，但未找到该标记，则其可在语法树中所需标记位置插入缺失的标记。 缺失的标记表示本应存在，但只有空范围的实际标记，并且其 <xref:Microsoft.CodeAnalysis.SyntaxNode.IsMissing?displayProperty=nameWithType> 属性返回 `true`。

- 分析程序可能会跳过标记，直至发现可继续分析的标记。 在这种情况下，跳过的令牌附加为 <xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind.SkippedTokensTrivia> 类型的琐碎内容节点。
