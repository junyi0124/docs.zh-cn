---
title: 编写大型的响应式 .NET Framework 应用
description: 编写大型的响应式 .NET 应用程序或处理大量数据的应用程序，如文件或数据库。
ms.date: 03/30/2017
ms.assetid: 123457ac-4223-4273-bb58-3bc0e4957e9d
author: BillWagner
ms.author: wiwagn
ms.openlocfilehash: 20b9f34595f8586eb5162715eb6b0df171f132a1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96266839"
---
# <a name="writing-large-responsive-net-framework-apps"></a>编写大型的响应式 .NET Framework 应用

本文提供用于改进大型 .NET Framework 应用或处理大量数据（如文件或数据库）的应用的性能的提示。 这些提示来自在托管代码中重写的 C# 和 Visual Basic 编译器，并且本文包括来自 C# 编译器的几个真实示例。
  
.NET Framework 构建应用的效率极高。 功能强大且安全的语言以及丰富的库集合使应用构建富有成果。 然而，伴随高效率而来的是责任问题。 你应该使用 .NET Framework 的所有功能，但随时准备在需要时调整你的代码性能。
  
## <a name="why-the-new-compiler-performance-applies-to-your-app"></a>为什么新的编译器性能适用于你的应用  

 .NET Compiler Platform ("Roslyn") 团队在托管代码中重写了 C# 和 Visual Basic 编译器，以提供新的 API 来建模和分析代码、构建工具，并在 Visual Studio 中实现更丰富、代码识别的体验。 重写编译器和在新编译器上构建 Visual Studio 体验揭示了实用的性能见解，该见解适用于任何大型 .NET Framework 应用或任何用于处理大量数据的应用。 要利用来自 C# 编译器的见解和示例，你并不需要了解编译器本身。
  
 Visual Studio 使用编译器 API 来构建所有用户喜爱的 IntelliSense 功能，例如标识符和关键字着色、语法完成列表、错误的波形曲线、参数提示、代码问题报告以及代码操作。 Visual Studio 可在开发人员键入和更改他们的代码时提供这种帮助，并且 Visual Studio 必须在编译器不断建模开发人员编辑的代码时保持响应。
  
 当你的最终用户与你的应用交互时，他们期望应用能够响应。 永远不应阻止键入或命令处理。 帮助应该迅速弹出，如果用户继续键入则帮助应中止。 你的应用应该避免通过使应用感觉迟钝的长计算阻止 UI 线程。
  
 有关 Roslyn 编译器的详细信息，请参阅 [.NET COMPILER PLATFORM SDK](../../csharp/roslyn-sdk/index.md)。
  
## <a name="just-the-facts"></a>事实小结  

 在优化性能和创建响应性 .NET Framework 应用时，请考虑以下事实。
  
### <a name="fact-1-dont-prematurely-optimize"></a>事实 1：切勿过早优化  

 编写比实际需要更为复杂的代码，将产生维护、调试和改进成本。 有经验的程序员对如何解决编码问题并编写出更高效的代码有一个直观的把握。 然而，有时他们过早地优化了代码。 例如，当一个简单的数组就足够的时候，他们却使用哈希表或使用可能泄漏内存的复杂缓存，而不是简单地重新计算值。 即使你是一个有经验的程序员，当你发现问题时，应该测试性能并分析你的代码。
  
### <a name="fact-2-if-youre-not-measuring-youre-guessing"></a>事实 2：若不测量，便只是猜测  

 配置文件和度量不会撒谎。 配置文件向你显示 CPU 是否已满或者你是否在磁盘 I/O 上受阻。 配置文件可告知正在分配的内存类型和大小，以及 CPU 是否在[垃圾回收](../../standard/garbage-collection/index.md) (GC) 中花费了大量的时间。
  
 你应该为应用中的关键客户体验或方案设定性能目标，并编写测试来测量性能。 应用科学的方法调查失败的测试：使用配置文件来指导你、假设有可能是什么问题，并用利用试验或代码更改来测试你的假设。 使用定期测试建立一段时间内的基线性能测量，以便你可以隔离导致性能衰退的更改。 通过以严格的方式处理性能工作，你可以避免将时间浪费在不需要的代码更新上。
  
### <a name="fact-3-good-tools-make-all-the-difference"></a>事实 3：好的工具将使一切大不相同  

 好的工具可以让你快速深入地了解最大的性能问题（CPU、内存或磁盘）并帮助你找到导致那些瓶颈的代码。 Microsoft 提供多种性能工具，如 [Visual Studio Profiler](/visualstudio/profiling/beginners-guide-to-performance-profiling) 和 [PerfView](https://www.microsoft.com/download/details.aspx?id=28567)。
  
 PerfView 是一个免费且功能极为强大的工具，它可以帮助你专注于深层问题，如磁盘 I/O、GC 事件和内存。 可以捕获与性能相关的 [Windows 事件跟踪](../wcf/samples/etw-tracing.md) (ETW) 事件，并很轻松地查看每个应用、每个进程、每个堆栈和每个线程信息。 PerfView 向你显示应用分配了多少内存以及分配了何种内存，并显示哪些函数或调用堆栈提供了内存分配以及他们提供了多少。 有关详细信息，请参见丰富的帮助主题、演示以及工具随附的视频（如第 9 频道上的 [PerfView 教程](https://channel9.msdn.com/Series/PerfView-Tutorial)）。
  
### <a name="fact-4-its-all-about-allocations"></a>事实 4：一切皆与分配有关  

 你可能会认为构建一个响应性 .NET Framework 应用只与算法（如使用快速排序，而不是气泡排序）相关，但事实并非如此。 构建一个响应性应用的最关键因素是分配内存，尤其是当你的应用非常大或需要处理大量数据的时候。
  
 几乎所有使用新编译器 API 来构建响应性 IDE 体验的工作，均涉及到避免分配和管理缓存策略。 PerfView 跟踪显示新的 C# 和 Visual Basic 编译器的性能很少受 CPU 约束。 在读取数十万行或数百万行的代码、读取元数据或发出生成的代码时，编译器可能会受 I/O 约束。 所有 UI 线程延迟几乎都是由垃圾回收造成的。 .NET Framework GC 的性能经过高度优化，能够在执行应用代码的同时，并行完成其大部分工作。 但是，单个分配可能会触发昂贵的 [gen2](../../standard/garbage-collection/fundamentals.md) 回收，从而停止所有线程。
  
## <a name="common-allocations-and-examples"></a>常见的分配和示例  

 本部分中的示例表达式具有看起来较小的隐藏分配。 但是，如果一个大型应用执行表达式足够多次数，表达式可以产生数百个 MB，甚至 GB 的分配。 例如，一分钟的测试以模拟开发人员在编辑器中键入分配的内存（以 GB 为单位），并带领性能团队专注于键入方案。
  
### <a name="boxing"></a>装箱  

 当通常存在于堆栈或数据结构中的值类型包装在一个对象中时，[装箱](../../csharp/programming-guide/types/boxing-and-unboxing.md)便会发生。 即，你分配一个对象以保存数据，然后将一个指针返回到该对象。 .NET Framework 有时由于方法的签名或存储位置的类型而装箱值。 在一个对象中包装值类型导致内存分配。 许多装箱操作可以向你的应用提供分配（以 MB 或 GB 为单位），这意味着你的应用将产生更多的 GC。 .NET Framework 和语言编译器应尽可能地避免装箱，但有时它会在最不经意的时候发生。
  
 若要查看 PerfView 中的装箱，打开跟踪，并在你的应用的进程名称（请记住，PerfView 报告所有进程）下查看 GC Heap Alloc Stack。 如果你在分配下看到类似于 <xref:System.Int32?displayProperty=nameWithType> 和 <xref:System.Char?displayProperty=nameWithType> 的类型，即表示你正在装箱值类型。 选择这些类型中一个，将显示它们在其中装箱的堆栈和函数。
  
 **示例 1：字符串方法和值类型参数**  
  
 此示例代码演示了潜在不必要的和过度的装箱：  
  
```csharp  
public class Logger  
{  
    public static void WriteLine(string s) { /*...*/ }  
}  
  
public class BoxingExample  
{  
    public void Log(int id, int size)  
    {  
        var s = string.Format("{0}:{1}", id, size);  
        Logger.WriteLine(s);  
    }  
}  
```  
  
 此代码提供登录功能，因此应用可以经常（可能是几百万次）调用 `Log` 函数。 问题在于对 `string.Format` 的调用解析为 <xref:System.String.Format%28System.String%2CSystem.Object%2CSystem.Object%29> 重载。
  
 此重载需要 .NET Framework 来将 `int` 值装箱到对象中，并将这些值传递到此方法调用。 部分修复是指调用 `id.ToString()` 和 `size.ToString()` 并且将所有字符串（这些字符串是对象）传递到 `string.Format` 调用。 调用 `ToString()` 将分配一个字符串，但该分配无论如何都将在 `string.Format` 内部发生。
  
 你可能会认为对 `string.Format` 的这种基本调用只是字符串串联，因此你可能会改为编写此代码：  
  
```csharp  
var s = id.ToString() + ':' + size.ToString();  
```  
  
 然而，该行代码因编译为 <xref:System.String.Concat%28System.Object%2CSystem.Object%2CSystem.Object%29> 而引入了装箱分配。 .NET Framework 必须装箱字符文本以调用 `Concat`  
  
 **示例 1 的修复**  
  
 完整的修复很简单。 只需将字符文本替换为字符串文本即可，由于字符串已经是对象了，因此这样不会导致装箱：  
  
```csharp  
var s = id.ToString() + ":" + size.ToString();  
```  
  
 **示例 2：枚举装箱**  
  
 由于常使用枚举类型，尤其是在字典查找操作中，此示例是导致新的 C# 和 Visual Basic 编译器中出现大量分配的原因。
  
```csharp  
public enum Color  
{  
    Red, Green, Blue  
}  
  
public class BoxingExample  
{  
    private string name;  
    private Color color;  
    public override int GetHashCode()  
    {  
        return name.GetHashCode() ^ color.GetHashCode();  
    }  
}  
```  
  
 此问题很微妙。 PerfView 会将此报告为 <xref:System.Enum.GetHashCode> 装箱，因为该方法出于实现的原因对枚举类型的根本表现形式进行装箱。 如果在 PerfView 中仔细查看，你可能会看到对 <xref:System.Enum.GetHashCode> 的每个调用都存在两个装箱分配。 编译器插入一个，而 .NET Framework 插入另一个。
  
 **示例 2 的修复**  
  
 在调用 <xref:System.Enum.GetHashCode> 前，你可以通过强制转换为基础表现形式，轻松避免这两个分配：  
  
```csharp  
((int)color).GetHashCode()  
```  
  
 枚举类型上另一种常见的装箱源是 <xref:System.Enum.HasFlag%28System.Enum%29?displayProperty=nameWithType> 方法。 传递到 <xref:System.Enum.HasFlag%28System.Enum%29> 的自变量必须进行装箱。 大多数情况下，将对 <xref:System.Enum.HasFlag%28System.Enum%29?displayProperty=nameWithType> 的调用替换为按位测试更简单并且无需分配。
  
 请记住第一个性能事实（即，切勿过早优化），不要以这种方式开始重写你所有的代码。 请留意这些装箱成本，但仅在分析完应用并找到热点后才更改你的代码。
  
### <a name="strings"></a>字符串  

 字符串操作是产生分配的最主要原因之一，并且它们经常出现在 PerfView 前五大分配之中。 程序将字符串用于序列化、JSON 和 REST API。 当你无法使用枚举类型时，可以将字符串用作编程常量与系统进行互操作。 当你的分析显示字符串极为影响性能时，则查找对 <xref:System.String> 方法的调用，如 <xref:System.String.Format%2A>、<xref:System.String.Concat%2A>、<xref:System.String.Split%2A>、<xref:System.String.Join%2A> 和 <xref:System.String.Substring%2A> 等。 使用 <xref:System.Text.StringBuilder> 来避免创建从多个片段创建字符串的成本能够到帮助作用，但即使是分配 <xref:System.Text.StringBuilder> 对象也可能会变成需要你管理的瓶颈。
  
 **示例 3：字符串操作**  
  
 C# 编译器具有此编写格式化 XML 文档注释的文本的代码：  
  
```csharp  
public void WriteFormattedDocComment(string text)  
{  
    string[] lines = text.Split(new[] { "\r\n", "\r", "\n" },  
                                StringSplitOptions.None);  
    int numLines = lines.Length;  
    bool skipSpace = true;  
    if (lines[0].TrimStart().StartsWith("///"))  
    {  
        for (int i = 0; i < numLines; i++)  
        {  
            string trimmed = lines[i].TrimStart();  
            if (trimmed.Length < 4 || !char.IsWhiteSpace(trimmed[3]))  
            {  
                skipSpace = false;  
                break;  
            }  
        }  
        int substringStart = skipSpace ? 4 : 3;  
        for (int i = 0; i < numLines; i++)  
            WriteLine(lines[i].TrimStart().Substring(substringStart));  
    }  
    else { /* ... */ }  
```  
  
 你可以看到此代码执行了很多字符串操作。 该代码使用库方法将行拆分为单独的字符串，以修整空格、检查自变量 `text` 是否为 XML 文档注释，并且从行提取子字符串。
  
 在 `WriteFormattedDocComment` 中的第一行上，`text.Split` 调用将分配一个新的三元素数组作为每次调用的自变量。 每次编译器都必须发出代码以分配此数组。 那是因为编译器不知道 <xref:System.String.Split%2A> 是否将数组存储在了一个该数组可能已由其他代码修改的位置，这样将影响后续对 `WriteFormattedDocComment` 的调用。 对 <xref:System.String.Split%2A> 的调用还会为 `text` 中的每一行分配一个字符串，并分配其他内存以执行操作。
  
 `WriteFormattedDocComment` 具有对 <xref:System.String.TrimStart%2A> 方法的三个调用。 其中两个调用在复制工作和分配的内循环中。 更糟的是，在没有实参的情况下调用 <xref:System.String.TrimStart%2A> 方法，除了分配字符串结果外，还将分配一个空数组（用于 `params` 形参）。
  
 最后，存在一个对 <xref:System.String.Substring%2A> 方法的调用，这通常会分配一个新字符串。
  
 **示例 3 的修复**  
  
 不同于以前的示例，略微编辑无法修复这些分配。 你需要返回一步、查看该问题，然后采用不同的方法处理它。 例如，你将注意到 `WriteFormattedDocComment()` 的参数是一个包含该方法所需全部信息的字符串，因此该代码可以执行更多索引而不是分配很多部分字符串。
  
 编译器的性能团队使用如下代码处理所有这些分配：  
  
```csharp  
private int IndexOfFirstNonWhiteSpaceChar(string text, int start) {  
    while (start < text.Length && char.IsWhiteSpace(text[start])) start++;  
    return start;  
}  
  
private bool TrimmedStringStartsWith(string text, int start, string prefix) {  
    start = IndexOfFirstNonWhiteSpaceChar(text, start);  
    int len = text.Length - start;  
    if (len < prefix.Length) return false;  
    for (int i = 0; i < len; i++)  
    {  
        if (prefix[i] != text[start + i]) return false;  
    }  
    return true;  
}  
  
// etc...
```  
  
 `WriteFormattedDocComment()` 的第一个版本分配了一个数组、多个子字符串、一个修整的子字符串和一个空 `params` 数组。 它还检查 "///"。 修改后的代码仅使用索引且不执行分配。 它查找第一个非空白字符，然后按字符检查字符，以查看字符串是否以 "///" 开头。 新代码使用 `IndexOfFirstNonWhiteSpaceChar` 而不是 <xref:System.String.TrimStart%2A> 来返回指定开始索引后的第一个索引 () 出现非空白字符。 修复并不完整，但你可以看到如何为完整解决方案应用类似的修复。 通过在整个代码中应用此方法，你可以删除 `WriteFormattedDocComment()` 中的所有分配。
  
 **示例 4：StringBuilder**  
  
 此示例使用 <xref:System.Text.StringBuilder> 对象。 以下函数生成泛型类型的完整类型名称：  
  
```csharp  
public class Example  
{  
    // Constructs a name like "SomeType<T1, T2, T3>"  
    public string GenerateFullTypeName(string name, int arity)  
    {  
        StringBuilder sb = new StringBuilder();  
  
        sb.Append(name);  
        if (arity != 0)  
        {  
            sb.Append("<");  
            for (int i = 1; i < arity; i++)  
            {  
                sb.Append("T"); sb.Append(i.ToString()); sb.Append(", ");  
            }  
            sb.Append("T"); sb.Append(i.ToString()); sb.Append(">");  
        }  
  
        return sb.ToString();  
    }  
}  
```  
  
 关注的焦点在创建新 <xref:System.Text.StringBuilder> 实例的行上。 该代码导致针对 `sb.ToString()` 的分配和 <xref:System.Text.StringBuilder> 实现中的内部分配，但如果你想要字符串结果，则无法控制那些分配。
  
 **示例 4 的修复**  
  
 若要修复 `StringBuilder` 对象分配，则缓存该对象。 即使是缓存可能丢弃的单个实例，也可以显著提高性能。 这是该函数的新实现，省去所有的代码，除了新的第一行和最后一行：  
  
```csharp  
// Constructs a name like "MyType<T1, T2, T3>"  
public string GenerateFullTypeName(string name, int arity)  
{  
    StringBuilder sb = AcquireBuilder();  
    /* Use sb as before */  
    return GetStringAndReleaseBuilder(sb);  
}  
```  
  
 重要部分是新 `AcquireBuilder()` 和 `GetStringAndReleaseBuilder()` 函数：  
  
```csharp  
[ThreadStatic]  
private static StringBuilder cachedStringBuilder;  
  
private static StringBuilder AcquireBuilder()  
{  
    StringBuilder result = cachedStringBuilder;  
    if (result == null)  
    {  
        return new StringBuilder();  
    }  
    result.Clear();  
    cachedStringBuilder = null;  
    return result;  
}  
  
private static string GetStringAndReleaseBuilder(StringBuilder sb)  
{  
    string result = sb.ToString();  
    cachedStringBuilder = sb;  
    return result;  
}  
```  
  
 由于新编译器使用线程处理，这些实现使用线程静态字段（<xref:System.ThreadStaticAttribute> 特性）来缓存 <xref:System.Text.StringBuilder>，并且你有可能可以放弃 `ThreadStatic` 声明。 线程静态字段为执行此代码的每个线程保存一个唯一值。
  
 在清除该值并设置字段或缓存为 null 后，`AcquireBuilder()` 将返回缓存的 <xref:System.Text.StringBuilder> 实例（如果存在）。 否则，`AcquireBuilder()` 将创建一个新的实例并返回它，同时保留该字段或将缓存设置为 null。
  
 当你已完成 <xref:System.Text.StringBuilder> 时，调用 `GetStringAndReleaseBuilder()` 以获取字符串结果、将 <xref:System.Text.StringBuilder> 实例保存在字段或缓存中，然后返回该结果。 执行可以重新进入此代码并创建多个 <xref:System.Text.StringBuilder> 对象（尽管这种情况很少发生）。 该代码仅保存最后释放的 <xref:System.Text.StringBuilder> 实例以供稍后使用。 这个简单的缓存策略可在新的编译器中显著减少分配。 部分 .NET Framework 和 MSBuild ("MSBuild") 使用类似的技术以提高性能。
  
 这个简单的缓存策略符合良好的缓存设计要求，因为它具有大小上限。 然而，现在存在比原来更多的代码，这意味着更多的维护成本。 仅当你发现了性能问题时，并且 PerfView 已显示 <xref:System.Text.StringBuilder> 分配是一个重要的参与者，才应采用该缓存策略。
  
### <a name="linq-and-lambdas"></a>LINQ 和 lambda  

Language-Integrated 查询 (LINQ) 与 lambda 表达式结合使用，就是一个工作效率功能的示例。 但是，它的使用可能会对性能产生很大的影响，并且可能会发现需要重写代码。
  
 **示例5： Lambda、List \<T> 和 IEnumerable\<T>**  
  
 此示例使用 [LINQ 和功能性代码](/archive/blogs/charlie/anders-hejlsberg-on-linq-and-functional-programming)在编译器模型中查找符号，给定的名称字符串为：  
  
```csharp  
class Symbol {  
    public string Name { get; private set; }  
    /*...*/  
}  
  
class Compiler {  
    private List<Symbol> symbols;  
    public Symbol FindMatchingSymbol(string name)  
    {  
        return symbols.FirstOrDefault(s => s.Name == name);  
    }  
}  
```  
  
 该新编译器和构建于其上的 IDE 体验极为频繁地调用 `FindMatchingSymbol()`，并且此函数的单行代码上有多个隐藏的分配。 若要检查那些分配，首先将该函数的单行代码拆分成两行：  
  
```csharp  
Func<Symbol, bool> predicate = s => s.Name == name;  
     return symbols.FirstOrDefault(predicate);  
```  
  
 第一行中，[Lambda 表达式](../../csharp/language-reference/operators/lambda-expressions.md) `s => s.Name == name` [封盖](/archive/blogs/ericlippert/what-are-closures)本地变量 `name`。 这意味着除了针对 `predicate` 所保存的 [委托](../../csharp/language-reference/builtin-types/reference-types.md#the-delegate-type)分配对象以外，该代码分配了静态类以保存捕获 `name` 的值的环境。 编译器生成的代码如下所示：  
  
```csharp  
// Compiler-generated class to hold environment state for lambda  
private class Lambda1Environment  
{  
    public string capturedName;  
    public bool Evaluate(Symbol s)  
    {  
        return s.Name == this.capturedName;  
    }  
}  
  
// Expanded Func<Symbol, bool> predicate = s => s.Name == name;  
Lambda1Environment l = new Lambda1Environment() { capturedName = name };  
var predicate = new Func<Symbol, bool>(l.Evaluate);  
```  
  
 这两个 `new` 分配（一个用于环境类，另一个用于委托）现在是显式的。
  
 现在查看对 `FirstOrDefault` 的调用。 <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> 类型上的此扩展方法也会导致分配。 因为 `FirstOrDefault` 采用 <xref:System.Collections.Generic.IEnumerable%601> 对象作为它的第一个自变量，你可以将调用展开到以下代码（略微简化以便讨论）：  
  
```csharp  
// Expanded return symbols.FirstOrDefault(predicate) ...
     IEnumerable<Symbol> enumerable = symbols;  
     IEnumerator<Symbol> enumerator = enumerable.GetEnumerator();  
     while(enumerator.MoveNext())  
     {  
         if (predicate(enumerator.Current))  
             return enumerator.Current;  
     }  
     return default(Symbol);  
```  
  
 `symbols` 变量具有类型 <xref:System.Collections.Generic.List%601>。 <xref:System.Collections.Generic.List%601> 集合类型实现 <xref:System.Collections.Generic.IEnumerable%601> 并且巧妙地定义了一个 <xref:System.Collections.Generic.IEnumerator%601> 使用 <xref:System.Collections.Generic.List%601> 实现的枚举器（`struct` 接口）。 使用结构而不是类意味着你通常避免任何堆分配，而后者可能会影响垃圾回收性能。 枚举器通常与语言的 `foreach` 循环结合使用，该循环使用枚举器结构，因为它是在调用堆栈上返回的。 递增调用堆栈指针从而为对象腾出空间，不会像堆分配那样影响 GC。
  
 对于展开的 `FirstOrDefault` 调用，代码需要在一个 `GetEnumerator()` 上调用 <xref:System.Collections.Generic.IEnumerable%601>。 将 `symbols` 分配到类型 `enumerable` 的 `IEnumerable<Symbol>` 变量失去了实际对象是一个 <xref:System.Collections.Generic.List%601> 的信息。 这意味着当代码使用 `enumerable.GetEnumerator()` 提取枚举器时，.NET Framework 必须装箱返回的结构以将其分配到 `enumerator` 变量。
  
 **示例 5 的修复**  
  
 修复方法是以下列方式重写 `FindMatchingSymbol`，将它的单行代码替换为仍旧简洁、容易阅读和理解且易于维护的六行代码：  
  
```csharp  
public Symbol FindMatchingSymbol(string name)  
    {  
        foreach (Symbol s in symbols)  
        {  
            if (s.Name == name)  
                return s;  
        }  
        return null;  
    }  
```  
  
 此代码不使用 LINQ 扩展方法、lambda 或枚举器，并且它不会导致分配。 不存在分配，因为编译器可以理解 `symbols` 集合是一个 <xref:System.Collections.Generic.List%601> 并且可以使用正确的类型将结果枚举器（一个结构）绑定到本地变量，从而避免装箱。 此函数的原始版本是展示 C# 表现力以及 .NET Framework 工作效率的出色示例。 这种新的并且更有效的版本保留了那些品质，且未添加任何用于维护的复杂代码。
  
### <a name="async-method-caching"></a>异步方法缓存  

下一个示例显示当尝试在[异步](../../csharp/programming-guide/concepts/async/index.md)方法中使用缓存结果时，将遇到的一个常见问题。
  
 **示例 6：在异步方法中缓存**  
  
 在新的 C# 和 Visual Basic 编译器上构建的 Visual Studio IDE 功能频繁地获取语法树，并且编译器在执行此操作时使用异步来保持 Visual Studio 的响应性。 以下是你可以编写的、用于获取语法树的该代码的第一个版本：  
  
```csharp  
class SyntaxTree { /*...*/ }  
  
class Parser { /*...*/  
    public SyntaxTree Syntax { get; }  
    public Task ParseSourceCode() { /*...*/ }  
}  
  
class Compilation { /*...*/  
    public async Task<SyntaxTree> GetSyntaxTreeAsync()  
    {  
        var parser = new Parser(); // allocation  
        await parser.ParseSourceCode(); // expensive  
        return parser.Syntax;  
    }  
}  
```  
  
 你可以看到调用 `GetSyntaxTreeAsync()` 实例化了一个 `Parser`，分析了该代码，然后返回一个 <xref:System.Threading.Tasks.Task> 对象 `Task<SyntaxTree>`。 成本高的部分是分配 `Parser` 实例和分析代码。 该函数返回了一个 <xref:System.Threading.Tasks.Task> 以便调用方可以等待分析工作并释放 UI 线程以响应用户输入。
  
 多个 Visual Studio 功能可能会尝试获取相同的语法树，因此你可以编写以下代码以缓存分析结果，从而节省时间和分配。 但是，此代码导致了一个分配：  
  
```csharp  
class Compilation { /*...*/  
  
    private SyntaxTree cachedResult;  
  
    public async Task<SyntaxTree> GetSyntaxTreeAsync()  
    {  
        if (this.cachedResult == null)  
        {  
            var parser = new Parser(); // allocation  
            await parser.ParseSourceCode(); // expensive  
            this.cachedResult = parser.Syntax;  
        }  
        return this.cachedResult;  
    }  
}  
```  
  
 你看到使用缓存的新代码具有一个命名为 `SyntaxTree` 的 `cachedResult` 字段。 当此字段是 null 时，`GetSyntaxTreeAsync()` 奏效并且将结果保存在缓存中。 `GetSyntaxTreeAsync()` 返回 `SyntaxTree` 对象。 问题在于当你具有一个类型为 `async` 的 `Task<SyntaxTree>` 函数时，并且你返回类型为 `SyntaxTree` 的值，编译器发出代码来分配保存结果的任务（通过使用 `Task<SyntaxTree>.FromResult()`）。 任务标记为已完成，并且结果立即可用。 在新编译器的代码中，已经完成的 <xref:System.Threading.Tasks.Task> 对象频繁地发生，以至于修复这些分配显著地提高了响应能力。
  
 **示例 6 的修复**  
  
 若要删除已完成的 <xref:System.Threading.Tasks.Task> 分配，可以缓存任务对象和已完成的结果：  
  
```csharp  
class Compilation { /*...*/  
  
    private Task<SyntaxTree> cachedResult;  
  
    public Task<SyntaxTree> GetSyntaxTreeAsync()  
    {  
        return this.cachedResult ??
               (this.cachedResult = GetSyntaxTreeUncachedAsync());  
    }  
  
    private async Task<SyntaxTree> GetSyntaxTreeUncachedAsync()  
    {  
        var parser = new Parser(); // allocation  
        await parser.ParseSourceCode(); // expensive  
        return parser.Syntax;  
    }  
}  
```  
  
 此代码将 `cachedResult` 的类型更改为 `Task<SyntaxTree>` 并且使用一个 `async` 保存来自 `GetSyntaxTreeAsync()` 原始代码的帮助程序函数。 如果 `GetSyntaxTreeAsync()` 不为 null，它现在使用 [null 合并运算符](../../csharp/language-reference/operators/null-coalescing-operator.md)来返回 `cachedResult`。 如果 `cachedResult` 为 null，则 `GetSyntaxTreeAsync()` 调用 `GetSyntaxTreeUncachedAsync()` 并且缓存结果。 请注意，`GetSyntaxTreeAsync()` 不会像代码通常所做的那样等待对 `GetSyntaxTreeUncachedAsync()` 的调用。 不使用 await 意味着当 `GetSyntaxTreeUncachedAsync()` 返回它的 <xref:System.Threading.Tasks.Task> 对象时，`GetSyntaxTreeAsync()` 立即返回 <xref:System.Threading.Tasks.Task>。 现在，缓存的结果是 <xref:System.Threading.Tasks.Task>，因此不存在用于返回缓存结果的分配。
  
### <a name="additional-considerations"></a>其他注意事项  

 以下是有关大型应用或处理大量数据的应用中潜在问题的几个要点。
  
 **字典**  
  
 字典在许多程序中普遍使用，此外字典非常方便且在本质上是高效的。 但是，它们经常使用不当。 在 Visual Studio 和新编译器中，分析表明许多字典包含单个元素或为空。 一个空 <xref:System.Collections.Generic.Dictionary%602> 具有十个字段并且在 x86 计算机的堆上占用 48 个字节。 当你需要一个使用常量时间查找的映射或关联数据结构时，字典是很有效的。 然而，当你只有几个元素时，使用字典便浪费了大量的空间。 相反，你可以采用迭代方式查找一个 `List<KeyValuePair\<K,V>>`，速度一样快。 如果你使用字典只是为了用数据加载它，然后从中读取（一个非常常用的模式），则使用附带 N(log(N)) 查找的排序数组可能差不多快，具体取决于你所使用的元素的数目。
  
 **类与结构**  
  
 在某种程度上，类和结构为优化你的应用提供了一个经典的空间/时间权衡。 即使类不具有字段，它们也会在 x86 计算机上产生 12 个字节的开销，但是你可以通过便宜的方法绕过它们，因为只需要一个指针来引用类的实例。 如果不对结构进行装箱，它们不会产生任何堆分配，但是当你将大型结构作为函数自变量或返回值传递时，CPU 需要一定时间来以原子方式复制结构的所有数据成员。 注意对返回结构的属性的重复调用，并且将属性的值缓存在一个局部变量中，以避免过度的数据复制。
  
 **缓存**  
  
 一个常用的性能技巧是缓存结果。 然而，没有大小上限或处置策略的缓存可能是一个内存泄漏。 当处理大量数据时，如果你在缓存中保存大量内存，可能会导致垃圾回收覆盖缓存查找带来的好处。
  
 在本文中，我们讨论了你应该如何注意可能会影响你的应用响应能力的性能瓶颈征兆，对于大型系统或处理大量数据的系统尤为如此。 常见的原因包括装箱、字符串操作、LINQ 和 lambda、异步方法中的缓存、没有大小限制或处置策略的缓存、不恰当的字典使用以及来回传递结构。 请记住优化应用的四个事实：  
  
- 切勿过早地优化 – 保持高效并在你发现问题时优化应用。
  
- 配置文件不会撒谎 – 若不测量，便只是猜测。
  
- 好的工具将使一切大不相同 – 下载 PerfView 并且试用。
  
- 一切皆与分配有关 – 这就是编译器平台团队花大部分时间改进新编译器性能的原因所在。
  
## <a name="see-also"></a>另请参阅

- [此主题的演示视频](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B333)
- [性能分析初学者指南](/visualstudio/profiling/beginners-guide-to-performance-profiling)
- [“性能”](index.md)
- [.NET 性能提示](/previous-versions/dotnet/articles/ms973839(v=msdn.10))
- [第 9 频道 PerfView 教程](https://channel9.msdn.com/Series/PerfView-Tutorial)
- [.NET Compiler Platform SDK](../../csharp/roslyn-sdk/index.md)
- [GitHub 上的 dotnet/roslyn 存储库](https://github.com/dotnet/roslyn)
