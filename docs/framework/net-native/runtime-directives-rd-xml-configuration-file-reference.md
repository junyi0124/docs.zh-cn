---
title: 运行时指令 (rd.xml) 配置文件引用
ms.date: 03/30/2017
ms.assetid: 8241523f-d8e1-4fb6-bf6a-b29bfe07b38a
ms.openlocfilehash: e74d34693446cca645003a9f93bc1777849e3182
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "76738406"
---
# <a name="runtime-directives-rdxml-configuration-file-reference"></a>运行时指令 (rd.xml) 配置文件引用

运行时指令 (.rd.xml) 文件是一个 XML 配置文件，它指定了指定程序元素是否适用于反射。 运行时指令文件的示例如下所示：

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
  <Application>
    <Namespace Name="Contoso.Cloud.AppServices" Serialize="Required Public" />
    <Namespace Name="ContosoClient.ViewModels" Serialize="Required Public" />
    <Namespace Name="ContosoClient.DataModel" Serialize="Required Public" />
    <Namespace Name="Contoso.Reader.UtilityLib" Serialize="Required Public" />

    <Namespace Name="System.Collections.ObjectModel" >
      <TypeInstantiation Name="ObservableCollection"
            Arguments="ContosoClient.DataModel.ProductItem" Serialize="Public" />
      <TypeInstantiation Name="ReadOnlyObservableCollection"
            Arguments="ContosoClient.DataModel.ProductGroup" Serialize="Public" />
    </Namespace>
  </Application>
</Directives>
```

## <a name="the-structure-of-a-runtime-directives-file"></a>运行时指令文件的结构

运行时指令文件使用 `http://schemas.microsoft.com/netfx/2013/01/metadata` 命名空间。

根元素为 [Directives](directives-element-net-native.md) 元素。 它可以包含零个或更多个 [Library](library-element-net-native.md) 元素，以及零个或一个 [Application](application-element-net-native.md) 元素，如以下结构中所示。 [Application](application-element-net-native.md) 元素的特性可定义应用程序范围的运行时反射策略，或可充当子元素的容器。 而 [Library](library-element-net-native.md) 元素则仅仅是一个容器。 [Application](application-element-net-native.md) 和 [Library](library-element-net-native.md) 元素的子级定义了适用于反射的类型、方法、字段、属性和事件。

有关引用信息，请从以下结构中选择元素或参阅[运行时指令元素](runtime-directive-elements.md)。 在以下层次结构中，省略号表示递归结构。 括号中的信息表明元素是可选项还是必需项，以及如果使用该元素，将允许多少个实例（一个或多个）。

- [Directives](directives-element-net-native.md) [1:1]
  - [Application](application-element-net-native.md) [0:1]
    - [Assembly](assembly-element-net-native.md) [0:M]
      - [命名空间](namespace-element-net-native.md)[0： M]。 . .
      - [类型](type-element-net-native.md)[0： M]。 . .
      - [类型实例化](typeinstantiation-element-net-native.md)（构造泛型类型） [0： M]。 . .
    - [Namespace](namespace-element-net-native.md) [0:M]
      - [命名空间](namespace-element-net-native.md)[0： M]。 . .
      - [类型](type-element-net-native.md)[0： M]。 . .
      - [类型实例化](typeinstantiation-element-net-native.md)（构造泛型类型） [0： M]。 . .
    - [Type](type-element-net-native.md) [0:M]
      - [Subtypes](subtypes-element-net-native.md)（包含类型的子类）[O:1]
      - [类型](type-element-net-native.md)[0： M]。 . .
      - [类型实例化](typeinstantiation-element-net-native.md)（构造泛型类型） [0： M]。 . .
      - [AttributeImplies](attributeimplies-element-net-native.md)（包含类型是一种特性）[O:1]
      - [GenericParameter](genericparameter-element-net-native.md) [0:M]
      - [Method](method-element-net-native.md) [0:M]
        - [Parameter](parameter-element-net-native.md) [0:M]
        - [TypeParameter](typeparameter-element-net-native.md) [0:M]
        - [GenericParameter](genericparameter-element-net-native.md) [0:M]
      - [MethodInstantiation](methodinstantiation-element-net-native.md)（构造泛型方法）[0:M]
      - [Property](property-element-net-native.md) [0:M]
      - [Field](field-element-net-native.md) [0:M]
      - [Event](event-element-net-native.md) [0:M]
    - [TypeInstantiation](typeinstantiation-element-net-native.md)（构造泛型类型）[0:M]
      - [类型](type-element-net-native.md)[0： M]。 . .
      - [类型实例化](typeinstantiation-element-net-native.md)（构造泛型类型） [0： M]。 . .
      - [Method](method-element-net-native.md) [0:M]
        - [Parameter](parameter-element-net-native.md) [0:M]
        - [TypeParameter](typeparameter-element-net-native.md) [0:M]
        - [GenericParameter](genericparameter-element-net-native.md) [0:M]
      - [MethodInstantiation](methodinstantiation-element-net-native.md)（构造泛型方法）[0:M]
      - [Property](property-element-net-native.md) [0:M]
      - [Field](field-element-net-native.md) [0:M]
      - [Event](event-element-net-native.md) [0:M]
  - [Library](library-element-net-native.md) [0:M]
    - [Assembly](assembly-element-net-native.md) [0:M]
      - [命名空间](namespace-element-net-native.md)[0： M]。 . .
      - [类型](type-element-net-native.md)[0： M]。 . .
      - [类型实例化](typeinstantiation-element-net-native.md)（构造泛型类型） [0： M]。 . .
    - [Namespace](namespace-element-net-native.md) [0:M]
      - [命名空间](namespace-element-net-native.md)[0： M]。 . .
      - [类型](type-element-net-native.md)[0： M]。 . .
      - [类型实例化](typeinstantiation-element-net-native.md)（构造泛型类型） [0： M]。 . .
    - [Type](type-element-net-native.md) [0:M]
      - [Subtypes](subtypes-element-net-native.md)（包含类型的子类）[O:1]
      - [类型](type-element-net-native.md)[0： M]。 . .
      - [类型实例化](typeinstantiation-element-net-native.md)（构造泛型类型） [0： M]。 . .
      - [AttributeImplies](attributeimplies-element-net-native.md)（包含类型是一种特性）[O:1]
      - [GenericParameter](genericparameter-element-net-native.md) [0:M]
      - [Method](method-element-net-native.md) [0:M]
      - [MethodInstantiation](methodinstantiation-element-net-native.md)（构造泛型方法）[0:M]
      - [Property](property-element-net-native.md) [0:M]
      - [Field](field-element-net-native.md) [0:M]
      - [Event](event-element-net-native.md) [0:M]
    - [TypeInstantiation](typeinstantiation-element-net-native.md)（构造泛型类型）[0:M]
      - [类型](type-element-net-native.md)[0： M]。 . .
      - [类型实例化](typeinstantiation-element-net-native.md)（构造泛型类型） [0： M]。 . .
      - [Method](method-element-net-native.md) [0:M]
      - [MethodInstantiation](methodinstantiation-element-net-native.md)（构造泛型方法）[0:M]
      - [Property](property-element-net-native.md) [0:M]
      - [Field](field-element-net-native.md) [0:M]
      - [Event](event-element-net-native.md) [0:M]

[Application](application-element-net-native.md) 元素可以不具任何属性，也可以具备在[“运行时指令和策略”部分](#Directives)中所讨论的策略属性。

[Library`Name` 元素具有单个属性 ](library-element-net-native.md)，该属性指定了库名或程序集名（不带文件扩展名）。 例如，以下 [Library](library-element-net-native.md) 元素适用于名为 Extensions.dll 的程序集。

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
  <Application>
     <!-- Child elements go here -->
  </Application>
  <Library Name="Extensions">
     <!-- Child elements go here -->
  </Library>
</Directives>
```

<a name="Directives"></a>

## <a name="runtime-directives-and-policy"></a>运行时指令和策略

[Application](application-element-net-native.md) 元素本身以及 [Library](library-element-net-native.md) 和 [Application](application-element-net-native.md) 元素的子元素表示策略，也就是说，它们定义了应用可将反射应用于程序元素的方式。 策略类型由元素属性定义（例如，`Serialize`）。 策略值由属性值定义（例如，`Serialize="Required"`）。

任何由元素属性指定的策略均适用于未对该策略指定值的子元素。 例如，如果策略由 [Type](type-element-net-native.md) 元素指定，则该策略适用于包含的所有尚未显式指定策略的类型和成员。

可通过 [Application](application-element-net-native.md)、[Assembly](assembly-element-net-native.md)、[AttributeImplies](attributeimplies-element-net-native.md)、[Namespace](namespace-element-net-native.md)、[Subtypes](subtypes-element-net-native.md) 和 [Type](type-element-net-native.md) 元素表示的策略有别于可针对每个成员表示的策略（通过 [Method](method-element-net-native.md)、[Property](property-element-net-native.md)、[Field](field-element-net-native.md) 和 [Event](event-element-net-native.md) 元素来表示）。

### <a name="specifying-policy-for-assemblies-namespaces-and-types"></a>为程序集、命名空间和类型指定策略

[Application](application-element-net-native.md)、[Assembly](assembly-element-net-native.md)、[AttributeImplies](attributeimplies-element-net-native.md)、[Namespace](namespace-element-net-native.md)、[Subtypes](subtypes-element-net-native.md) 和 [Type](type-element-net-native.md) 元素支持下列策略类型：

- `Activate`. 控制运行时对构造函数的访问，以启用实例激活。

- `Browse`. 控制对有关程序元素信息的查询，但并不启用任何运行时访问。

- `Dynamic`. 控制运行时对所有类型成员的访问，包括构造函数、方法、字段、属性和事件，以启用动态编程。

- `Serialize`. 控制运行时对构造函数、字段和属性的访问，以便通过 Newtonsoft JSON 序列化程序等第三方库实现类型实例的序列化。

- `DataContractSerializer`. 控制使用 <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType> 类的序列化策略。

- `DataContractJsonSerializer`. 控制使用 <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType> 类的 JSON 序列化策略。

- `XmlSerializer`. 控制使用 <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType> 类的 XML 序列化策略。

- `MarshalObject`. 控制将引用类型封送到 WinRT 和 COM 的策略。

- `MarshalDelegate`. 控制将委托类型作为函数指针封送到本机代码的策略。

- `MarshalStructure` . 控制封送结构到本机代码的策略。

与这些策略类型有关的设置如下：

- `All`. 为工具链未删除的所有类型和成员启用策略。

- `Auto`. 使用默认行为。 （未指定策略等同于将该策略设置为 `Auto`，除非该策略被替代，例如被父元素替代。）

- `Excluded`. 禁用程序元素策略。

- `Public`. 启用公共类型或成员策略，除非工具链确定该成员为多余并因此将其删除。 （在后一种情况下，你必须使用 `Required Public`，以确保该成员已被保留并且具有反射功能。）

- `PublicAndInternal`. 如果工具链未将公共和内部类型或成员删除，则为它们启用策略。

- `Required Public`. 不管公共类型和成员是否处于占用状态，工具链均须将它们保留下来，并为它们启用策略。

- `Required PublicAndInternal`. 不管公共和内部类型和成员是否处于占用状态，工具链均须将它们保留下来，并为它们启用策略。

- `Required All`. 不管所有类型和成员是否处于占用状态，工具链均须将它们保留下来，并为它们启用策略。

例如，以下运行时指令文件为程序集 DataClasses.dll 中的所有类型和成员定义了策略。 借此可以实现对所有公共属性的序列化反射；可实现对所有类型和类型成员进行浏览；可实现对所有类型的激活（由于具有 `Dynamic` 属性）；还可实现对所有公共类型和成员的反射。

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="Required Public"
                Browse="All" Activate="PublicAndInternal"
                Dynamic="Public"  />
   </Application>
   <Library Name="UtilityLibrary">
     <!-- Child elements go here -->
   </Library>
</Directives>
```

### <a name="specifying-policy-for-members"></a>为成员指定策略

[Property](property-element-net-native.md) 和 [Field](field-element-net-native.md) 元素支持下列策略类型：

- `Browse` - 控制查询有关此成员的信息，但并不启用任何运行时访问。

- `Dynamic` - 控制运行时访问所有类型的成员，包括构造函数、方法、字段、属性和事件，以启用动态编程。 还控制查询有关包含类型的信息。

- `Serialize` - 控制运行时访问成员，以便通过 Newtonsoft JSON 序列化程序等库实现类型实例的序列化和反序列化。 此策略可应用于构造函数、字段和属性。

[Method](method-element-net-native.md) 和 [Event](event-element-net-native.md) 元素支持下列策略类型：

- `Browse` - 控制查询有关此成员的信息，但并不启用任何运行时访问。

- `Dynamic` - 控制运行时访问所有类型的成员，包括构造函数、方法、字段、属性和事件，以启用动态编程。 还控制查询有关包含类型的信息。

 与这些策略类型有关的设置如下：

- `Auto` - 使用默认行为。 （未指定策略等同于将该策略设置为 `Auto`，除非某些内容将该策略替代。）

- `Excluded` - 始终不包括成员的元数据。

- `Included` - 如果输出中存在父类型，则启用策略。

- `Required` - 即便该成员显示为未使用，工具链也必须将它保留下来，并为它启用策略。

## <a name="runtime-directives-file-semantics"></a>运行时指令文件语义

可同时为较高级别和较低级别元素定义策略。 例如，我们可为程序集以及其中包含的一些类型定义策略。 如果无法表示特定的较低级别元素，则说明该元素继承了其父级的策略。 例如，如果存在 `Assembly` 元素却不存在 `Type` 元素，则说明在 `Assembly` 中指定的策略适用于数据集中的每一个类型。 多个元素也可将策略应用到同一个程序元素。 例如，不同的 [Assembly](assembly-element-net-native.md) 元素可能会以不同的方式为同一个数据集定义同一个策略元素。 以下部分将分别说明在下列情况下应如何处理特定类型策略。

泛型类型或方法的一个 [Type](type-element-net-native.md) 或 [Method](method-element-net-native.md) 元素将它的策略应用到所有不具备自身策略的实例化。 例如，为 `Type` 指定了策略的 <xref:System.Collections.Generic.List%601> 元素适用于该泛型类型的所有构造实例，除非 `List<Int32>` 元素将其替代为特殊构造的泛型类型（例如 `TypeInstantiation`）。 否则，元素将为指定的程序元素定义策略。

元素不明确时，引擎将查找匹配项，如果发现完全匹配就将使用该条匹配。 如果发现多条匹配，将出现警告或错误。

### <a name="if-two-directives-apply-policy-to-the-same-program-element"></a>如果两条指令将策略应用于同一个程序元素

如果不同运行时指令文件中的两个元素尝试将同一程序元素（例如程序集或类型）的同一策略类型设置为不同的值，则解决冲突的方法如下：

1. 如果存在 `Excluded` 元素，则该元素优先。

2. `Required` 优先于非 `Required`。

3. `All` 优先于 `PublicAndInternal`，而后者又优先于 `Public`。

4. 任何显式设置均优先于 `Auto`。

例如，如果单个项目包含以下两种运行时指令文件，DataClasses.dll 的序列化策略将设置为 `Required Public` 和 `All`。 在此情况下，序列化策略将被处理为 `Required All`。

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="Required Public"/>
   </Application>
   <Library Name="DataClasses">
      <!-- any other elements -->
   </Library>
</Directives>
```

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="All" />
   </Application>
   <Library Name="DataClasses">
      <!-- any other elements -->
   </Library>
</Directives>
```

但如果单个运行时指令文件中的两个指令尝试设置同一程序元素的同一策略类型，则 XML Scheme Definition 工具将显示错误消息。

### <a name="if-child-and-parent-elements-apply-the-same-policy-type"></a>如果子元素和父元素应用的是同一策略类型

子元素将替代其父元素，包括 `Excluded` 设置。 你想要指定 `Auto` 的主要原因就是要执行替代操作。

在以下示例中，`DataClasses` 中所有内容（`DataClasses.ViewModels` 中不含这些内容）的序列化策略设置将为 `Required Public`，而 `DataClasses` 和 `DataClasses.ViewModels` 中所有内容的序列化策略设置将为 `All`。

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="Required Public" >
         <Namespace Name="DataClasses.ViewModels" Serialize="All" />
      </Assembly>
   </Application>
   <Library Name="DataClasses">
      <!-- any other elements -->
   </Library>
</Directives>
```

### <a name="if-open-generics-and-instantiated-elements-apply-the-same-policy-type"></a>如果开放式泛型元素和已实例化的元素应用的是同一策略类型

在以下示例中，只有当引擎出于其他原因提供 `Dictionary<int,int>` 策略时（亦可为默认行为），才会为 `Browse` 分配 `Browse` 策略；<xref:System.Collections.Generic.Dictionary%602> 的所有其他实例化将具有所有可浏览的成员。

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="Required Public" >
         <Namespace Name="DataClasses.ViewModels" Serialize="All" />
      </Assembly>
      <Namespace Name="DataClasses.Generics" />
      <Type Name="Dictionary" Browse="All" />
      <TypeInstantiation Name="Dictionary"
                         Arguments="System.Int32,System.Int32" Browse="Auto" />
   </Application>
   <Library Name="DataClasses">
      <!-- any other elements -->
   </Library>
</Directives>
```

### <a name="how-policy-is-inferred"></a>如何推断出策略

每个策略类型均具有一组不同的规则，这些规则决定了该策略类型的存在如何对其他构造造成影响。

#### <a name="the-effect-of-browse-policy"></a>“浏览”策略的效果

将 `Browse` 策略应用于类型涉及下列策略更改：

- 类型的基类型标有 `Browse` 策略。

- 如果类型为实例化泛型类型，则该类型的未实例化版本将标有 `Browse` 策略。

- 如果类型为委托类型，则针对该类型的 `Invoke` 方法将标有 `Dynamic` 策略。

- 每个类型接口将标有 `Browse` 策略。

- 应用于类型的每个属性类型将标有 `Browse` 策略。

- 如果类型为泛型，则每个约束类型将标有 `Browse` 策略。

- 如果类型为泛型，则实例化该类型所采用的类型将标有 `Browse` 策略。

将 `Browse` 策略应用于方法涉及下列策略更改：

- 方法的每个参数类型将标有 `Browse` 策略。

- 方法的返回类型将标有 `Browse` 策略。

- 方法的包含类型将标有 `Browse` 策略。

- 如果方法为实例化泛型方法，则未实例化泛型方法将标有 `Browse` 策略。

- 应用于方法的每个属性类型将标有 `Browse` 策略。

- 如果方法为泛型，则每个约束类型将标有 `Browse` 策略。

- 如果方法为泛型，则实例化该方法所采用的类型将标有 `Browse` 策略。

将 `Browse` 策略应用于字段涉及下列策略更改：

- 应用于字段的每个属性类型将标有 `Browse` 策略。

- 字段类型将标有 `Browse` 策略。

- 字段从属的类型将标有 `Browse` 策略。

#### <a name="the-effect-of-dynamic-policy"></a>“动态”策略的效果

将 `Dynamic` 策略应用于类型涉及下列策略更改：

- 类型的基类型标有 `Dynamic` 策略。

- 如果类型为实例化泛型类型，则该类型的未实例化版本将标有 `Dynamic` 策略。

- 如果类型为委托类型，则针对该类型的 `Invoke` 方法将标有 `Dynamic` 策略。

- 每个类型接口将标有 `Browse` 策略。

- 应用于类型的每个属性类型将标有 `Browse` 策略。

- 如果类型为泛型，则每个约束类型将标有 `Browse` 策略。

- 如果类型为泛型，则实例化该类型所采用的类型将标有 `Browse` 策略。

将 `Dynamic` 策略应用于方法涉及下列策略更改：

- 方法的每个参数类型将标有 `Browse` 策略。

- 方法的返回类型将标有 `Dynamic` 策略。

- 方法的包含类型将标有 `Dynamic` 策略。

- 如果方法为实例化泛型方法，则未实例化泛型方法将标有 `Browse` 策略。

- 应用于方法的每个属性类型将标有 `Browse` 策略。

- 如果方法为泛型，则每个约束类型将标有 `Browse` 策略。

- 如果方法为泛型，则实例化该方法所采用的类型将标有 `Browse` 策略。

- `MethodInfo.Invoke` 可调用方法，并且可通过 <xref:System.Reflection.MethodInfo.CreateDelegate%2A?displayProperty=nameWithType> 进行委托创建。

将 `Dynamic` 策略应用于字段涉及下列策略更改：

- 应用于字段的每个属性类型将标有 `Browse` 策略。

- 字段类型将标有 `Dynamic` 策略。

- 字段从属的类型将标有 `Dynamic` 策略。

#### <a name="the-effect-of-activation-policy"></a>“激活”策略的效果

将“激活”策略应用于类型涉及下列策略更改：

- 如果类型为实例化泛型类型，则该类型的未实例化版本将标有 `Browse` 策略。

- 如果类型为委托类型，则针对该类型的 `Invoke` 方法将标有 `Dynamic` 策略。

- 类型的构造函数标有 `Activation` 策略。

将 `Activation` 策略应用于方法涉及下列策略更改：

- <xref:System.Reflection.ConstructorInfo.Invoke%2A?displayProperty=nameWithType> 和 <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> 方法可调用构造函数。 对于方法而言，`Activation` 策略仅会影响构造函数。

将 `Activation` 策略应用于字段未产生任何效果。

#### <a name="the-effect-of-serialize-policy"></a>“序列化”策略的效果

`Serialize` 策略可启用基于反射的常用序列化程序。 但由于 Microsoft 不清楚非 Microsoft 序列化程序的确切反射访问模式，该策略可能无法完全生效。

将 `Serialize` 策略应用于类型涉及下列策略更改：

- 类型的基类型标有 `Serialize` 策略。

- 如果类型为实例化泛型类型，则该类型的未实例化版本将标有 `Browse` 策略。

- 如果类型为委托类型，则针对该类型的 `Invoke` 方法将标有 `Dynamic` 策略。

- 如果类型为枚举，则类型数组将标有 `Serialize` 策略。

- 如果类型实施 <xref:System.Collections.Generic.IEnumerable%601>，则 `T` 将标有 `Serialize` 策略。

- 如果类型为 <xref:System.Collections.Generic.IEnumerable%601>、<xref:System.Collections.Generic.IList%601>、<xref:System.Collections.Generic.ICollection%601>、<xref:System.Collections.Generic.IReadOnlyCollection%601>或 <xref:System.Collections.Generic.IReadOnlyList%601>，则 `T[]` 和 <xref:System.Collections.Generic.List%601> 将标有 `Serialize` 策略，但接口类型的所有成员均未标有 `Serialize` 策略。

- 如果类型为 <xref:System.Collections.Generic.List%601>，则该类型的所有成员均未标有 `Serialize` 策略。

- 如果类型为 <xref:System.Collections.Generic.IDictionary%602>，则 <xref:System.Collections.Generic.Dictionary%602> 将标有 `Serialize` 策略。 但该类型的所有成员均未标有 `Serialize` 策略。

- 如果类型为 <xref:System.Collections.Generic.Dictionary%602>，则该类型的所有成员均未标有 `Serialize` 策略。

- 如果类型实施 <xref:System.Collections.Generic.IDictionary%602>，则 `TKey` 和 `TValue` 均标有 `Serialize` 策略。

- 每个构造函数、属性访问器和字段均标有 `Serialize` 策略。

将 `Serialize` 策略应用于方法涉及下列策略更改：

- 包含类型将标有 `Serialize` 策略。

- 方法的返回类型将标有 `Serialize` 策略。

将 `Serialize` 策略应用于字段涉及下列策略更改：

- 包含类型将标有 `Serialize` 策略。

- 字段类型将标有 `Serialize` 策略。

#### <a name="the-effect-of-xmlserializer-datacontractserializer-and-datacontractjsonserializer-policies"></a>XmlSerializer、DataContractSerializer 和 DataContractJsonSerializer 策略的影响

与用于 `Serialize` 基于反射的序列化程序的策略不同， <xref:System.Xml.Serialization.XmlSerializer> 、 <xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 策略用于启用一组对 .NET Native 工具链已知的序列化程序。 这些序列化程序并非通过反射实施，但却按照与确定可反射类型的设置类似的方式来确定可在运行时实现序列化的类型的设置。

将这些策略之一应用于类型，该类型便可借助匹配的序列化程序实现序列化。 同样，任何序列化引擎可静态确定为需要序列化的类型也可实现序列化。

这些策略对方法或字段无效。

有关详细信息，请参阅[将 Windows 应用商店应用迁移到 .NET Native](migrating-your-windows-store-app-to-net-native.md) 中的“序列化程序的差异”一节。

## <a name="see-also"></a>另请参阅

- [运行时指令元素](runtime-directive-elements.md)
- [反射和 .NET Native](reflection-and-net-native.md)
