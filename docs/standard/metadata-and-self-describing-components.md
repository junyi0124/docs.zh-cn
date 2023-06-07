---
title: 元数据和自描述组件
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- runtime, metadata
- languages, interoperability
- portable executable files, metadata
- self-describing files
- metadata, about metadata
- common language runtime, metadata
- PE files, metadata
- components [.NET], metadata
ms.assetid: 3dd13c5d-a508-455b-8dce-0a852882a5a7
ms.openlocfilehash: c597c1ae6153050f08ed2976f7ee6102cdd3c797
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94822342"
---
# <a name="metadata-and-self-describing-components"></a>元数据和自描述组件

在过去，以一种语言编写的软件组件（.exe 或 .dll）不能方便地使用以另一种语言编写的软件组件。 在这个问题的解决上，COM 向前迈进了一步。 .NET 允许编译器向所有的模块和程序集发出附加的说明性信息，从而使组件互用更加简单。 这种叫做“元数据”的信息有助于组件无缝交互。

 元数据是一种二进制信息，用以对存储在公共语言运行时可迁移可执行文件 (PE) 文件或存储在内存中的程序进行描述。 将您的代码编译为 PE 文件时，便会将元数据插入到该文件的一部分中，而将代码转换为 Microsoft 中间语言 (MSIL) 并将其插入到该文件的另一部分中。 在模块或程序集中定义和引用的每个类型和成员都将在元数据中进行说明。 当执行代码时，运行时将元数据加载到内存中，并引用它来发现有关代码的类、成员、继承等信息。

 元数据以非特定语言的方式描述在代码中定义的每一类型和成员。 元数据存储以下信息：

- 程序集的说明。

  - 标识（名称、版本、区域性、公钥）。

  - 导出的类型。

  - 该程序集所依赖的其他程序集。

  - 运行所需的安全权限。

- 类型的说明。

  - 名称、可见性、基类和实现的接口。

  - 成员（方法、字段、属性、事件、嵌套的类型）。

- 特性。

  - 修饰类型和成员的其他说明性元素。

## <a name="benefits-of-metadata"></a>元数据的优点

对于一种更简单的编程模型来说，元数据是关键，该模型不再需要接口定义语言 (IDL) 文件、头文件或任何外部组件引用方法。 元数据使 .NET 语言能够自动以非特定语言的方式对其自身进行描述，而这是开发人员和用户都无法看见的。 另外，通过使用特性，可以对元数据进行扩展。 元数据具有以下主要优点：

- 自描述文件。

  公共语言运行时模块和程序集是自描述的。 模块的元数据包含与另一个模块进行交互所需的全部信息。 元数据自动提供 COM 中 IDL 的功能，因此可以将一个文件同时用于定义和实现。 运行时模块和程序集甚至不需要向操作系统注册。 结果，运行时使用的说明始终反映编译文件中的实际代码，从而提高应用程序的可靠性。

- 语言互用性和更简单的基于组件的设计。

  元数据提供所有必需的有关已编译代码的信息，以供你从用不同语言编写的 PE 文件中继承类。 你可以创建用任何托管语言（任何面向公共语言运行时的语言）编写的任何类的实例，而不用担心显式封送处理或使用自定义的互用代码。

- 特性。

  .NET 允许在编译文件中声明特定种类的元数据（称为特性）。 特性在 .NET 中处处可见，用于更精确地控制程序在运行时的行为。 另外，还可以通过用户定义的自定义特性，向 .NET 文件发出自己的自定义元数据。 有关更多信息，请参阅[特性](attributes/index.md)。

## <a name="metadata-and-the-pe-file-structure"></a>元数据和 PE 文件结构

元数据存储在 .NET 可迁移可执行文件 (PE) 文件的一个部分中，而 Microsoft 中间语言 (MSIL) 则存储在 PE 文件的另一部分中。 文件的元数据部分包含一系列的表和堆数据结构。 MSIL 部分包含 MSIL 和引用 PE 文件元数据部分的元数据标记。 例如，使用 [MSIL 反汇编程序 (Ildasm.exe)](../framework/tools/ildasm-exe-il-disassembler.md) 等工具查看代码的 MSIL 时，可能会遇到元数据标记。

### <a name="metadata-tables-and-heaps"></a>元数据表和堆

每个元数据表都保留有关程序元素的信息。 例如，一个元数据表说明代码中的类，另一个元数据表说明字段等。 如果你的代码中有 10 个类，类表将有 10 行，每行一类。 元数据表引用其他的表和堆。 例如，类的元数据表引用方法表。

元数据还以四种堆结构存储信息：字符串、Blob、用户字符串和 GUID。 所有用于对类型和成员进行命名的字符串都存储在字符串堆中。 例如，方法表不直接存储特定方法的名称，而是指向存储在字符串堆中的方法的名称。

### <a name="metadata-tokens"></a>元数据标记

元数据标记在 PE 文件的 MSIL 部分中唯一确定每个元数据表的每一行。 元数据标记在概念上和指针相似，永久驻留在 MSIL 中，引用特定的元数据表。

元数据标记是一个四个字节的数字。 最高位字节表示特定标记（方法、类型等）引用的元数据表。 剩下的三个字节指定与所说明的编程元素对应的元数据表中的行。 如果您用 C# 定义一个方法并将其编译到 PE 文件，下面的元数据标记可能存在于 PE 文件的 MSIL 部分：

`0x06000004`

最高位字节 (`0x06`) 表示这是一个 MethodDef  标记。 低位的三个字节 (`000004`) 指示公共语言运行时在 MethodDef  表的第四行查找对该方法定义进行描述的信息。

### <a name="metadata-within-a-pe-file"></a>PE 文件中的元数据

当为公共语言运行时编译程序时，该程序转换为由三部分组成的 PE 文件。 下表说明了每部分的内容。

|PE 部分|PE 部分的内容|
|----------------|----------------------------|
|PE 头|PE 文件主要部分的索引和入口点的地址。<br /><br /> 运行时使用该信息确定该文件为 PE 文件并确定当将程序加载到内存时执行从何处开始。|
|MSIL 指令|组成代码的 Microsoft 中间语言指令 (MSIL)。 许多 MSIL 指令带有元数据标记。|
|元数据|元数据表和堆。 运行时使用该部分记录您的代码中每个类型和成员的信息。 本部分还包括自定义特性和安全性信息。|

## <a name="run-time-use-of-metadata"></a>元数据在运行时的作用

要更好地理解元数据和它在公共语言运行时中的作用，构造一个简单的程序并说明元数据如何影响它的运行时情况可能很有帮助。 下面的代码示例显示名为 `MyApp` 的类中的两种方法。 `Main` 方法是程序入口点，而 `Add` 方法只返回两个整数参数的和。

```vb
Public Class MyApp
   Public Shared Sub Main()
      Dim ValueOne As Integer = 10
      Dim ValueTwo As Integer = 20
      Console.WriteLine("The Value is: {0}", Add(ValueOne, ValueTwo))
   End Sub

   Public Shared Function Add(One As Integer, Two As Integer) As Integer
      Return (One + Two)
   End Function
End Class
```

```csharp
using System;
public class MyApp
{
   public static int Main()
   {
      int ValueOne = 10;
      int ValueTwo = 20;
      Console.WriteLine("The Value is: {0}", Add(ValueOne, ValueTwo));
      return 0;
   }
   public static int Add(int One, int Two)
   {
      return (One + Two);
   }
}
```

当代码运行时，运行时将模块加载到内存并向元数据咨询该类的信息。 加载后，运行时对方法的 Microsoft 中间语言 (MSIL) 流执行广泛的分析，将其转换为快速本机指令。 运行时根据需要使用实时 (JIT) 编译器将 MSIL 指令转换为本机代码，每次转换一个方法。

下面的示例显示了从以前代码的 `Main` 功能生成的部分 MSIL。 可使用 [MSIL 反汇编程序 (Ildasm.exe)](../framework/tools/ildasm-exe-il-disassembler.md) 从任何 .NET 应用程序中查看 MSIL 和元数据。

```console
.entrypoint
.maxstack  3
.locals ([0] int32 ValueOne,
         [1] int32 ValueTwo,
         [2] int32 V_2,
         [3] int32 V_3)
IL_0000:  ldc.i4.s   10
IL_0002:  stloc.0
IL_0003:  ldc.i4.s   20
IL_0005:  stloc.1
IL_0006:  ldstr      "The Value is: {0}"
IL_000b:  ldloc.0
IL_000c:  ldloc.1
IL_000d:  call int32 ConsoleApplication.MyApp::Add(int32,int32) /* 06000003 */
```

JIT 编译器读取整个方法的 MSIL，对其进行彻底地分析，然后为该方法生成有效的本机指令。 在 `IL_000d` 遇到 `Add` 方法 (`/*` `06000003 */`) 的元数据标记，运行时使用该标记参考 MethodDef 表的第三行  。

下表显示了说明  **方法的元数据标记所引用的 MethodDef**`Add` 表的一部分。 虽然程序集中存在其他元数据表并具有它们自己唯一的值，但这里只讨论该表。

|行|相对虚拟地址 (RVA)|ImplFlags|Flags|“属性”<br /><br /> （指向字符串堆。）|Signature（指向 Blob 堆）|
|---------|--------------------------------------|---------------|-----------|-----------------------------------------|----------------------------------------|
|1|0x00002050|IL<br /><br /> Managed|Public<br /><br /> ReuseSlot<br /><br /> SpecialName<br /><br /> RTSpecialName<br /><br /> .ctor|.ctor（构造函数）||
|2|0x00002058|IL<br /><br /> Managed|Public<br /><br /> Static<br /><br /> ReuseSlot|Main|String|
|3|0x0000208c|IL<br /><br /> Managed|Public<br /><br /> Static<br /><br /> ReuseSlot|添加|int, int, int|

该表的每一列都包含有关代码的重要信息。 RVA  列允许运行时计算定义此方法的 MSIL 的起始内存地址。 ImplFlags  和 Flags  列包含说明该方法的位屏蔽（例如，该方法是公共的还是私有的）。 Name  列对来自字符串堆的方法的名称进行了索引。 Signature  列对在 Blob 堆中的方法签名的定义进行了索引。

运行时在第三行的 RVA  列计算所需的偏移量地址并将该地址返回到 JIT 编译器，然后，JIT 编译器进入新地址。 JIT 编译器继续在新地址处理 MSIL，直到它遇到另一个元数据标记，之后，重复该过程。

使用元数据，运行时可以访问加载代码并将其处理为本机指令所需的所有信息。 以这种方式，元数据使自描述文件、常规类型系统和跨语言继承成为可能。

## <a name="related-topics"></a>相关主题

|Title|描述|
|-----------|-----------------|
|[特性](attributes/index.md)|描述如何应用特性、编写自定义特性及检索存储在特性中的信息。|
