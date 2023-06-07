---
title: 编写单元测试的最佳做法
description: 了解有关编写单元测试的最佳做法，以提高 .NET Core 和 .NET Standard 项目的代码质量和弹性。
author: jpreese
ms.author: wiwagn
ms.date: 07/28/2018
ms.openlocfilehash: 6c1e9a665ad541bf6109634a6df857880ee47042
ms.sourcegitcommit: 74d05613d6c57106f83f82ce8ee71176874ea3f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93281644"
---
# <a name="unit-testing-best-practices-with-net-core-and-net-standard"></a>.NET Core 和 .NET Standard 单元测试最佳做法

编写单元测试有许多好处；它们有助于回归、提供文档和促进良好的设计。 然而，难懂且脆弱的单元测试会对代码库造成严重破坏。 本文介绍一些有关 .NET Core 和 .NET Standard 项目的单元测试设计的最佳做法。

本指南将介绍一些在编写单元测试时的最佳做法，使测试具有弹性且易于理解。

作者是 [John Reese](https://reese.dev) 且特别感谢 [Roy Osherove](https://osherove.com/)

## <a name="why-unit-test"></a>为什么要执行单元测试？

### <a name="less-time-performing-functional-tests"></a>比执行功能测试节省时间

功能测试费用高。 它们通常涉及打开应用程序并执行一系列你（或其他人）必须遵循的步骤，以验证预期的行为。 测试人员可能并不总是了解这些步骤，这意味着为了执行测试，他们必须联系更熟悉该领域的人。 对于细微更改，测试本身可能需要几秒钟，对于较大更改，可能需要几分钟。 最后，在系统中所做的每项更改都必须重复此过程。

而单元测试只需按一下按钮即可运行，只需要几毫秒时间，且无需测试人员了解整个系统。 测试通过与否取决于测试运行程序，而非测试人员。

### <a name="protection-against-regression"></a>防止回归

回归缺陷是在对应用程序进行更改时引入的缺陷。 测试人员通常不仅测试新功能，还要测试预先存在的功能，以验证先前实现的功能是否仍按预期运行。

使用单元测试，可在每次生成后，甚至在更改一行代码后重新运行整套测试。 让你确信新代码不会破坏现有功能。

### <a name="executable-documentation"></a>可执行文档

在给定某个输入的情况下，特定方法的作用或行为可能并不总是很明显。 你可能会想知道：如果我将空白字符串传递给它，此方法会有怎样的行为？ Null?

如果你有一套命名正确的单元测试，每个测试应能够清楚地解释给定输入的预期输出。 此外，它应该能够验证其确实有效。

### <a name="less-coupled-code"></a>减少耦合代码

当代码紧密耦合时，可能难以进行单元测试。 如果不为编写的代码创建单元测试，则耦合可能不太明显。

为代码编写测试会自然地解耦代码，因为采用其他方法测试会更困难。

## <a name="characteristics-of-a-good-unit-test"></a>优质单元测试的特征

- 快速。 对成熟项目进行数千次单元测试，这很常见。 应花非常少的时间来运行单元测试。 几毫秒。
- 独立。 单元测试是独立的，可以单独运行，并且不依赖文件系统或数据库等任何外部因素。
- 可重复。 运行单元测试的结果应该保持一致，也就是说，如果在运行期间不更改任何内容，总是返回相同的结果。
- 自检查。 测试应该能够在没有任何人工交互的情况下，自动检测测试是否通过。
- 及时。 与要测试的代码相比，编写单元测试不应花费过多不必要的时间。 如果发现测试代码与编写代码相比需要花费大量的时间，请考虑一种更易测试的设计。

## <a name="code-coverage"></a>代码覆盖率

高代码覆盖率百分比通常与较高的代码质量相关联。 但该度量值本身无法确定代码的质量。 设置过高的代码覆盖率百分比目标可能会适得其反。 假设一个复杂的项目有数千个条件分支，并且假设你设定了一个 95% 代码覆盖率的目标。 该项目当前维保持 90% 的代码覆盖率。 要覆盖剩余 5% 的所有边缘事例，需要花费巨大的工作量，而且价值主张会迅速降低。

高代码覆盖率百分比不指示成功，也不意味着高代码质量。 它仅仅表示单元测试所涵盖的代码量。 有关详细信息，请参阅[单元测试代码覆盖率](unit-testing-code-coverage.md)。

## <a name="lets-speak-the-same-language"></a>让我们使用相同的术语

遗憾的是，当谈到测试时，术语“mock”经常被滥用。 以下几点定义了编写单元测试时最常见的 fake 类型：

Fake - Fake 是一个通用术语，可用于描述 stub 或 mock 对象。 它是 stub 还是 mock 取决于使用它的上下文。 也就是说，Fake 可以是 stub 或 mock。

Mock - Mock 对象是系统中的 fake 对象，用于确定单元测试是否通过。 Mock 起初为 Fake，直到对其断言。

Stub - Stub 是系统中现有依赖项（或协作者）的可控制替代项。 通过使用 Stub，可以在无需使用依赖项的情况下直接测试代码。 默认情况下，fake 起初为 Stub。

请思考以下代码片段：

```csharp
var mockOrder = new MockOrder();
var purchase = new Purchase(mockOrder);

purchase.ValidateOrders();

Assert.True(purchase.CanBeShipped);
```

这是 Stub 被引用为 mock 的一个示例。 在本例中，它是 Stub。 只是将 Order 作为实例化 `Purchase`（被测系统）的一种方法传递。 名称 `MockOrder` 也具有误导性，因为同样地 order 不是 mock。

更好的方法是

```csharp
var stubOrder = new FakeOrder();
var purchase = new Purchase(stubOrder);

purchase.ValidateOrders();

Assert.True(purchase.CanBeShipped);
```

通过将类重命名为 `FakeOrder`，使类更通用，类可以用作 mock 或 stub。 以更适合测试用例者为准。 在上述示例中，`FakeOrder` 用作 stub。 在断言期间，没有以任何形状或形式使用 `FakeOrder`。 `FakeOrder` 传递到 `Purchase` 类，以满足构造函数的要求。

要将其用作 Mock，可执行如下操作

```csharp
var mockOrder = new FakeOrder();
var purchase = new Purchase(mockOrder);

purchase.ValidateOrders();

Assert.True(mockOrder.Validated);
```

在这种情况下，检查 Fake 上的属性（对其进行断言），因此在以上代码片段中，`mockOrder` 是 Mock。

> [!IMPORTANT]
> 正确理解此术语至关重要。 如果将 stub 称为 mock，其他开发人员会对你的意图做出错误的判断。

关于 mock 与 stub 需要注意的一个重点是，mock 与 stub 很像，但可以针对 mock 对象进行断言，而不针对 stub 进行断言。

## <a name="best-practices"></a>最佳实践

编写单元测试时，尽量不要引入基础结构依赖项。 这些依赖项会降低测试速度，使测试更加脆弱，应将其保留供集成测试使用。 可以通过遵循 [Explicit Dependencies Principle](https://deviq.com/explicit-dependencies-principle)（显式依赖项原则）和使用 [Dependency Injection](../extensions/dependency-injection.md)（依赖项注入）避免应用程序中的这些依赖项。 还可以将单元测试保留在单独的项目中，与集成测试相分隔。 这可确保单元测试项目没有引用或依赖于基础结构包。

### <a name="naming-your-tests"></a>为测试命名

测试的名称应包括三个部分：

- 要测试的方法的名称。
- 测试的方案。
- 调用方案时的预期行为。

#### <a name="why"></a>为什么？

- 命名标准非常重要，因为它们明确地表达了测试的意图。

测试不仅能确保代码有效，还能提供文档。 只需查看单元测试套件，就可以在不查看代码本身的情况下推断代码的行为。 此外，测试失败时，可以确切地看到哪些方案不符合预期。

#### <a name="bad"></a>不佳：

[!code-csharp[BeforeNaming](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeNaming)]

#### <a name="better"></a>良好：

[!code-csharp[AfterNamingAndMinimallyPassing](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterNamingAndMinimallyPassing)]

### <a name="arranging-your-tests"></a>安排测试

“Arrange、Act、Assert”是单元测试时的常见模式。 顾名思义，它包含三个主要操作：

- 安排对象，根据需要对其进行创建和设置。
- 作用于对象。
- 断言某些项按预期进行。

#### <a name="why"></a>为什么？

- 明确地将要测试的内容从“arrange”和“assert”步骤分开 。
- 降低将断言与“Act”代码混杂的可能性。

可读性是编写测试时最重要的方面之一。 在测试中分离这些操作会明确地突出显示调用代码所需的依赖项、调用代码的方式以及尝试断言的内容。 虽然可以组合一些步骤并减小测试的大小，但主要目标是使测试尽可能可读。

#### <a name="bad"></a>不佳：

[!code-csharp[BeforeArranging](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeArranging)]

#### <a name="better"></a>良好：

[!code-csharp[AfterArranging](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterArranging)]

### <a name="write-minimally-passing-tests"></a>以最精简方式编写通过测试

单元测试中使用的输入应为最简单的输入，以便验证当前正在测试的行为。

#### <a name="why"></a>为什么？

- 测试对代码库的未来更改更具弹性。
- 更接近于测试行为而非实现。

包含比通过测试所需信息更多信息的测试更可能将错误引入测试，并且可能使测试的意图变得不太明确。 编写测试时需要将重点放在行为上。 在模型上设置额外的属性或在不需要时使用非零值，只会偏离所要证明的内容。

#### <a name="bad"></a>不佳：

[!code-csharp[BeforeMinimallyPassing](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeMinimallyPassing)]

#### <a name="better"></a>良好：

[!code-csharp[AfterNamingAndMinimallyPassing](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterNamingAndMinimallyPassing)]

### <a name="avoid-magic-strings"></a>避免魔幻字符串

单元测试中的命名变量和生产代码中的命名变量同样重要。 单元测试不应包含魔幻字符串。

#### <a name="why"></a>为什么？

- 测试读者无需检查生产代码即可了解值的特殊之处。
- 明确地显示所要证明的内容，而不是显示要完成的内容 。

魔幻字符串可能会让测试读者感到困惑。 如果字符串看起来不寻常，他们可能想知道为什么为某个参数或返回值选择了某个值。 这可能会使他们仔细查看实现细节，而不是专注于测试。

> [!TIP]
> 编写测试时，应力求表达尽可能多的意图。 对于魔幻字符串，一种很好的方法是将这些值赋给常量。

#### <a name="bad"></a>不佳：

[!code-csharp[BeforeMagicString](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeMagicString)]

#### <a name="better"></a>良好：

[!code-csharp[AfterMagicString](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterMagicString)]

### <a name="avoid-logic-in-tests"></a>在测试中应避免逻辑

编写单元测试时，请避免手动字符串串联和逻辑条件，例如 `if`、`while`、`for` 和 `switch` 等等。

#### <a name="why"></a>为什么？

- 降低在测试中引入 bug 的可能性。
- 专注于最终结果，而不是实现细节。

将逻辑引入测试套件中时，引入 bug 的可能性大幅度增加。 你最不希望测试套件中出现 bug。 你应该对测试工作充满高度的自信，否则你将不会信任它们。 你不信任的测试不会带来任何价值。 当测试失败时，你希望意识到代码存在问题且无法忽略该问题。

> [!TIP]
> 如果不可避免地要在测试中使用逻辑，请考虑将测试分成两个或多个不同的测试。

#### <a name="bad"></a>不佳：

[!code-csharp[LogicInTests](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#LogicInTests)]

#### <a name="better"></a>良好：

[!code-csharp[AfterTestLogic](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterTestLogic)]

### <a name="prefer-helper-methods-to-setup-and-teardown"></a>更偏好 helper 方法而非 setup 和 teardown

如果测试需要类似的对象或状态，那么比起使用 `Setup` 和 `Teardown` 属性（如果存在），更偏好使用 helper 方法。

#### <a name="why"></a>为什么？

- 读者阅读测试时产生的困惑减少，因为每个测试中都可以看到所有代码。
- 给定测试的设置过多或过少的可能性降低。
- 在测试之间共享状态（这会在测试之间创建不需要的依赖项）的可能性降低。

在单元测试框架中，在测试套件的每个单元测试之前调用 `Setup`。 虽然有些人可能会将其视为有用的工具，但它通常最终导致庞大且难懂的测试。 每个测试通常有不同的要求，以使测试启动并运行。 遗憾的是，`Setup` 迫使你对每个测试使用完全相同的要求。

> [!NOTE]
> 自版本 2.x 起，xUnit 已删除 SetUp 和 TearDown

#### <a name="bad"></a>不佳：

[!code-csharp[BeforeSetup](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeSetup)]

```csharp
// more tests...
```

[!code-csharp[BeforeHelperMethod](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeHelperMethod)]

#### <a name="better"></a>良好：

[!code-csharp[AfterHelperMethod](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterHelperMethod)]

```csharp
// more tests...
```

[!code-csharp[AfterSetup](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterSetup)]

### <a name="avoid-multiple-asserts"></a>避免多个断言

在编写测试时，请尝试每次测试只包含一个 Assert。 仅使用一个 assert 的常用方法包括：

- 为每个 assert 创建单独的测试。
- 使用参数化测试。

#### <a name="why"></a>为什么？

- 如果一个 Assert 失败，将不计算后续 Assert。
- 确保在测试中没有断言多个事例。
- 让你从整体上了解测试失败原因。

将多个断言引入测试用例时，不能保证所有断言都会执行。 在大多数单元测试框架中，一旦断言在单元测试中失败，则进行中的测试会自动被视为失败。 这可能会令人困惑，因为正在运行的功能将显示为失败。

> [!NOTE]
> 此规则的一个常见例外是对对象进行断言。 在这种情况下，通常可以对每个属性进行多次断言，以确保对象处于所预期的状态。

#### <a name="bad"></a>不佳：

[!code-csharp[BeforeMultipleAsserts](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeMultipleAsserts)]

#### <a name="better"></a>良好：

[!code-csharp[AfterMultipleAsserts](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterMultipleAsserts)]

### <a name="validate-private-methods-by-unit-testing-public-methods"></a>通过单元测试公共方法验证专有方法

在大多数情况下，不需要测试专用方法。 专用方法是实现细节。 可以这样认为：专用方法永远不会孤立存在。 在某些时候，存在调用专用方法作为其实现的一部分的面向公共的方法。 你应关心的是调用到专用方法的公共方法的最终结果。

请考虑下列情形

```csharp
public string ParseLogLine(string input)
{
    var sanitizedInput = TrimInput(input);
    return sanitizedInput;
}

private string TrimInput(string input)
{
    return input.Trim();
}
```

你的第一反应可能是开始为 `TrimInput` 编写测试，因为想要确保该方法按预期工作。 但是，`ParseLogLine` 完全有可能以一种你所不期望的方式操纵 `sanitizedInput`，使得对 `TrimInput` 的测试变得毫无用处。

真正的测试应该针对面向公共的方法 `ParseLogLine` 进行，因为这是你最终应该关心的。

```csharp
public void ParseLogLine_StartsAndEndsWithSpace_ReturnsTrimmedResult()
{
    var parser = new Parser();

    var result = parser.ParseLogLine(" a ");

    Assert.Equals("a", result);
}
```

由此，如果看到一个专用方法，可以找到公共方法并针对该方法编写测试。 不能仅仅因为专用方法返回预期结果就认为最终调用专用方法的系统正确地使用结果。

### <a name="stub-static-references"></a>Stub 静态引用

单元测试的原则之一是其必须完全控制被测试的系统。 当生产代码包含对静态引用（例如 `DateTime.Now`）的调用时，这可能会存在问题。 考虑下列代码

```csharp
public int GetDiscountedPrice(int price)
{
    if (DateTime.Now.DayOfWeek == DayOfWeek.Tuesday)
    {
        return price / 2;
    }
    else
    {
        return price;
    }
}
```

如何对此代码进行单元测试？ 可以尝试一种方法，例如

```csharp
public void GetDiscountedPrice_NotTuesday_ReturnsFullPrice()
{
    var priceCalculator = new PriceCalculator();

    var actual = priceCalculator.GetDiscountedPrice(2);

    Assert.Equals(2, actual)
}

public void GetDiscountedPrice_OnTuesday_ReturnsHalfPrice()
{
    var priceCalculator = new PriceCalculator();

    var actual = priceCalculator.GetDiscountedPrice(2);

    Assert.Equals(1, actual);
}
```

遗憾的是，你会很快意识到你的测试存在一些问题。

- 如果在星期二运行测试套件，则第二个测试将通过，但第一个测试将失败。
- 如果在任何其他日期运行测试套件，则第一个测试将通过，但第二个测试将失败。

要解决这些问题，需要将“seam”引入生产代码中。 一种方法是在接口中包装需要控制的代码，并使生产代码依赖于该接口。

```csharp
public interface IDateTimeProvider
{
    DayOfWeek DayOfWeek();
}

public int GetDiscountedPrice(int price, IDateTimeProvider dateTimeProvider)
{
    if (dateTimeProvider.DayOfWeek() == DayOfWeek.Tuesday)
    {
        return price / 2;
    }
    else
    {
        return price;
    }
}
```

你的测试套件现在变得

```csharp
public void GetDiscountedPrice_NotTuesday_ReturnsFullPrice()
{
    var priceCalculator = new PriceCalculator();
    var dateTimeProviderStub = new Mock<IDateTimeProvider>();
    dateTimeProviderStub.Setup(dtp => dtp.DayOfWeek()).Returns(DayOfWeek.Monday);

    var actual = priceCalculator.GetDiscountedPrice(2, dateTimeProviderStub);

    Assert.Equals(2, actual);
}

public void GetDiscountedPrice_OnTuesday_ReturnsHalfPrice()
{
    var priceCalculator = new PriceCalculator();
    var dateTimeProviderStub = new Mock<IDateTimeProvider>();
    dateTimeProviderStub.Setup(dtp => dtp.DayOfWeek()).Returns(DayOfWeek.Tuesday);

    var actual = priceCalculator.GetDiscountedPrice(2, dateTimeProviderStub);

    Assert.Equals(1, actual);
}
```

现在，测试套件可以完全控制 `DateTime.Now`，并且在调用方法时可以存根任何值。
