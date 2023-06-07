---
title: 版本容错序列化
description: 了解版本容错序列化，这组功能使修改可序列化类型更加容易。
ms.date: 08/08/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- version tolerant serialization
- serialization, custom serialization
- serialization, version tolerant
- serialization, controlling
- versions and serialization
- BinaryFormatter class, samples
- serialization, attributes
ms.assetid: bea0ffe3-2708-4a16-ac7d-e586ed6b8e8d
ms.openlocfilehash: 26612c5b0591efa61fcd476733aee2b219d67c62
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96438160"
---
# <a name="version-tolerant-serialization"></a>版本容错序列化

在最早版本的 .NET Framework 中，创建可从应用程序的一个版本重用至下一个版本的可序列化类型时会产生问题。 如果通过添加额外字段来修改类型，则会出现以下问题：

- 要求应用程序的较旧版本反序列化旧类型的新版本时，会引发异常。
- 应用程序的较新版本反序列化缺少数据的类型的较旧版本时，会引发异常。

版本容错序列化 (VTS) 是一组功能，它使得修改可序列化类型随着时间推移而变得更加容易。 VTS 功能尤其是为应用了 <xref:System.SerializableAttribute> 特性的类（包括泛型类型）而启用的。 VTS 允许向这些类添加新字段，而不破坏与该类型其他版本的兼容性。

当使用 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 时，将启用 VTS 功能。 此外，当使用 <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> 时，也会启用除外来数据容错以外的其他所有功能。 有关将这些类用于序列化的详细信息，请参见[二进制序列化](binary-serialization.md)。

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]

## <a name="feature-list"></a>功能列表

功能集包括：

- 外来或意外数据容错功能。 该功能允许类型的较新版本将数据发送至较旧版本。
- 缺少可选数据容错功能。 该功能允许较旧版本将数据发送至较新版本。
- 序列化回调。 该功能在缺少数据的情况下启用智能默认值设置。

此外，添加了新的可选字段时还有声明功能。 这是 <xref:System.Runtime.Serialization.OptionalFieldAttribute.VersionAdded%2A> 特性的 <xref:System.Runtime.Serialization.OptionalFieldAttribute> 属性。

以下部分详细描述了这些功能。

### <a name="tolerance-of-extraneous-or-unexpected-data"></a>外来或意外数据容错功能

过去在反序列化时，任何外来或意外数据都会导致引发异常。 有了 VTS 后，在相同情况下会忽略所有外来或意外数据，而不会导致引发异常。 这就使得使用类型的较新版本（即包含更多字段的版本）的应用程序可以将信息发送至需要使用同一类型的较旧版本的应用程序。

在下面的示例中，当较旧版本的应用程序反序列化较新版本时，将忽略 `CountryField` 类 2.0 版的 `Address` 中包含的额外数据。

```csharp  
// Version 1 of the Address class.  
[Serializable]  
public class Address  
{  
    public string Street;  
    public string City;  
}  
// Version 2.0 of the Address class.  
[Serializable]  
public class Address  
{  
    public string Street;  
    public string City;  
    // The older application ignores this data.  
    public string CountryField;  
}  
```  
  
```vb  
' Version 1 of the Address class.  
<Serializable> _  
Public Class Address  
    Public Street As String  
    Public City As String  
End Class  
  
' Version 2.0 of the Address class.  
<Serializable> _  
Public Class Address  
    Public Street As String  
    Public City As String  
    ' The older application ignores this data.  
    Public CountryField As String  
End Class  
```  

### <a name="tolerance-of-missing-data"></a>丢失数据容错功能

通过将 <xref:System.Runtime.Serialization.OptionalFieldAttribute> 特性应用于字段，可以将字段标记为可选。 在反序列化期间，如果缺少可选的数据，序列化引擎会忽略这一缺失且不会引发异常。 因此，需要使用类型的较旧版本的应用程序可以将数据发送至需要使用同一类型的较新版本的应用程序。

下面的示例显示的是 `Address` 类的 2.0 版，其 `CountryField` 字段标记为可选。 如果较旧版本的应用程序将版本 1 发送至需要使用 2.0 版的较新版本的应用程序，则会忽略数据缺失。

```csharp
[Serializable]
public class Address
{
    public string Street;
    public string City;

    [OptionalField]
    public string CountryField;
}
```

```vb
<Serializable> _
Public Class Address
    Public Street As String
    Public City As String

    <OptionalField> _
    Public CountryField As String
End Class
```
  
### <a name="serialization-callbacks"></a>序列化回调

序列化回调是一种机制，它在序列化/反序列化过程中的四个点提供挂钩。

|特性|调用关联的方法时|典型用法|
|---------------|------------------------------------------|-----------------|
|<xref:System.Runtime.Serialization.OnDeserializingAttribute>|反序列化之前。\*|初始化可选字段的默认值。|
|<xref:System.Runtime.Serialization.OnDeserializedAttribute>|反序列化之后。|根据其他字段的内容修改可选字段值。|
|<xref:System.Runtime.Serialization.OnSerializingAttribute>|序列化之前。|准备序列化。 例如，创建可选数据结构。|
|<xref:System.Runtime.Serialization.OnSerializedAttribute>|序列化之后。|记录序列化事件。|

 \* 如果有反序列化构造函数，则在该构造函数之前调用此回调。

#### <a name="using-callbacks"></a>使用回调

要使用回调，请将相应的特性应用于接受 <xref:System.Runtime.Serialization.StreamingContext> 参数的方法。 每个类只有一个方法可以用其中每个特性进行标记。 例如：

```csharp
[OnDeserializing]
private void SetCountryRegionDefault(StreamingContext sc)
{
    CountryField = "Japan";
}
```

```vb
<OnDeserializing>
Private Sub SetCountryRegionDefault(sc As StreamingContext)
    CountryField = "Japan"
End Sub
```

这些方法旨在用于版本管理。 在反序列化期间，如果可选字段缺少数据，则可能无法正确初始化该字段。 若要更正这一情况，可以创建分配正确值的方法，然后将 OnDeserializingAttribute 或 OnDeserializedAttribute 特性应用于该方法。 

下面的示例演示类型上下文中的方法。 如果应用程序的较旧版本将 `Address` 类的实例发送至该应用程序的较新版本，将会丢失 `CountryField` 字段数据。 但是反序列化之后，会将字段设置为默认值“Japan”。

```csharp
[Serializable]
public class Address
{
    public string Street;
    public string City;
    [OptionalField]
    public string CountryField;

    [OnDeserializing]
    private void SetCountryRegionDefault(StreamingContext sc)
    {
        CountryField = "Japan";
    }
}
```

```vb
<Serializable> _
Public Class Address
    Public Street As String
    Public City As String
    <OptionalField> _
    Public CountryField As String

    <OnDeserializing> _
    Private Sub SetCountryRegionDefault(sc As StreamingContext)
        CountryField = "Japan"
    End Sub
End Class
```

## <a name="the-versionadded-property"></a>VersionAdded 属性

OptionalFieldAttribute 具有 VersionAdded 属性。  该属性指示向给定字段添加了类型的哪个版本。 每次修改类型时，版本应该正好增加一（从 2 开始），如下例所示：

```csharp
// Version 1.0
[Serializable]
public class Person
{
    public string FullName;
}

// Version 2.0
[Serializable]
public class Person
{
    public string FullName;

    [OptionalField(VersionAdded = 2)]
    public string NickName;
    [OptionalField(VersionAdded = 2)]
    public DateTime BirthDate;
}

// Version 3.0
[Serializable]
public class Person
{
    public string FullName;

    [OptionalField(VersionAdded=2)]
    public string NickName;
    [OptionalField(VersionAdded=2)]
    public DateTime BirthDate;

    [OptionalField(VersionAdded=3)]
    public int Weight;
}
```

```vb
' Version 1.0
<Serializable> _
Public Class Person
    Public FullName
End Class

' Version 2.0
<Serializable> _
Public Class Person
    Public FullName As String

    <OptionalField(VersionAdded := 2)> _
    Public NickName As String
    <OptionalField(VersionAdded := 2)> _
    Public BirthDate As DateTime
End Class

' Version 3.0
<Serializable> _
Public Class Person
    Public FullName As String

    <OptionalField(VersionAdded := 2)> _
    Public NickName As String
    <OptionalField(VersionAdded := 2)> _
    Public BirthDate As DateTime

    <OptionalField(VersionAdded := 3)> _
    Public Weight As Integer
End Class
```

## <a name="serializationbinder"></a>SerializationBinder

由于服务器和客户端要求使用不同的类版本，因此，有些用户可能需要控制要序列化和反序列化哪些类。 <xref:System.Runtime.Serialization.SerializationBinder> 是抽象类，用于控制在序列化和反序列化期间使用的实际类型。 若要使用此类，请从 <xref:System.Runtime.Serialization.SerializationBinder> 派生类，并重写 <xref:System.Runtime.Serialization.SerializationBinder.BindToName%2A> 和 <xref:System.Runtime.Serialization.SerializationBinder.BindToType%2A> 方法。 有关详细信息，请参阅[使用 SerializationBinder 控制序列化和反序列化](../../framework/wcf/feature-details/controlling-serialization-and-deserialization-with-serializationbinder.md)。

## <a name="best-practices"></a>最佳实践

要确保版本管理行为正确，修改类型版本时请遵循以下规则：

- 切勿移除已序列化的字段。
- 如果未在以前版本中将 <xref:System.NonSerializedAttribute> 特性应用于某个字段，则切勿将该特性应用于该字段。
- 切勿更改已序列化字段的名称或类型。
- 添加新的已序列化字段时，请应用 OptionalFieldAttribute 特性。
- 从字段（在以前版本中不可序列化）中移除 NonSerializedAttribute 特性时，请应用 OptionalFieldAttribute 特性。 
- 对于所有可选字段，除非可接受 0 或 null 作为默认值，否则请使用序列化回调设置有意义的默认值。

要确保类型与将来的序列化引擎兼容，请遵循以下准则：

- 始终正确设置 OptionalFieldAttribute 特性上的 VersionAdded 属性。 
- 避免版本管理分支。

## <a name="see-also"></a>请参阅

- <xref:System.SerializableAttribute>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>
- <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>
- <xref:System.Runtime.Serialization.OptionalFieldAttribute.VersionAdded%2A>
- <xref:System.Runtime.Serialization.OptionalFieldAttribute>
- <xref:System.Runtime.Serialization.OnDeserializingAttribute>
- <xref:System.Runtime.Serialization.OnDeserializedAttribute>
- <xref:System.Runtime.Serialization.OnSerializingAttribute>
- <xref:System.Runtime.Serialization.OnSerializedAttribute>
- <xref:System.Runtime.Serialization.StreamingContext>
- <xref:System.NonSerializedAttribute>
- [二进制序列化](binary-serialization.md)
