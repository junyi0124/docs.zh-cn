---
title: 代码协定
description: 探索代码协定，它提供了一种方法，用于指定 .NET 代码中的前置条件、后置条件和对象固定。
ms.date: 09/05/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Code contracts
ms.assetid: 84526045-496f-489d-8517-a258cf76f040
ms.openlocfilehash: 60f794373af75bd3f745c224e0a8c7a84192e4c4
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904138"
---
# <a name="code-contracts"></a>代码协定

代码协定提供了在代码中指定前置条件、后置条件和对象固定的方法。 前置条件是输入方法或属性时必须满足的要求。 后置条件描述在方法或属性代码退出时的预期。 对象固定描述处于良好状态的类的预期状态。

代码协定包含用于标记代码的类、用于编译时分析的静态分析器以及运行时分析器。 代码协定的类位于 <xref:System.Diagnostics.Contracts> 命名空间。

代码协定包括以下优点：

- 改进测试：代码协定提供静态协定验证、运行时检查和文档生成。

- 自动测试工具：可通过过滤掉不满足前置条件的无意义的测试参数，使用代码协定来生成更有意义的单元测试。

- 静态验证：静态检查器无需运行程序即可决定是否存在任何协定冲突。 它可检查隐式协定（如 null 取消引用和数组绑定）和显式协定。

- 参考文档：文档生成器扩充具有协定信息的现有 XML 文档文件。 还提供了可与 [Sandcastle ](https://github.com/EWSoftware/SHFB)一起使用的样式表，因此，生成的文档页具有协定部分。

所有 .NET Framework 语言可立即使用协定；无需编写特定分析器或编译器。 Visual Studio 外接程序使你可以指定要执行的代码协定分析的级别。 分析器可确认协定的格式是否标准（类型检查和名称解析），并且可以 Microsoft 中间语言 (MSIL) 格式生成约定的已编译形式。 通过在 Visual Studio 中创建协定，你可以使用工具提供的标准 IntelliSense。

协定类中的大多数方法都进行条件编译；即，编译器仅在你定义特殊符号 CONTRACTS_FULL 时才使用 `#define` 指令发出对这些方法的调用。 借助 CONTRACTS_FULL，你无需需使用 `#ifdef` 指令就可将协定写入代码；还可生成两种不同的版本：一种带有协定，一种未带协定。

有关使用代码协定的工具和详细说明，请参阅 Visual Studio marketplace 网站上的[代码协定](https://marketplace.visualstudio.com/items?itemName=RiSEResearchinSoftwareEngineering.CodeContractsforNET)。

## <a name="preconditions"></a>Preconditions

可使用 <xref:System.Diagnostics.Contracts.Contract.Requires%2A?displayProperty=nameWithType> 方法表达前置条件。 前置条件在方法被调用时指定状态。 它们通常用于指定有效的参数值。 前置条件中提到的所有成员至少都必须与方法本身一样可以访问；否则，方法的调用方可能无法理解此前置条件。 条件必须无副作用。 运行时分析器确定前置条件失败时的运行时行为。

例如，以下前置条件表示参数 `x` 必须为非 null。

```csharp
Contract.Requires(x != null);
```

如果你的代码必须在前置条件失败时引发特定异常，可使用 <xref:System.Diagnostics.Contracts.Contract.Requires%2A> 的泛型重载，如下所示。

```csharp
Contract.Requires<ArgumentNullException>(x != null, "x");
```

### <a name="legacy-requires-statements"></a>旧版需要语句

大多数代码包含一些参数验证，其形式为 `if`-`then`-`throw` 代码。 在以下情况中，协定工具将这些语句识别为前置条件：

- 这些语句显示在方法中任何其他语句之前。

- 整组此类语句后面接显式 <xref:System.Diagnostics.Contracts.Contract> 方法调用，如对 <xref:System.Diagnostics.Contracts.Contract.Requires%2A>、<xref:System.Diagnostics.Contracts.Contract.Ensures%2A>、<xref:System.Diagnostics.Contracts.Contract.EnsuresOnThrow%2A> 或 <xref:System.Diagnostics.Contracts.Contract.EndContractBlock%2A> 方法的调用。

`if`-`then`-`throw` 语句显示为此形式时，工具会将其识别为旧的 `requires` 语句。 如果 `if`-`then`-`throw` 序列后未接其他协定，则代码以 <xref:System.Diagnostics.Contracts.Contract.EndContractBlock%2A?displayProperty=nameWithType> 方法结束。

```csharp
if (x == null) throw new ...
Contract.EndContractBlock(); // All previous "if" checks are preconditions
```

请注意，上述测试中的条件是取反的前置条件。 （实际前提条件为 `x != null` 。）求反的前置条件受到严格限制：必须按前面的示例所示编写：也就是说，它不应包含 `else` 子句，并且子句的主体 `then` 必须是单个 `throw` 语句。 `if` 测试受纯度和可见性规则约束（请参阅[使用准则](#usage_guidelines)），但 `throw` 表达式仅受纯度规则约束。 但是，引发的异常类型必须与发生协定的方法同样可见。

## <a name="postconditions"></a>Postconditions

后置条件是方法终止时的方法的状态协定。 刚好退出方法前检查后置条件。 运行时分析器确定后置条件失败时的运行时行为。

与前置条件不同，后置条件可能引用可见性较低的成员。 客户端可能无法理解或利用后置条件使用私有状态表达的某些信息，但这不影响客户端正确使用方法的能力。

### <a name="standard-postconditions"></a>标准后置条件

可使用 <xref:System.Diagnostics.Contracts.Contract.Ensures%2A> 方法表达标准后置条件。 后置条件表示方法正常终止时必须为 `true` 的条件。

```csharp
Contract.Ensures(this.F > 0);
```

### <a name="exceptional-postconditions"></a>异常后置条件

异常后置条件是在方法引发特定异常时应为 `true` 的后置条件。 可使用 <xref:System.Diagnostics.Contracts.Contract.EnsuresOnThrow%2A?displayProperty=nameWithType> 方法来指定这些后置条件，如下例所示。

```csharp
Contract.EnsuresOnThrow<T>(this.F > 0);
```

自变量是指每次引发作为 `T` 的子类型的异常时必须为 `true` 的条件。

有一些异常类型很难在异常后置条件中使用。 例如，若要使用 `T` 的 <xref:System.Exception> 类型，则无论引发的异常类型如何（即使是堆栈溢出或其他不可控制的异常），方法都必须保证条件。 应仅将异常后置条件用于调用成员时可能引发的特定异常，例如，当对 <xref:System.TimeZoneInfo> 方法调用引发 <xref:System.InvalidTimeZoneException> 时。

### <a name="special-postconditions"></a>特殊后置条件

以下方法可能仅在后置条件中使用：

- 通过使用表达式 `Contract.Result<T>()`（其中 `T` 替换为方法的返回类型），可引用后置条件中的方法返回值。 编译器无法推断出类型时，必须显式提供此类型。 例如，C# 编译器无法推断不带任何参数的方法类型，因此它需要后置条件 `Contract.Ensures(0 <Contract.Result<int>())`。返回类型为 `void` 的方法无法引用后置条件中的 `Contract.Result<T>()`。

- 后置条件中的预状态值是指方法或属性开头的表达式的值。 它使用表达式 `Contract.OldValue<T>(e)`，其中 `T` 是 `e` 的类型。 每次编译器可推断出类型时，都可发出泛型类型参数。 （例如，c # 编译器始终会推断类型，因为它采用了参数。）在中可能出现的情况 `e` 和可能出现旧表达式的上下文中有几个限制。 旧表达式中不能包含其他旧表达式。 最重要的是，旧表达式必须引用方法前置条件状态中的一个值。 换言之，只要方法前置条件为 `true`，此表达式都必须可以进行计算。 以下是此规则的几个实例。

  - 方法的前置条件状态中必须存在值。 若要引用对象上的字段，前提条件必须保证对象始终为非 null。

  - 不能引用旧表达式中方法的返回值：

      ```csharp
      Contract.OldValue(Contract.Result<int>() + x) // ERROR
      ```

  - 不能引用旧表达式中的 `out` 参数。

  - 如果限定符的范围取决于方法的返回值，则旧表达式不能依赖限定符的绑定变量：

      ```csharp
      Contract.ForAll(0, Contract.Result<int>(), i => Contract.OldValue(xs[i]) > 3); // ERROR
      ```

  - 旧表达式不能引用 <xref:System.Diagnostics.Contracts.Contract.ForAll%2A> 或 <xref:System.Diagnostics.Contracts.Contract.Exists%2A> 调用中的匿名委托的参数，除非它被用作方法调用的索引器或自变量：

      ```csharp
      Contract.ForAll(0, xs.Length, i => Contract.OldValue(xs[i]) > 3); // OK
      Contract.ForAll(0, xs.Length, i => Contract.OldValue(i) > 3); // ERROR
      ```

  - 如果旧表达式的值取决于匿名委托的任一参数，匿名委托的主体中不能出现旧表达式，除非匿名委托是 <xref:System.Diagnostics.Contracts.Contract.ForAll%2A> 或 <xref:System.Diagnostics.Contracts.Contract.Exists%2A> 方法的参数：

      ```csharp
      Method(... (T t) => Contract.OldValue(... t ...) ...); // ERROR
      ```

  - `Out` 参数出现问题，因为协定显示在方法主体之前，而大多数编译器不允许引用后置条件中的 `out` 参数。 若要解决此问题，<xref:System.Diagnostics.Contracts.Contract> 类需提供 <xref:System.Diagnostics.Contracts.Contract.ValueAtReturn%2A> 方法，它允许基于 `out` 参数的后置条件。

      ```csharp
      public void OutParam(out int x)
      {
          Contract.Ensures(Contract.ValueAtReturn(out x) == 3);
          x = 3;
      }
      ```

      与 <xref:System.Diagnostics.Contracts.Contract.OldValue%2A> 方法一样，每次编译器能推断出类型时都可发出泛型类型参数。 协定重写程序将方法调用替换为 `out` 参数的值。 <xref:System.Diagnostics.Contracts.Contract.ValueAtReturn%2A> 方法仅可在后置条件中出现。 方法的自变量必须为 `out` 参数或结构 `out` 参数的字段。 在引用结构构造函数后置条件中的字段时，后者也非常有用。

      > [!NOTE]
      > 目前，代码协定分析工具不会检查 `out` 参数是否正确初始化，并且忽略它在后置条件中的描述。 因此，在先前示例中，如果协定后面的行使用了 `x` 的值而不是为其分配一个整数，编译器不会发出正确错误。 但是，在未定义 CONTRACTS_FULL 预处理器符号的版本（如 asa 发布版本）中，编译器将发出错误。

## <a name="invariants"></a>固定协定

对象固定是指无论何时对象对客户端可见都应适合类的每个实例的条件。 它们表示对象被视为正确的条件。

固定协定方法的标识方式是以 <xref:System.Diagnostics.Contracts.ContractInvariantMethodAttribute> 属性进行标记。 除了 <xref:System.Diagnostics.Contracts.Contract.Invariant%2A> 方法的调用序列（其中每个调用指定一个固定协定），固定协定方法不可包含任何代码，如以下示例所示。

```csharp
[ContractInvariantMethod]
protected void ObjectInvariant ()
{
    Contract.Invariant(this.y >= 0);
    Contract.Invariant(this.x > this.y);
    ...
}
```

固定协定由 CONTRACTS_FULL 预处理器符号有条件地进行定义。 在运行时检查期间，每次公共方法结束都要检查固定协定。 如果固定协定提到相同类中的公共方法，则将禁用通常在此公共方法结束时执行的固定协定检查。 相反，仅在此方法的最外层方法调用结束时进行检查。 如果因调用其他类上的方法而重新输入类，也会发生此类情况。 不会检查对象终结器和实现的固定条件 <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> 。

<a name="usage_guidelines"></a>

## <a name="usage-guidelines"></a>使用准则

### <a name="contract-ordering"></a>协定顺序

下表显示编写方法协定时应使用的元素顺序。

|`If-then-throw statements`|向后兼容的公共前置条件|
|-|-|
|<xref:System.Diagnostics.Contracts.Contract.Requires%2A>|所有公共前置条件。|
|<xref:System.Diagnostics.Contracts.Contract.Ensures%2A>|所有的公共（正常）后置条件。|
|<xref:System.Diagnostics.Contracts.Contract.EnsuresOnThrow%2A>|所有的公共（正常）后置条件。|
|<xref:System.Diagnostics.Contracts.Contract.Ensures%2A>|所有的私有/内部（正常）后置条件。|
|<xref:System.Diagnostics.Contracts.Contract.EnsuresOnThrow%2A>|所有的私有/内部（正常）后置条件。|
|<xref:System.Diagnostics.Contracts.Contract.EndContractBlock%2A>|如果使用没有任何其他协定的 `if`-`then`-`throw` 样式前置条件，请调用 <xref:System.Diagnostics.Contracts.Contract.EndContractBlock%2A> 以表示之前所有的 if 检查均为前置条件。|

<a name="purity"></a>

### <a name="purity"></a>纯度

协定中调用的所有方法都必须是纯方法；即，它们不能更新任何预存在的状态。 纯方法可修改输入纯方法后创建的对象。

目前代码协定工具假定以下代码元素为纯元素：

- 标记有 <xref:System.Diagnostics.Contracts.PureAttribute> 的方法。

- 标记有 <xref:System.Diagnostics.Contracts.PureAttribute>（此属性适用于所有类型的方法）的类型。

- 属性 get 访问器。

- 运算符（名称以“op”开头且具有一个或两个参数和非 void 返回类型的静态方法）。

- 完全限定名以“System.Diagnostics.Contracts.Contract”、“System.String”、“System.IO.Path”或“System.Type”开头的所有方法。

- 任何调用的委托，前提条件是委托类型本身具有 <xref:System.Diagnostics.Contracts.PureAttribute> 属性。 委托类型 <xref:System.Predicate%601?displayProperty=nameWithType> 和 <xref:System.Comparison%601?displayProperty=nameWithType> 被视为纯类型。

<a name="visibility"></a>

### <a name="visibility"></a>可见性

协定中提到的所有成员至少必须与包含它们的方法一样可见。 例如，公共方法的前置条件中不可提及私有字段；客户端在调用方法前不可验证此类协定。 但是，如果字段标记有 <xref:System.Diagnostics.Contracts.ContractPublicPropertyNameAttribute>，则不受这些规则限制。

## <a name="example"></a>示例

下面的示例显示了代码协定的用法。

[!code-csharp[ContractExample#1](../../../samples/snippets/csharp/VS_Snippets_CLR/contractexample/cs/program.cs#1)]
[!code-vb[ContractExample#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/contractexample/vb/program.vb#1)]
