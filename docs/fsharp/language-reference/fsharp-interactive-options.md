---
title: 交互式选项
description: 了解 F# 交互窗口、fsi.exe 支持的命令行选项。
ms.date: 08/15/2020
ms.openlocfilehash: adc8dc86f14366720e1acbf35115d4e318a76aef
ms.sourcegitcommit: 9c45035b781caebc63ec8ecf912dc83fb6723b1f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88810525"
---
# <a name="f-interactive-options"></a>F# 交互窗口选项

本文介绍 F# 交互窗口支持的命令行选项 `fsi.exe` 。 F# 交互窗口接受许多与 F # 编译器相同的命令行选项，但也接受一些其他选项。

## <a name="use-f-interactive-for-scripting"></a>使用 F# 交互窗口进行脚本编写

F# 交互窗口， `dotnet fsi` 可以交互方式启动，也可以从命令行启动以运行脚本。 命令行语法为

```dotnetcli
dotnet fsi [options] [ script-file [arguments] ]
```

F # 脚本文件的文件扩展名为 `.fsx` 。

## <a name="table-of-f-interactive-options"></a>F# 交互窗口选项的表

下表总结了 F# 交互窗口支持的选项。 可以在命令行上或通过 Visual Studio IDE 设置这些选项。 若要在 Visual Studio IDE 中设置这些选项，请打开 " **工具** " 菜单，选择 " **选项**"，展开 " **F # 工具** " 节点，然后选择 " **F# 交互窗口**"。

其中列表显示在 F# 交互窗口选项参数中，列表元素由分号分隔 (`;`) 。

|选项|说明|
|------|-----------|
|**--**|用于指示 F# 交互窗口将剩余的参数作为命令行参数处理 F # 程序或脚本，您可以使用 **Fsi.exe my.application.commandlineargs**的代码访问这些参数。|
|**--checked**[ **+**&#124;**-** ]|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|
|**--代码页： &lt; int&gt;**|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|
|**--consolecolors**[ **+**&#124;**-** ]|输出警告和错误消息的颜色。|
|**--crossoptimize**[ **+**&#124;**-** ]|启用或禁用跨模块优化。|
|**--debug**[ **+**&#124;**-** ]<br /><br />**--debug：**[**full**&#124;**pdbonly**&#124;**可移植**&#124;**嵌入**]<br /><br />**-g**[ **+**&#124;**-** ]<br /><br />**-g：**[**full**&#124;**pdbonly**&#124;**可移植**&#124;**嵌入**]|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|
|**--define： &lt; string&gt;**|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|
|**--确定性**[ **+**&#124;**-** ]|生成一个确定性程序集 (包括模块版本 GUID 和时间戳) 。|
|**--exec**|指示 F # interactive 在加载文件或运行命令行上给定的脚本文件后退出。|
|**--fullpaths**|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|
|**--gui**[ **+**&#124;**-** ]|启用或禁用 Windows 窗体事件循环。 默认是启用的。|
|**--帮助**<br /><br />**-?**|用于显示命令行语法和每个选项的简短说明。|
|**--lib： &lt; 文件夹-列表&gt;**<br /><br />**-I： &lt; 文件夹列表&gt;**|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|
|**--load： &lt; filename&gt;**|在启动时编译给定的源代码，并将已编译的 F # 构造加载到会话中。|
|**--mlcompatibility**|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|
|**--noframework**|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)|
|**--nologo**|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|
|**--nowarn： &lt; warning-list&gt;**|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|
|**--optimize**[ **+**&#124;**-** ]|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|
|**--preferreduilang： &lt; lang&gt;**| 指定首选输出语言区域性名称 (例如，es，ja-jp) 。 |
|**--quiet**|抑制 F# 交互窗口输出到 **stdout** 流。|
|**--引用-调试**|指定应为从 F # 引号文本和反射定义派生的表达式发出额外的调试信息。 调试信息将添加到 F # 表达式树节点的自定义属性中。 请参阅 [代码引用](code-quotations.md) 和 [CustomAttributes](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-quotations-fsharpexpr.html#CustomAttributes)。|
|**--readline**[ **+**&#124;**-** ]|启用或禁用交互模式下的 tab 自动补全。|
|**--reference： &lt; filename&gt;**<br /><br />**-r： &lt; filename&gt;**|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|
|**--tailcalls**[ **+**&#124;**-** ]|启用或禁用 tail IL 指令，这将导致为尾递归函数重用堆栈帧。 默认情况下会启用此选项。|
|**--targetprofile： &lt; string&gt;**|指定此程序集的目标框架配置文件。 有效值为 `mscorlib`、`netcore` 或 `netstandard`。 默认为 `mscorlib`。|
|**--use： &lt; filename&gt;**|通知解释器在启动时使用给定文件作为初始输入。|
|**--utf8output**|与 fsc.exe 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|
|**--warning： &lt; 警告级别&gt;**|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|
|**--warnaserror**[ **+**&#124;**-** ]|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|
|**--warnaserror**[ **+**&#124;**-** ]：** &lt; int-list &gt; **|与 **fsc.exe** 编译器选项相同。 有关详细信息，请参阅 [编译器选项](compiler-options.md)。|

## <a name="f-interactive-structured-printing"></a>F# 交互窗口结构化打印

F# 交互窗口 (`dotnet fsi`) 使用 [结构化纯文本格式](plaintext-formatting.md) 的扩展版本来报告值。

1. `%A`支持纯文本格式的所有功能，一些功能还可自定义。

2. 如果输出控制台支持颜色，打印将着色。

3. 将限制放置在显示的字符串的长度，除非显式计算该字符串。

4. 可以通过对象使用一组可由用户定义的设置 `fsi` 。

用于自定义报告值的纯文本打印的可用设置包括：

```fsharp
open System.Globalization

fsi.FormatProvider <- CultureInfo("de-DE")  // control the default culture for primitives

fsi.PrintWidth <- 120        // Control the width used for structured printing

fsi.PrintDepth <- 10         // Control the maximum depth of nested printing

fsi.PrintLength <- 10        // Control the length of lists and arrays

fsi.PrintSize <- 100         // Control the maximum overall object count

fsi.ShowProperties <- false  // Control whether properties of .NET objects are shown by default

fsi.ShowIEnumerable <- false // Control whether sequence values are expanded by default

fsi.ShowDeclarationValues <- false // Control whether values are shown for declaration outputs
```

### <a name="customize-with-addprinter-and-addprinttransformer"></a>自定义 `AddPrinter` 和 `AddPrintTransformer`

可以使用和自定义打印 F# 交互窗口输出 `fsi.AddPrinter` `fsi.AddPrintTransformer` 。
第一个函数提供文本来替换对象打印。 第二个函数返回要改为显示的代理项对象。 例如，请考虑以下 F # 代码：

```fsharp
open System

fsi.AddPrinter<DateTime>(fun dt -> dt.ToString("s"))

type DateAndLabel =
    { Date: DateTime
      Label: string  }

let newYearsDay1999 =
    { Date = DateTime(1999, 1, 1)
      Label = "New Year" }
```

如果在 F# 交互窗口中执行该示例，则会根据格式设置选项集输出。 在这种情况下，它会影响日期和时间的格式：

```console
type DateAndLabel =
  { Date: DateTime
    Label: string }
val newYearsDay1999 : DateAndLabel = { Date = 1999-01-01T00:00:00
                                       Label = "New Year" }
```

`fsi.AddPrintTransformer` 可用于为打印提供代理项对象：

```fsharp
type MyList(values: int list) =
    member _.Values = values

fsi.AddPrintTransformer(fun (x:MyList) -> box x.Values)

let x = MyList([1..10])
```

此输出：

```console
val x : MyList = [1; 2; 3; 4; 5; 6; 7; 8; 9; 10]
```

如果传递给的转换器函数 `fsi.AddPrintTransformer` 返回 `null` ，则会忽略 print 转换器。
这可用于通过从类型开始筛选任何输入值 `obj` 。  例如：

```fsharp
fsi.AddPrintTransformer(fun (x:obj) ->
    match x with
    | :? string as s when s = "beep" -> box ["quack"; "quack"; "quack"]
    | _ -> null)

let y = "beep"
```

此输出：

```console
val y : string = ["quack"; "quack"; "quack"]
```

## <a name="related-topics"></a>相关主题

|Title|描述|
|-----|-----------|
|[编译器选项](compiler-options.md)|介绍可用于 F # 编译器的命令行选项 **fsc.exe**。|
