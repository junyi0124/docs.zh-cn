---
title: 编写自定义特性
description: 在 .NET 中设计你自己的自定义属性。 自定义属性本质上是直接或间接派生自 System.Attribute 的类。
ms.date: 07/17/2018
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- multiple attribute instances
- AttributeTargets enumeration
- attributes [.NET], custom
- AllowMultiple property
- custom attributes
- AttributeUsageAttribute class, custom attributes
- Inherited property
- attribute classes, declaring
ms.assetid: 97216f69-bde8-49fd-ac40-f18c500ef5dc
ms.openlocfilehash: e3c97f28a05f2e5396872fe808cae0d48d5a4824
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726998"
---
# <a name="writing-custom-attributes"></a>编写自定义特性

要设计你自己的自定义特性，无需掌握许多新的概念。 如果你熟悉面向对象的编程，并且知道如何设计类，那么你已经具备大部分所需知识。 自定义特性本质上是直接或间接派生自 <xref:System.Attribute?displayProperty=nameWithType>的传统类。 与传统类一样，自定义特性包含用于存储和检索数据的方法。  
  
 正确设计自定义特性的主要步骤如下：  
  
- [应用 AttributeUsageAttribute](#applying-the-attributeusageattribute)  
  
- [声明特性类](#declaring-the-attribute-class)  
  
- [声明构造函数](#declaring-constructors)  
  
- [声明属性](#declaring-properties)  
  
 本节描述上述各个步骤，并以 [自定义特性的示例](#custom-attribute-example)结束本节的描述。  
  
## <a name="applying-the-attributeusageattribute"></a>应用 AttributeUsageAttribute  

 自定义特性声明以 <xref:System.AttributeUsageAttribute?displayProperty=nameWithType> 开头，定义了特性类的一些主要特征。 例如，你可以指定其他类是否可以继承你的特性，或者指定此特性可以应用到哪些元素。 下面的代码片段演示了 <xref:System.AttributeUsageAttribute> 的使用方式。  
  
 [!code-cpp[Conceptual.Attributes.Usage#5](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#5)]
 [!code-csharp[Conceptual.Attributes.Usage#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#5)]
 [!code-vb[Conceptual.Attributes.Usage#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#5)]  
  
 <xref:System.AttributeUsageAttribute> 包含下列三个成员，它们对创建自定义属性非常重要：[AttributeTargets](#attributetargets-member)、[Inherited](#inherited-property) 和 [AllowMultiple](#allowmultiple-property)。  
  
### <a name="attributetargets-member"></a>AttributeTargets 成员  

 上述示例中指定了 <xref:System.AttributeTargets.All?displayProperty=nameWithType>，它表示此特性可应用于所有程序元素。 或者，你可指定 <xref:System.AttributeTargets.Class?displayProperty=nameWithType> 和 <xref:System.AttributeTargets.Method?displayProperty=nameWithType>，前者表示你的特性仅可适用于一个类，后者表示你的特性仅可应用于一种方法。 所有程序元素都可以通过这种方式使用自定义特性来标记，以对其进行描述。  
  
 你还可传递多个 <xref:System.AttributeTargets> 值。 下面的代码段指定了自定义特性可应用于任何类或方法。  
  
 [!code-cpp[Conceptual.Attributes.Usage#6](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#6)]
 [!code-csharp[Conceptual.Attributes.Usage#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#6)]
 [!code-vb[Conceptual.Attributes.Usage#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#6)]  
  
### <a name="inherited-property"></a>Inherited 属性  

 <xref:System.AttributeUsageAttribute.Inherited%2A?displayProperty=nameWithType> 属性指明要对其应用属性的类的派生类能否继承此属性。 此属性使用 `true`（默认值）或 `false` 标志。 在下例中，`MyAttribute` 的 <xref:System.AttributeUsageAttribute.Inherited%2A> 值为默认值 `true`，而 `YourAttribute` 的 <xref:System.AttributeUsageAttribute.Inherited%2A> 值为 `false`。  
  
 [!code-cpp[Conceptual.Attributes.Usage#7](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#7)]
 [!code-csharp[Conceptual.Attributes.Usage#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#7)]
 [!code-vb[Conceptual.Attributes.Usage#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#7)]  
  
 然后，这两个特性应用于基类 `MyClass`中的方法。  
  
 [!code-cpp[Conceptual.Attributes.Usage#9](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#9)]
 [!code-csharp[Conceptual.Attributes.Usage#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#9)]
 [!code-vb[Conceptual.Attributes.Usage#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#9)]  
  
 最后，从基类 `YourClass` 中继承类 `MyClass`。 方法 `MyMethod` 显示 `MyAttribute`，但不显示 `YourAttribute`。  
  
 [!code-cpp[Conceptual.Attributes.Usage#10](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#10)]
 [!code-csharp[Conceptual.Attributes.Usage#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#10)]
 [!code-vb[Conceptual.Attributes.Usage#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#10)]  
  
### <a name="allowmultiple-property"></a>AllowMultiple 属性  

 <xref:System.AttributeUsageAttribute.AllowMultiple%2A?displayProperty=nameWithType> 属性指明元素能否包含属性的多个实例。 如果设置为 `true`，则允许多个实例；如果设置为 `false`（默认值），则只允许一个实例。  
  
 在下例中，`MyAttribute` 的 <xref:System.AttributeUsageAttribute.AllowMultiple%2A> 值为默认值 `false`，而 `YourAttribute` 的值为 `true`。  
  
 [!code-cpp[Conceptual.Attributes.Usage#11](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#11)]
 [!code-csharp[Conceptual.Attributes.Usage#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#11)]
 [!code-vb[Conceptual.Attributes.Usage#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#11)]  
  
 当应用这些特性的多个实例时， `MyAttribute` 会生成编译器错误。 下面的代码示例显示 `YourAttribute` 的有效用法和 `MyAttribute`的无效用法。  
  
 [!code-cpp[Conceptual.Attributes.Usage#13](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#13)]
 [!code-csharp[Conceptual.Attributes.Usage#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#13)]
 [!code-vb[Conceptual.Attributes.Usage#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#13)]  
  
 如果 <xref:System.AttributeUsageAttribute.AllowMultiple%2A> 属性和 <xref:System.AttributeUsageAttribute.Inherited%2A> 属性都设置为 `true`，则从另一个类继承的类可继承一个特性，并且具有同一子类中应用的同一特性的另一个实例。 如果 <xref:System.AttributeUsageAttribute.AllowMultiple%2A> 设置为 `false`，则父类中的所有特性的值将被子类中同一特性的新实例覆盖。  
  
## <a name="declaring-the-attribute-class"></a>声明特性类  

 应用 <xref:System.AttributeUsageAttribute>以后，可以开始定义特性的细节。 特性类的声明类似于传统类的声明，如以下代码所示。  
  
 [!code-cpp[Conceptual.Attributes.Usage#14](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#14)]
 [!code-csharp[Conceptual.Attributes.Usage#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#14)]
 [!code-vb[Conceptual.Attributes.Usage#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#14)]  
  
 此特性定义说明了以下几点：  
  
- 特性类必须声明为公共类。  
  
- 按照约定，特性类的名称以单词 **Attribute** 结束。 尽管没有要求，但仍建议执行此约定以保证可读性。 应用特性时，可以选择是否包含单词 Attribute。  
  
- 所有特性类必须直接或间接从 <xref:System.Attribute?displayProperty=nameWithType> 继承。  
  
- 在 Microsoft Visual Basic 中，所有自定义特性类必须具有 <xref:System.AttributeUsageAttribute?displayProperty=nameWithType> 特性。  
  
## <a name="declaring-constructors"></a>声明构造函数  

 与传统类的初始化方式相同，使用构造函数初始化特性。 下面的代码段阐明了典型的特性构造函数。 此公共构造函数采用一个参数，并设置一个等于其值的成员变量。  
  
 [!code-cpp[Conceptual.Attributes.Usage#15](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#15)]
 [!code-csharp[Conceptual.Attributes.Usage#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#15)]
 [!code-vb[Conceptual.Attributes.Usage#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#15)]  
  
 可以重载此构造函数以适应值的各种组合。 如果你还为自定义特性类定义了 [属性](/previous-versions/visualstudio/visual-studio-2013/65zdfbdt(v=vs.120)) ，则在初始化该特性时可以使用命名参数和定位参数的组合。 通常情况下，将所有必选的参数定义为定位参数，将所有可选的参数定义为命名参数。 在这种情况下，没有必选参数则无法初始化特性。 其他所有参数都是可选参数。 请注意，在 Visual Basic 中，特性类的构造函数不应使用 ParamArray 参数。  
  
 下面的代码示例显示如何使用可选和必选参数应用使用上例中的构造函数的特性。 该示例假定特性有一个必选的布尔值和一个可选的字符串属性。  
  
 [!code-cpp[Conceptual.Attributes.Usage#17](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#17)]
 [!code-csharp[Conceptual.Attributes.Usage#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#17)]
 [!code-vb[Conceptual.Attributes.Usage#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#17)]  
  
## <a name="declaring-properties"></a>声明属性  

 如果你想要定义一个命名参数，或者提供一种简单的方法来返回由特性存储的值，请声明 [属性](/previous-versions/visualstudio/visual-studio-2013/65zdfbdt(v=vs.120))。 应将特性的属性声明为公共实体，此公告实体包含将返回的数据类型的描述。 定义将保存属性值的变量，并将此变量与 **get** 和 **set** 方法相关联。 下面的代码示例说明如何在特性中实现一个简单属性。  
  
 [!code-cpp[Conceptual.Attributes.Usage#16](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#16)]
 [!code-csharp[Conceptual.Attributes.Usage#16](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#16)]
 [!code-vb[Conceptual.Attributes.Usage#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#16)]  
  
## <a name="custom-attribute-example"></a>自定义特性的示例  

 本节内容结合了前面的信息，显示如何设计一个简单特性来记录有关代码段的作者的信息。 本示例中的特性存储了编程人员的姓名和级别，以及是否已检查此代码的信息。 它使用三个私有变量来存储要保存的实际值。 每个变量用获取和设置这些值的公共属性表示。 最后，使用两个必选参数定义构造函数。  
  
 [!code-cpp[Conceptual.Attributes.Usage#4](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#4)]
 [!code-csharp[Conceptual.Attributes.Usage#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#4)]
 [!code-vb[Conceptual.Attributes.Usage#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#4)]  
  
 可以采用以下任一种方法，使用全称 `DeveloperAttribute`或缩写名称 `Developer`应用此特性。  
  
 [!code-cpp[Conceptual.Attributes.Usage#12](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source2.cpp#12)]
 [!code-csharp[Conceptual.Attributes.Usage#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source2.cs#12)]
 [!code-vb[Conceptual.Attributes.Usage#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source2.vb#12)]  
  
 第一个示例显示只应用了必选命名参数的特性，第二个示例显示同时应用了必选参数和可选参数的特性。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Attribute?displayProperty=nameWithType>
- <xref:System.AttributeUsageAttribute?displayProperty=nameWithType>
- [特性](index.md)
