---
title: 数据协定架构参考
ms.date: 03/30/2017
helpviewer_keywords:
- data contracts [WCF], schema reference
ms.assetid: 9ebb0ebe-8166-4c93-980a-7c8f1f38f7c0
ms.openlocfilehash: 04d1f753e5788460404942a21a29e1612f674e90
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84593563"
---
# <a name="data-contract-schema-reference"></a>数据协定架构参考

本主题介绍 <xref:System.Runtime.Serialization.DataContractSerializer> 用来描述 XML 序列化的公共语言运行库 (CLR) 类型的 XML 架构 (XSD) 的子集。

## <a name="datacontractserializer-mappings"></a>DataContractSerializer 映射

`DataContractSerializer`使用元数据终结点或工作的[元数据实用工具（svcutil.exe）](../servicemodel-metadata-utility-tool-svcutil-exe.md)从 Windows Communication Foundation （WCF）服务导出元数据时，将 CLR 类型映射到 XSD。 有关详细信息，请参阅[数据协定序列化程序](data-contract-serializer.md)。

在使用 Svcutil.exe 访问 Web 服务描述语言 (WSDL) 或 XSD 并生成服务或客户端的数据协定时， `DataContractSerializer` 还会将 XSD 映射到 CLR 类型。

只有符合此文档中所述需要的 XML 架构实例才可以使用 `DataContractSerializer`映射到 CLR 类型。

### <a name="support-levels"></a>支持级别

`DataContractSerializer` 提供对于给定的 XML 架构功能提供以下支持级别：

- **支持**。 从此功能到使用 `DataContractSerializer`的 CLR 类型或属性（或二者）具有显式映射。

- 已**忽略**。 在 `DataContractSerializer`导入的架构中允许此功能，当对代码生成无任何影响。

- **禁止**。 `DataContractSerializer` 不支持导入使用此功能的架构。 例如，Svcutil.exe 在访问带有使用这类功能的架构的 WSDL 时会转而改用 <xref:System.Xml.Serialization.XmlSerializer> 。 这是默认设置。

## <a name="general-information"></a>常规信息

- 架构命名空间在 [XML 架构](https://www.w3.org/2001/XMLSchema)中描述。 在此文档中使用前缀“xs”。

- 将忽略具有非架构命名空间的任何属性。

- 将忽略任何批注（在此文档中描述的批注除外）。

### <a name="xsschema-attributes"></a>\<xs:schema>：特性

|属性|DataContract|
|---------------|------------------|
|`attributeFormDefault`|已忽略。|
|`blockDefault`|已忽略。|
|`elementFormDefault`|必须进行限定。 必须对一个架构的所有元素进行限定才能得到 `DataContractSerializer`的支持。 这可以通过将设置 xs:schema/@elementFormDefault 为 "限定" 或 xs:element/@form 在每个单个元素声明上设置为 "合格" 来实现。|
|`finalDefault`|已忽略。|
|`Id`|已忽略。|
|`targetNamespace`|支持，并已映射到数据协定命名空间。 如果未指定此属性，则使用空白命名空间。 不能是保留的命名空间 `http://schemas.microsoft.com/2003/10/Serialization/` 。|
|`version`|已忽略。|

### <a name="xsschema-contents"></a>\<xs:schema>：内容

|目录|架构|
|--------------|------------|
|`include`|支持。 `DataContractSerializer` 支持 xs:include 和 xs:import。 但是，从本地文件加载元数据时，Svcutil.exe 会限制下面的 `xs:include/@schemaLocation` 和 `xs:import/@location` 引用。 在这种情况下，必须通过带外机制而非通过 `include` 来传递架构文件的列表；将忽略 `include`架构文档。|
|`redefine`|已禁止。 出于安全方面的原因， `xs:redefine` 禁止使用 `DataContractSerializer` ： `x:redefine` 要求后跟 `schemaLocation` 。 在某些情况下，使用 DataContract 的 Svcutil.exe 会限制 `schemaLocation`的使用。|
|`import`|支持。 `DataContractSerializer` 支持 `xs:include` 和 `xs:import`。 但是，从本地文件加载元数据时，Svcutil.exe 会限制下面的 `xs:include/@schemaLocation` 和 `xs:import/@location` 引用。 在这种情况下，必须通过带外机制而非通过 `include` 来传递架构文件的列表；将忽略 `include`架构文档。|
|`simpleType`|支持。 请参见“ `xs:simpleType` ”一节。|
|`complexType`|支持，将映射到数据协定。 请参见“ `xs:complexType` ”一节。|
|`group`|已忽略。 `DataContractSerializer` 不支持使用 `xs:group`、 `xs:attributeGroup`和 `xs:attribute`。 这些声明将作为 `xs:schema`子级被忽略，但无法从 `complexType` 或其他支持的结构内引用。|
|`attributeGroup`|已忽略。 `DataContractSerializer` 不支持使用 `xs:group`、 `xs:attributeGroup`和 `xs:attribute`。 这些声明将作为 `xs:schema`子级被忽略，但无法从 `complexType` 或其他支持的结构内引用。|
|`element`|支持。 请参见“全局元素声明 (GED)”。|
|`attribute`|已忽略。 `DataContractSerializer` 不支持使用 `xs:group`、 `xs:attributeGroup`和 `xs:attribute`。 这些声明将作为 `xs:schema`子级被忽略，但无法从 `complexType` 或其他支持的结构内引用。|
|`notation`|已忽略。|

## <a name="complex-types--xscomplextype"></a>复杂类型–\<xs:complexType>

### <a name="general-information"></a>常规信息

每个复杂类型 \<xs:complexType> 映射到一个数据协定。

### <a name="xscomplextype-attributes"></a>\<xs:complexType>：特性

|属性|架构|
|---------------|------------|
|`abstract`|必须是 false（默认值）。|
|`block`|已禁止。|
|`final`|已忽略。|
|`id`|已忽略。|
|`mixed`|必须是 false（默认值）。|
|`name`|支持，并已映射到数据协定名称。 如果名称中有句点，则尝试将类型映射到内部类型。 例如，名为 `A.B` 的复杂类型将映射到一个数据协定类型，该数据协定类型是具有数据协定名称 `A`的类型的内部类型，但前提是这种数据协定类型存在。 嵌套可能有多个级别：例如， `A.B.C` 可以是内部类型，但前提是 `A` 和 `A.B` 两者都存在。|

### <a name="xscomplextype-contents"></a>\<xs:complexType>：内容

|目录|架构|
|--------------|------------|
|`simpleContent`|禁止扩展。<br /><br /> 仅允许从 `anySimpleType`进行限制。|
|`complexContent`|支持。 请参见“继承”。|
|`group`|已禁止。|
|`all`|已禁止。|
|`choice`|禁止|
|`sequence`|支持，将映射到数据协定的数据成员。|
|`attribute`|已禁止，即使 use="prohibited"（但有一个例外）。 只支持标准序列化架构命名空间中的可选属性。 这些属性不映射到数据协定编程模型中的数据成员。 当前，仅有一个这样的属性有意义，将在“ISerializable”一节讨论。 将忽略所有其他成员。|
|`attributeGroup`|已禁止。 在 WCF v1 版本中， `DataContractSerializer` 将忽略内部的 `attributeGroup` 存在 `xs:complexType` 。|
|`anyAttribute`|已禁止。|
|（空）|映射到没有数据成员的数据协定。|

### <a name="xssequence-in-a-complex-type-attributes"></a>\<xs:sequence>在复杂类型中：特性

|属性|架构|
|---------------|------------|
|`id`|已忽略。|
|`maxOccurs`|必须是 1（默认值）。|
|`minOccurs`|必须是 1（默认值）。|

### <a name="xssequence-in-a-complex-type-contents"></a>\<xs:sequence>在复杂类型中：内容

|目录|架构|
|--------------|------------|
|`element`|每个实例都映射到一个数据成员。|
|`group`|已禁止。|
|`choice`|已禁止。|
|`sequence`|已禁止。|
|`any`|已禁止。|
|（空）|映射到没有数据成员的数据协定。|

## <a name="elements--xselement"></a>单元\<xs:element>

### <a name="general-information"></a>常规信息

`<xs:element>` 会在以下上下文中发生：

- 在描述常规（非集合）数据协定的数据成员的 `<xs:sequence>`内发生。 在这种情况下， `maxOccurs` 属性必须为 1。 （不允许值为 0。）

- 在描述集合数据协定的数据成员的 `<xs:sequence>`内发生。 在这种情况下， `maxOccurs` 属性必须大于 1 或为“unbounded”。

- 在作为全局元素声明 (GED) 的 `<xs:schema>` 内发生。

### <a name="xselement-with-maxoccurs1-within-an-xssequence-data-members"></a>\<xs:element>在 \<xs:sequence> （数据成员）中使用 maxOccurs = 1

|属性|架构|
|---------------|------------|
|`ref`|已禁止。|
|`name`|支持，映射到数据成员名称。|
|`type`|支持，映射到数据成员类型。 有关更多信息，请参见“类型/基元映射”。 如果未指定（并且元素不包含匿名类型），则假定 `xs:anyType` 。|
|`block`|已忽略。|
|`default`|已禁止。|
|`fixed`|已禁止。|
|`form`|必须进行限定。 可以通过 `elementFormDefault` 上的 `xs:schema`来设置此特性。|
|`id`|已忽略。|
|`maxOccurs`|1|
|`minOccurs`|映射到数据成员的 <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> 属性（当`IsRequired` 为 1 时， `minOccurs` 为 true）。|
|`nillable`|影响类型映射。 请参见“类型/基元映射”。|

### <a name="xselement-with-maxoccurs1-within-an-xssequence-collections"></a>\<xs:element>使用 maxOccurs>1 \<xs:sequence> （集合）

- 映射到 <xref:System.Runtime.Serialization.CollectionDataContractAttribute>。

- 在集合类型中，在 xs:sequence 内只允许一个 xs:element。

 集合可以是以下类型之一：

- 常规集合（例如，数组）。

- 字典集合（将一个值映射到另一个值，例如 <xref:System.Collections.Hashtable>）。

- 字典和密钥/值对类型的数组之间仅有的差别在于生成的编程模型。 有一种架构批注机制可用来指示给定的类型是一个字典集合。

`ref`、 `block`、 `default`、 `fixed`, `form`和 `id` 属性的规则与非集合情况下的规则相同。 其他属性包含下表中的规则。

|属性|架构|
|---------------|------------|
|`name`|支持，映射到 <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ItemName%2A> 属性 (Attribute) 中的 `CollectionDataContractAttribute` 属性 (Property)。|
|`type`|支持，映射到集合中存储的类型。|
|`maxOccurs`|大于 1 或为“unbounded”。 DC 架构应使用“unbounded”。|
|`minOccurs`|已忽略。|
|`nillable`|影响类型映射。 对于字典集合，忽略此属性。|

### <a name="xselement-within-an-xsschema-global-element-declaration"></a>\<xs:element>在 \<xs:schema> 全局元素声明中

- 在架构中的某个类型时具有相同的名称和命名空间的全局元素声明 (GED) 或在其内部指定匿名类型的全局元素声明 (GED) 可认为是与该类型相关联。

- 架构导出：将为每个生成的（简单的和复杂的）类型生成相关联的 GED。

- 反序列化/序列化：将关联的 GED 用作类型的根元素。

- 架构导入：当关联的 GED 遵循以下规则时，将不是必需的并可忽略（除非它们定义了类型）。

|属性|架构|
|---------------|------------|
|`abstract`|对于关联的 GED 必须是 false。|
|`block`|在关联的 GED 中禁止。|
|`default`|在关联的 GED 中禁止。|
|`final`|对于关联的 GED 必须是 false。|
|`fixed`|在关联的 GED 中禁止。|
|`id`|已忽略。|
|`name`|支持。 请参见关联的 GED 的定义。|
|`nillable`|对于关联的 GED 必须是 true。|
|`substitutionGroup`|在关联的 GED 中禁止。|
|`type`|支持，必须与关联的 GED 的关联类型相匹配（除非该元素包含一个匿名类型）。|

### <a name="xselement-contents"></a>\<xs:element>：内容

|目录|架构|
|--------------|------------|
|`simpleType`|支持。*|
|`complexType`|支持。*|
|`unique`|已忽略。|
|`key`|已忽略。|
|`keyref`|已忽略。|
|(空白)|支持。|

\*对 `simpleType` `complexType,` 匿名类型使用和映射与非匿名类型的映射相同，不同之处在于不存在匿名数据协定，因此创建命名的数据协定，并使用从元素名称派生的生成名称。 下面的列表中是匿名类型的规则：

- WCF 实现详细信息：如果 `xs:element` 名称不包含句点，则匿名类型映射到外部数据协定类型的内部类型。 如果名称包含句点，则结果数据协定类型是独立的（不是内部类型）。

- 内部类型的生成数据协定名称是由外部类型的数据协定名称后跟一个句点、元素名称和字符串“Type”构成。

- 如果已存在具有这个名称的数据协定，则通过在该名称后追加“1”、“2”、“3”等以使名称具有唯一性，从而创建一个唯一的名称。

## <a name="simple-types---xssimpletype"></a>简单类型-\<xs:simpleType>

### <a name="xssimpletype-attributes"></a>\<xs:simpleType>：特性

|属性|架构|
|---------------|------------|
|`final`|已忽略。|
|`id`|已忽略。|
|`name`|支持，映射到数据协定名称。|

### <a name="xssimpletype-contents"></a>\<xs:simpleType>：内容

|目录|架构|
|--------------|------------|
|`restriction`|支持。 映射到枚举数据协定。 如果与枚举模式不匹配，则忽略此属性。 请参见“ `xs:simpleType` 限制”一节。|
|`list`|支持。 映射到标志枚举数据协定。 请参见“ `xs:simpleType` 列表”一节。|
|`union`|已禁止。|

### \<xs:restriction>

- 仅 base="`xs:anyType`" 支持复杂类型限制。

- 除 `xs:string` 之外没有任何限制方面的 `xs:enumeration` 的简单类型限制映射到枚举数据协定。

- 将所有其他简单类型限制映射到它们限制的类型。 例如， `xs:int` 的限制将映射到一个整数，就像 `xs:int` 自身一样。 有关基元类型映射的详细信息，请参阅类型/基元映射。

### <a name="xsrestriction-attributes"></a>\<xs:restriction>：特性

|属性|架构|
|---------------|------------|
|`base`|必须是一个支持的简单类型或 `xs:anyType`。|
|`id`|已忽略。|

### <a name="xsrestriction-for-all-other-cases-contents"></a>\<xs:restriction>对于所有其他情况：内容

|目录|架构|
|--------------|------------|
|`simpleType`|如果存在，必须派生自支持的基元类型。|
|`minExclusive`|已忽略。|
|`minInclusive`|已忽略。|
|`maxExclusive`|已忽略。|
|`maxInclusive`|已忽略。|
|`totalDigits`|已忽略。|
|`fractionDigits`|已忽略。|
|`length`|已忽略。|
|`minLength`|已忽略。|
|`maxLength`|已忽略。|
|`enumeration`|已忽略。|
|`whiteSpace`|已忽略。|
|`pattern`|已忽略。|
|(空白)|支持。|

## <a name="enumeration"></a>枚举

### <a name="xsrestriction-for-enumerations-attributes"></a>\<xs:restriction>对于枚举：特性

|属性|架构|
|---------------|------------|
|`base`|如果存在，必须是 `xs:string`。|
|`id`|已忽略。|

### <a name="xsrestriction-for-enumerations-contents"></a>\<xs:restriction>对于枚举：内容

|目录|架构|
|--------------|------------|
|`simpleType`|如果存在，必须是数据协定支持的枚举限制（本节）。|
|`minExclusive`|已忽略。|
|`minInclusive`|已忽略。|
|`maxExclusive`|已忽略。|
|`maxInclusive`|已忽略。|
|`totalDigits`|已忽略。|
|`fractionDigits`|已忽略。|
|`length`|已禁止。|
|`minLength`|已禁止。|
|`maxLength`|已禁止。|
|`enumeration`|支持。 忽略枚举“id”，并且“值”映射到枚举数据协定上的值名称。|
|`whiteSpace`|已禁止。|
|`pattern`|已禁止。|
|（空）|支持，映射到空枚举类型。|

 下面的代码演示 C# 枚举类。

```csharp
public enum MyEnum
{
  first = 3,
  second = 4,
  third =5
}
```

此类通过 `DataContractSerializer`映射到下面的架构。 如果枚举值从 1 开始，则不生成 `xs:annotation` 块。

```xml
<xs:simpleType name="MyEnum">
  <xs:restriction base="xs:string">
    <xs:enumeration value="first">
      <xs:annotation>
        <xs:appinfo>
          <EnumerationValue xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
          3
          </EnumerationValue>
        </xs:appinfo>
      </xs:annotation>
    </xs:enumeration>
    <xs:enumeration value="second">
      <xs:annotation>
        <xs:appinfo>
          <EnumerationValue xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
          4
          </EnumerationValue>
        </xs:appinfo>
      </xs:annotation>
    </xs:enumeration>
  </xs:restriction>
</xs:simpleType>
```

### \<xs:list>

`DataContractSerializer` 将用 `System.FlagsAttribute` 标记的枚举类型映射到从 `xs:list` 派生的 `xs:string`。 不支持其他 `xs:list` 变体。

### <a name="xslist-attributes"></a>\<xs:list>：特性

|属性|架构|
|---------------|------------|
|`itemType`|已禁止。|
|`id`|已忽略。|

### <a name="xslist-contents"></a>\<xs:list>：内容

|目录|架构|
|--------------|------------|
|`simpleType`|必须是来自使用 `xs:string` 方面的 `xs:enumeration` 的限制。|

如果枚举值后面没有 2 的幂级数（标志的默认值），则在 `xs:annotation/xs:appInfo/ser:EnumerationValue` 元素中存储值。

例如，下面的代码标志一个枚举类型。

```csharp
[Flags]
public enum AuthFlags
{
  AuthAnonymous = 1,
  AuthBasic = 2,
  AuthNTLM = 4,
  AuthMD5 = 16,
  AuthWindowsLiveID = 64,
}
```

此类型映射到下面的架构。

```xml
<xs:simpleType name="AuthFlags">
    <xs:list>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:enumeration value="AuthAnonymous" />
          <xs:enumeration value="AuthBasic" />
          <xs:enumeration value="AuthNTLM" />
          <xs:enumeration value="AuthMD5">
            <xs:annotation>
              <xs:appinfo>
                <EnumerationValue xmlns="http://schemas.microsoft.com/2003/10/Serialization/">16</EnumerationValue>
              </xs:appinfo>
            </xs:annotation>
          </xs:enumeration>
          <xs:enumeration value="AuthWindowsLiveID">
            <xs:annotation>
              <xs:appinfo>
                <EnumerationValue xmlns="http://schemas.microsoft.com/2003/10/Serialization/">64</EnumerationValue>
              </xs:appinfo>
            </xs:annotation>
          </xs:enumeration>
        </xs:restriction>
      </xs:simpleType>
    </xs:list>
  </xs:simpleType>
```

## <a name="inheritance"></a>继承

### <a name="general-rules"></a>一般规则

数据协定可以继承自另一个数据协定。 这类数据协定映射到一个基协定，并由扩展类型使用 `<xs:extension>` XML 架构协定进行派生。

数据协定不能继承自集合数据协定。

例如，下面的代码是数据协定。

```csharp
[DataContract]
public class Person
{
  [DataMember]
  public string Name;
}
[DataContract]
public class Employee : Person
{
  [DataMember]
  public int ID;
}
```

此数据协定映射到下面的 XML 架构类型声明。

```xml
<xs:complexType name="Employee">
 <xs:complexContent mixed="false">
  <xs:extension base="tns:Person">
   <xs:sequence>
    <xs:element minOccurs="0" name="ID" type="xs:int"/>
   </xs:sequence>
  </xs:extension>
 </xs:complexContent>
</xs:complexType>
<xs:complexType name="Person">
 <xs:sequence>
  <xs:element minOccurs="0" name="Name"
    nillable="true" type="xs:string"/>
 </xs:sequence>
</xs:complexType>
```

### <a name="xscomplexcontent-attributes"></a>\<xs:complexContent>：特性

|属性|架构|
|---------------|------------|
|`id`|已忽略。|
|`mixed`|必须是 false。|

### <a name="xscomplexcontent-contents"></a>\<xs:complexContent>：内容

|目录|架构|
|--------------|------------|
|`restriction`|已禁止，除了 base="`xs:anyType`"。 后者等效于将 `xs:restriction` 的内容直接放到 `xs:complexContent`的容器下。|
|`extension`|支持。 映射到数据协定继承。|

### <a name="xsextension-in-xscomplexcontent-attributes"></a>\<xs:extension>in \<xs:complexContent> ：特性

|属性|架构|
|---------------|------------|
|`id`|已忽略。|
|`base`|支持。 映射到此类型继承自的基数据协定类型。|

### <a name="xsextension-in-xscomplexcontent-contents"></a>\<xs:extension>在中 \<xs:complexContent> ：内容

规则与 `<xs:complexType>` 内容的规则相同。

如果提供 `<xs:sequence>` ，则其成员元素将映射到存在于派生的数据协定中的附加数据成员。

如果派生的类型包含与基类型中的元素有相同名称的元素，则重复元素声明映射到一个具有生成的唯一名称的数据成员。 将向数据成员名称添加正整数数字（“member1”、“member2”等）直至查找到唯一的名称。 相反：

- 如果派生的数据协定有一个数据成员与基数据协定中的数据成员具有相同的名称和类型，则 `DataContractSerializer` 在派生的类型中生成此对应的元素。

- 如果派生的数据协定有一个数据成员与基数据协定中的数据成员具有相同的名称，但类型不同，则 `DataContractSerializer` 使用基类型和派生的类型声明中的 `xs:anyType` 类型的元素导入架构。 在 `xs:annotations/xs:appInfo/ser:ActualType/@Name`中保留原始类型名称。

两个变体可能会导致一个使用有歧义的内容模型的架构，此内容模型取决于各自数据成员的顺序。

## <a name="typeprimitive-mapping"></a>类型/基元映射

`DataContractSerializer` 使用下面的 XML 架构基元类型映射。

|XSD 类型|.NET 类型|
|--------------|---------------|
|`anyType`|<xref:System.Object>.|
|`anySimpleType`|<xref:System.String>.|
|`duration`|<xref:System.TimeSpan>.|
|`dateTime`|<xref:System.DateTime>.|
|`dateTimeOffset`|表示偏移量的<xref:System.DateTime> 和 <xref:System.TimeSpan> 。 请参见下面的“DateTimeOffset 序列化”。|
|`time`|<xref:System.String>.|
|`date`|<xref:System.String>.|
|`gYearMonth`|<xref:System.String>.|
|`gYear`|<xref:System.String>.|
|`gMonthDay`|<xref:System.String>.|
|`gDay`|<xref:System.String>.|
|`gMonth`|<xref:System.String>.|
|`boolean`|<xref:System.Boolean>|
|`base64Binary`|<xref:System.Byte> 数组。|
|`hexBinary`|<xref:System.String>.|
|`float`|<xref:System.Single>.|
|`double`|<xref:System.Double>.|
|`anyURI`|<xref:System.Uri>.|
|`QName`|<xref:System.Xml.XmlQualifiedName>.|
|`string`|<xref:System.String>.|
|`normalizedString`|<xref:System.String>.|
|`token`|<xref:System.String>.|
|`language`|<xref:System.String>.|
|`Name`|<xref:System.String>.|
|`NCName`|<xref:System.String>.|
|`ID`|<xref:System.String>.|
|`IDREF`|<xref:System.String>.|
|`IDREFS`|<xref:System.String>.|
|`ENTITY`|<xref:System.String>.|
|`ENTITIES`|<xref:System.String>.|
|`NMTOKEN`|<xref:System.String>.|
|`NMTOKENS`|<xref:System.String>.|
|`decimal`|<xref:System.Decimal>.|
|`integer`|<xref:System.Int64>.|
|`nonPositiveInteger`|<xref:System.Int64>.|
|`negativeInteger`|<xref:System.Int64>.|
|`long`|<xref:System.Int64>.|
|`int`|<xref:System.Int32>.|
|`short`|<xref:System.Int16>.|
|`Byte`|<xref:System.SByte>.|
|`nonNegativeInteger`|<xref:System.Int64>.|
|`unsignedLong`|<xref:System.UInt64>.|
|`unsignedInt`|<xref:System.UInt32>.|
|`unsignedShort`|<xref:System.UInt16>.|
|`unsignedByte`|<xref:System.Byte>.|
|`positiveInteger`|<xref:System.Int64>.|

## <a name="iserializable-types-mapping"></a>ISerializable 类型映射

在 .NET Framework 版本1.0 中， <xref:System.Runtime.Serialization.ISerializable> 是作为一种用于序列化对象以实现持久性或数据传输的常规机制。 可以实现许多 .NET Framework 类型，这些类型 `ISerializable` 可以在应用程序之间传递。 <xref:System.Runtime.Serialization.DataContractSerializer> 自然支持 `ISerializable` 类。 `DataContractSerializer` 映射到 `ISerializable` 实现架构类型，这些类型仅在类型的 QName（限定名）上存在不同，并且实际上是属性集合。 例如， `DataContractSerializer` 映射 <xref:System.Exception> 到命名空间中的以下 XSD 类型 `http://schemas.datacontract.org/2004/07/System` 。

```xml
<xs:complexType name="Exception">
 <xs:sequence>
  <xs:any minOccurs="0" maxOccurs="unbounded"
      namespace="##local" processContents="skip"/>
 </xs:sequence>
 <xs:attribute ref="ser:FactoryType"/>
</xs:complexType>
```

数据协定序列化架构中声明的可选属性 `ser:FactoryType` 引用可以反序列化类型的工厂类。 工厂类必须是正在使用的 `DataContractSerializer` 实例的已知类型集合的一部分。 有关已知类型的详细信息，请参阅[数据协定已知类型](data-contract-known-types.md)。

## <a name="datacontract-serialization-schema"></a>DataContract 序列化架构

许多由 `DataContractSerializer` 导出的架构都使用一个特殊数据协定序列化命名空间中的类型、元素和属性：

`http://schemas.microsoft.com/2003/10/Serialization`

下面是一个完整的数据协定序列化架构声明。

```xml
<xs:schema attributeFormDefault="qualified"
   elementFormDefault="qualified"
   targetNamespace =
    "http://schemas.microsoft.com/2003/10/Serialization/"
   xmlns:xs="http://www.w3.org/2001/XMLSchema"
   xmlns:tns="http://schemas.microsoft.com/2003/10/Serialization/">

 <!-- Top-level elements for primitive types. -->
 <xs:element name="anyType" nillable="true" type="xs:anyType"/>
 <xs:element name="anyURI" nillable="true" type="xs:anyURI"/>
 <xs:element name="base64Binary"
       nillable="true" type="xs:base64Binary"/>
 <xs:element name="boolean" nillable="true" type="xs:boolean"/>
 <xs:element name="byte" nillable="true" type="xs:byte"/>
 <xs:element name="dateTime" nillable="true" type="xs:dateTime"/>
 <xs:element name="decimal" nillable="true" type="xs:decimal"/>
 <xs:element name="double" nillable="true" type="xs:double"/>
 <xs:element name="float" nillable="true" type="xs:float"/>
 <xs:element name="int" nillable="true" type="xs:int"/>
 <xs:element name="long" nillable="true" type="xs:long"/>
 <xs:element name="QName" nillable="true" type="xs:QName"/>
 <xs:element name="short" nillable="true" type="xs:short"/>
 <xs:element name="string" nillable="true" type="xs:string"/>
 <xs:element name="unsignedByte"
       nillable="true" type="xs:unsignedByte"/>
 <xs:element name="unsignedInt"
       nillable="true" type="xs:unsignedInt"/>
 <xs:element name="unsignedLong"
       nillable="true" type="xs:unsignedLong"/>
 <xs:element name="unsignedShort"
       nillable="true" type="xs:unsignedShort"/>

 <!-- Primitive types introduced for certain .NET simple types. -->
 <xs:element name="char" nillable="true" type="tns:char"/>
 <xs:simpleType name="char">
  <xs:restriction base="xs:int"/>
 </xs:simpleType>

 <!-- xs:duration is restricted to an ordered value space,
    to map to System.TimeSpan -->
 <xs:element name="duration" nillable="true" type="tns:duration"/>
 <xs:simpleType name="duration">
  <xs:restriction base="xs:duration">
   <xs:pattern
     value="\-?P(\d*D)?(T(\d*H)?(\d*M)?(\d*(\.\d*)?S)?)?"/>
   <xs:minInclusive value="-P10675199DT2H48M5.4775808S"/>
   <xs:maxInclusive value="P10675199DT2H48M5.4775807S"/>
  </xs:restriction>
 </xs:simpleType>

 <xs:element name="guid" nillable="true" type="tns:guid"/>
 <xs:simpleType name="guid">
  <xs:restriction base="xs:string">
   <xs:pattern value="[\da-fA-F]{8}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{12}"/>
  </xs:restriction>
 </xs:simpleType>

 <!-- This is used for schemas exported from ISerializable type. -->
 <xs:attribute name="FactoryType" type="xs:QName"/>
</xs:schema>
```

应注意以下事项：

- 已引入`ser:char` 来表示 <xref:System.Char>类型的 Unicode 字符。

- `valuespace` 的 `xs:duration` 已简化为一个有序集，因此可以将其映射到 <xref:System.TimeSpan>。

- 在从派生自`FactoryType` 的类型中导出的架构中使用 <xref:System.Runtime.Serialization.ISerializable>。

## <a name="importing-non-datacontract-schemas"></a>导入非 DataContract 架构

`DataContractSerializer` 具有 `ImportXmlTypes` 选项，可允许导入不符合 `DataContractSerializer` XSD 配置文件的架构（请参见 <xref:System.Runtime.Serialization.XsdDataContractImporter.Options%2A> 属性）。 将此选项设置为 `true` 可接受不符合要求的架构类型，并将其映射到下面的实现，即包装 <xref:System.Xml.Serialization.IXmlSerializable> 数组的 <xref:System.Xml.XmlNode> （仅类名称不同）。

```csharp
[GeneratedCodeAttribute("System.Runtime.Serialization", "3.0.0.0")]
[System.Xml.Serialization.XmlSchemaProviderAttribute("ExportSchema")]
[System.Xml.Serialization.XmlRootAttribute(IsNullable=false)]
public partial class Person : object, IXmlSerializable
{
  private XmlNode[] nodesField;
  private static XmlQualifiedName typeName =
new XmlQualifiedName("Person","http://Microsoft.ServiceModel.Samples");
  public XmlNode[] Nodes
  {
    get {return this.nodesField;}
    set {this.nodesField = value;}
  }
  public void ReadXml(XmlReader reader)
  {
    this.nodesField = XmlSerializableServices.ReadNodes(reader);
  }
  public void WriteXml(XmlWriter writer)
  {
    XmlSerializableServices.WriteNodes(writer, this.Nodes);
  }
  public System.Xml.Schema.XmlSchema GetSchema()
  {
    return null;
  }
  public static XmlQualifiedName ExportSchema(XmlSchemaSet schemas)
  {
    XmlSerializableServices.AddDefaultSchema(schemas, typeName);
    return typeName;
  }
}
```

## <a name="datetimeoffset-serialization"></a>DateTimeOffset 序列化

未将 <xref:System.DateTimeOffset> 视为基元类型， 而是将其序列化为具有两部分的复杂元素。 第一部分表示日期时间，第二部分表示日期时间的偏移量。 在下面的代码中演示已序列化的 DateTimeOffset 值的示例。

```xml
<OffSet xmlns:a="http://schemas.datacontract.org/2004/07/System">
  <DateTime i:type="b:dateTime" xmlns=""
    xmlns:b="http://www.w3.org/2001/XMLSchema">2008-08-28T08:00:00
  </DateTime>
  <OffsetMinutes i:type="b:short" xmlns=""
   xmlns:b="http://www.w3.org/2001/XMLSchema">-480
   </OffsetMinutes>
</OffSet>
```

相应的架构如下所示。

```xml
<xs:schema targetNamespace="http://schemas.datacontract.org/2004/07/System">
   <xs:complexType name="DateTimeOffset">
      <xs:sequence minOccurs="1" maxOccurs="1">
         <xs:element name="DateTime" type="xs:dateTime"
         minOccurs="1" maxOccurs="1" />
         <xs:element name="OffsetMinutes" type="xs:short"
         minOccurs="1" maxOccurs="1" />
      </xs:sequence>
   </xs:complexType>
</xs:schema>
```

## <a name="see-also"></a>请参阅

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- <xref:System.Runtime.Serialization.XsdDataContractImporter>
- [使用数据协定](using-data-contracts.md)
