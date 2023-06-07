---
title: 协定优先工具
ms.date: 03/30/2017
ms.assetid: 0a880690-f460-4475-a5f4-9f91ce08fcc6
ms.openlocfilehash: 1896a76892c76fb7277c3e36978604a4d290018e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96255125"
---
# <a name="contract-first-tool"></a>协定优先工具

服务协定往往需要从现有的服务创建。 在 .NET Framework 4.5 及更高版本中，可以使用协定优先工具从现有服务中自动创建数据协定类。 要使用协定优先工具，必须本地下载 XML 架构定义 (XSD) 文件；该工具无法通过 HTTP 导入远程数据协定。

 协定优先工具作为生成任务集成到 Visual Studio 2012 中。 由生成任务所生成的代码文件在每次生成项目时创建，以使项目可以轻松地采用基础服务协定中的更改。

 协定优先工具可以导入的架构类型包括：

```xml
<xsd:complexType>
 <xsd:simpleType>
 </xsd:simpleType>
</xsd:complexType>
```

 如果它们是基元（如 `Int16` 或 `String`），将不会生成简单类型；如果它们的类型为 `Collection`，将不会生成复杂类型。 如果它们是另一 `xsd:complexType` 的组成部分，则也不会生成类型。 在所有这些情况下，这些类型将会被引用到项目中的现有类型。

## <a name="adding-a-data-contract-to-a-project"></a>向项目中添加数据协定

 使用协定优先工具之前，服务协定 (XSD) 必须添加到项目中。 出于本概述的需要，以下协定将用于阐述协定优先函数。 此服务定义是必应的搜索 API 所使用的服务协定的一小部分。

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema id="ServiceSchema"
    targetNamespace="http://tempuri.org/ServiceSchema.xsd"
    elementFormDefault="qualified"
    xmlns="http://tempuri.org/ServiceSchema.xsd"
    xmlns:mstns="http://tempuri.org/ServiceSchema.xsd"
    xmlns:xs="http://www.w3.org/2001/XMLSchema"
>
  <xs:complexType name="SearchRequest">
    <xs:sequence>
      <xs:element minOccurs="0" maxOccurs="1" name="Version" type="xs:string" default="2.2" />
      <xs:element minOccurs="0" maxOccurs="1" name="Market" type="xs:string" />
      <xs:element minOccurs="0" maxOccurs="1" name="UILanguage" type="xs:string" />
      <xs:element minOccurs="1" maxOccurs="1" name="Query" type="xs:string" />
      <xs:element minOccurs="1" maxOccurs="1" name="AppId" type="xs:string" />
      <xs:element minOccurs="0" maxOccurs="1" name="Latitude" type="xs:double" />
      <xs:element minOccurs="0" maxOccurs="1" name="Longitude" type="xs:double" />
      <xs:element minOccurs="0" maxOccurs="1" name="Radius" type="xs:double" />
    </xs:sequence>
  </xs:complexType>
  <xs:simpleType name="WebSearchOption">
    <xs:restriction base="xs:string">
      <xs:enumeration value="DisableHostCollapsing" />
      <xs:enumeration value="DisableQueryAlterations" />
    </xs:restriction>
  </xs:simpleType>
</xs:schema>
```

 若要将上述服务协定添加到项目中，请右键单击该项目，然后选择 "添加" " **新建 ...**"。 从“模板”对话框的 WCF 窗格中选择架构定义，然后将新文件命名为 SampleContract.xsd。 复制上面的代码并粘贴到新文件的代码视图中。

## <a name="configuring-contract-first-options"></a>配置协定优先选项

 协定优先选项可在 WCF 项目的 "属性" 菜单中进行配置。 若要启用协定优先开发，请在 "项目属性" 窗口的 "WCF" 页中选中 "将 **XSD 作为类型定义语言启用** " 复选框。

 ![启用了协定优先开发的 WCF 选项的屏幕截图。](./media/contract-first-tool/contract-first-options.png)

 要配置高级属性，请单击“高级”按钮。

 !["高级协定代码生成设置" 对话框。](./media/contract-first-tool/advanced-contract-settings.png)

 可以从协定中配置下列高级设置用于代码生成。 只能为项目中的所有文件配置设置；目前不能为单独的文件配置设置。

- **序列化程序模式**：此设置确定用于读取服务协定文件的序列化程序。 选择 " **XML 序列化程序** " 时，" **集合类型** " 和 " **重复使用类型** " 选项处于禁用状态。 这些选项仅适用于 **数据协定序列化程序**。

- **重用类型**：此设置指定用于类型重用的库。 此设置仅适用于 **序列化程序模式** 设置为 **数据协定序列化程序** 的情况。

- **集合类型**：此设置指定要用于集合数据类型的完全限定或程序集限定类型。 此设置仅适用于 **序列化程序模式** 设置为 **数据协定序列化程序** 的情况。

- **字典类型**：此设置指定要用于字典数据类型的完全限定或程序集限定类型。

- **EnableDataBinding**：此设置指定是否 <xref:System.ComponentModel.INotifyPropertyChanged> 在所有数据类型上实现接口以实现数据绑定。

- **ExcludedTypes**：此设置指定要从引用的程序集中排除的完全限定或程序集限定类型的列表。 此设置仅适用于 **序列化程序模式** 设置为 **数据协定序列化程序** 的情况。

- **GenerateInternalTypes**：此设置指定是否生成标记为内部的类。 此设置仅适用于 **序列化程序模式** 设置为 **数据协定序列化程序** 的情况。

- **GenerateSerializableTypes**：此设置指定是否生成具有属性的类 <xref:System.SerializableAttribute> 。 此设置仅适用于 **序列化程序模式** 设置为 **数据协定序列化程序** 的情况。

- **ImportXMLTypes**：此设置指定是否将数据协定序列化程序配置为在 <xref:System.SerializableAttribute> 没有属性的情况下将属性应用于类 <xref:System.Runtime.Serialization.DataContractAttribute> 。  此设置仅适用于 **序列化程序模式** 设置为 **数据协定序列化程序** 的情况。

- **SupportFx35TypedDataSets**：此设置指定是否为为 .NET Framework 3.5 创建的类型化数据集提供附加功能。 当  **序列化程序模式** 设置为 **xml 序列化** 程序时， <xref:System.Data.Design.TypedDataSetSchemaImporterExtensionFx35> 当此值设置为 True 时，该扩展将添加到 xml 架构导入程序。 当  **序列化程序模式** 设置为 " **数据协定序列化程序**" 时， <xref:System.DateTimeOffset> 如果此值设置为 "False"，则将从引用中排除该类型，以便 <xref:System.DateTimeOffset> 始终为较早版本的框架生成。

- **InputXsdFiles**：此设置指定输入文件的列表。 每个文件都必须包含有效的 XML 架构。

- **Language**：此设置指定生成的协定代码的语言。 该设置必须为 <xref:System.CodeDom.Compiler.CodeDomProvider> 所识别。

- **NamespaceMappings**：此设置指定从 XSD 目标命名空间到 CLR 命名空间的映射。 每个映射应使用以下格式：

    ```xml
    "Schema Namespace, CLR Namespace"
    ```

     XML 序列化程序只接受以下格式的一个映射：

    ```xml
    "*, CLR Namespace"
    ```

- **OutputDirectory**：此设置指定将在其中生成代码文件的目录。

 这些设置将用于在生成项目时从服务协定文件中生成服务协定类型。

## <a name="using-contract-first-development"></a>使用协定优先开发

 将服务协定添加到项目并确认生成设置后，按 **F6** 生成项目。 然后，服务协定中定义的类型将可用于项目中。

 若要使用在服务协定中定义的类型，请在当前命名空间下添加对 `ContractTypes` 的引用：

```csharp
using MyProjectNamespace.ContractTypes;
```

 然后，在服务协定中定义的类型可在项目中解析，如下所示：

 ![键入前几个字母后，在 IntelliSense 中显示 SearchRequest 类。](./media/contract-first-tool/service-contract-types.png)

 在 GeneratedXSDTypes.cs 文件中创建由工具生成的类型。 \<project directory>默认情况下，在/Obj//XSDGeneratedCode/目录中创建文件 \<build configuration> 。 本文开头的示例架构转换如下：

```csharp
//------------------------------------------------------------------------------
// <auto-generated>
//     This code was generated by a tool.
//     Runtime Version:4.0.30319.17330
//
//     Changes to this file may cause incorrect behavior and will be lost if
//     the code is regenerated.
// </auto-generated>
//------------------------------------------------------------------------------

namespace TestXSD3.ContractTypes
{
    using System.Xml.Serialization;

    /// <remarks/>
    [System.CodeDom.Compiler.GeneratedCodeAttribute("System.Xml", "4.0.30319.17330")]
    [System.SerializableAttribute()]
    [System.Diagnostics.DebuggerStepThroughAttribute()]
    [System.ComponentModel.DesignerCategoryAttribute("code")]
    [System.Xml.Serialization.XmlTypeAttribute(Namespace="http://tempuri.org/ServiceSchema.xsd")]
    [System.Xml.Serialization.XmlRootAttribute(Namespace="http://tempuri.org/ServiceSchema.xsd", IsNullable=true)]
    public partial class SearchRequest
    {

        private string versionField;

        private string marketField;

        private string uILanguageField;

        private string queryField;

        private string appIdField;

        private double latitudeField;

        private bool latitudeFieldSpecified;

        private double longitudeField;

        private bool longitudeFieldSpecified;

        private double radiusField;

        private bool radiusFieldSpecified;

        public SearchRequest()
        {
            this.versionField = "2.2";
        }

        /// <remarks/>
        [System.ComponentModel.DefaultValueAttribute("2.2")]
        public string Version
        {
            get
            {
                return this.versionField;
            }
            set
            {
                this.versionField = value;
            }
        }

        /// <remarks/>
        public string Market
        {
            get
            {
                return this.marketField;
            }
            set
            {
                this.marketField = value;
            }
        }

        /// <remarks/>
        public string UILanguage
        {
            get
            {
                return this.uILanguageField;
            }
            set
            {
                this.uILanguageField = value;
            }
        }

        /// <remarks/>
        public string Query
        {
            get
            {
                return this.queryField;
            }
            set
            {
                this.queryField = value;
            }
        }

        /// <remarks/>
        public string AppId
        {
            get
            {
                return this.appIdField;
            }
            set
            {
                this.appIdField = value;
            }
        }

        /// <remarks/>
        public double Latitude
        {
            get
            {
                return this.latitudeField;
            }
            set
            {
                this.latitudeField = value;
            }
        }

        /// <remarks/>
        [System.Xml.Serialization.XmlIgnoreAttribute()]
        public bool LatitudeSpecified
        {
            get
            {
                return this.latitudeFieldSpecified;
            }
            set
            {
                this.latitudeFieldSpecified = value;
            }
        }

        /// <remarks/>
        public double Longitude
        {
            get
            {
                return this.longitudeField;
            }
            set
            {
                this.longitudeField = value;
            }
        }

        /// <remarks/>
        [System.Xml.Serialization.XmlIgnoreAttribute()]
        public bool LongitudeSpecified
        {
            get
            {
                return this.longitudeFieldSpecified;
            }
            set
            {
                this.longitudeFieldSpecified = value;
            }
        }

        /// <remarks/>
        public double Radius
        {
            get
            {
                return this.radiusField;
            }
            set
            {
                this.radiusField = value;
            }
        }

        /// <remarks/>
        [System.Xml.Serialization.XmlIgnoreAttribute()]
        public bool RadiusSpecified
        {
            get
            {
                return this.radiusFieldSpecified;
            }
            set
            {
                this.radiusFieldSpecified = value;
            }
        }
    }

    /// <remarks/>
    [System.CodeDom.Compiler.GeneratedCodeAttribute("System.Xml", "4.0.30319.17330")]
    [System.SerializableAttribute()]
    [System.Xml.Serialization.XmlTypeAttribute(Namespace="http://tempuri.org/ServiceSchema.xsd")]
    [System.Xml.Serialization.XmlRootAttribute(Namespace="http://tempuri.org/ServiceSchema.xsd", IsNullable=false)]
    public enum WebSearchOption
    {

        /// <remarks/>
        DisableHostCollapsing,

        /// <remarks/>
        DisableQueryAlterations,
    }
}
```

## <a name="errors-and-warnings"></a>错误和警告

 分析 XSD 架构时遇到的错误和警告将显示为生成错误和警告。

## <a name="interface-inheritance"></a>接口继承

 在协定优先开发中无法使用接口继承；这与接口在其他操作中的行为方式一致。 为了使用继承基接口的接口，应使用两个单独的终结点。 第一个终结点使用继承的协定，第二个终结点实现基接口。
