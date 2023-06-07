---
title: 自定义序列化
description: 自定义序列化控制着类型的序列化和反序列化。 控制序列化可确保序列化兼容性。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- binary serialization, custom serialization
- custom serialization
- binary serialization, controlling
- OptionalFieldAttribute class, custom serialization
- ISerializable interface, custom serialization
- OnDeserializingAttribute class, custom serialization
- OnSerializedAttribute class, custom serialization
- serialization, custom serialization
- serialization, controlling
- OnDeserializedAttribute class, custom serialization
- OnSerializingAttribute class, custom serialization
ms.assetid: 12ed422d-5280-49b8-9b71-a2ed129c0384
ms.openlocfilehash: 4ca78c71f464a914c07583825d4a7027ebb11bf6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679034"
---
# <a name="custom-serialization"></a>自定义序列化

自定义序列化是控制类型的序列化和反序列化的过程。 通过控制序列化，可以确保序列化兼容性。换而言之，在不中断类型核心功能的情况下，可在类型的不同版本之间序列化和反序列化。 例如，在类型的第一个版本中，可能只有两个字段。 在类型的下一个版本中，添加了其他几个字段。 但是，第二个版本的应用程序必须可对这两种类型进行序列化和反序列化。 以下各节说明如何控制序列化。

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]
  
> [!IMPORTANT]
> 在早于 .NET Framework 4.0 的版本中，部分受信任的程序集中自定义用户数据的序列化是使用 `GetObjectData` 完成的。 从版本 4.0 开始，该方法将标记有 <xref:System.Security.SecurityCriticalAttribute> 特性，该特性阻止在部分受信任的程序集中执行。 若要解决此情况，请实现 <xref:System.Runtime.Serialization.ISafeSerializationData> 接口。  
  
## <a name="running-custom-methods-during-and-after-serialization"></a>在序列化期间和序列化之后运行自定义方法

序列化期间和序列化之后运行自定义方法的建议方法，是在序列化期间和序列化之后，将下列属性应用于更正数据所用的方法：  
  
- <xref:System.Runtime.Serialization.OnDeserializedAttribute>  
  
- <xref:System.Runtime.Serialization.OnDeserializingAttribute>  
  
- <xref:System.Runtime.Serialization.OnSerializedAttribute>  
  
- <xref:System.Runtime.Serialization.OnSerializingAttribute>  
  
 这些属性允许类型参与序列化和反序列化过程中的任何一个阶段或所有四个阶段。 这些特性为类型指定了应该在每个阶段调用的方法。 这些方法不会访问序列化流，但是反而允许您在序列化或反序列化前后更改对象。 可以在类型继承层次结构中的所有级别上应用这些特性，而每种方法在该层次结构中是按从基类型到派生程度最大的类型的顺序调用的。 这种机制使得序列化和反序列化可以负责到派生程度最大的实现，从而可以避免实现 <xref:System.Runtime.Serialization.ISerializable> 接口时的复杂性和任何导致的问题。 此外，这种机制允许格式化程序忽略字段的填充以及从序列化流中检索。 有关控制序列化和反序列化的详细信息和示例，请单击以上任一链接。  
  
 另外，向现有的可序列化类型中添加新字段时，可将 <xref:System.Runtime.Serialization.OptionalFieldAttribute> 特性应用于该字段。 处理缺少新字段的流时，<xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 和 <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> 可以忽略不存在该字段的情况。  
  
## <a name="implementing-the-iserializable-interface"></a>实现 ISerializable 接口  

 控制序列化的另一种方法是对某个对象实现 <xref:System.Runtime.Serialization.ISerializable> 接口。 但请注意，上一节采用的方法会取代这种方法对序列化进行控制。  
  
 除此之外，如果使用 [Serializable](xref:System.SerializableAttribute) 属性对某个类进行标记，且该类在类级别或对其构造函数具有声明性或命令性安全，则不应对该类执行默认的序列化。 相反，这样的类应该始终实现 <xref:System.Runtime.Serialization.ISerializable> 接口。  
  
 实现 <xref:System.Runtime.Serialization.ISerializable> 涉及到实现 `GetObjectData` 方法以及反序列化对象时所用的特殊构造函数。 下面的示例代码演示如何对上一节中的 <xref:System.Runtime.Serialization.ISerializable> 类实现 `MyObject`。  
  
```csharp
[Serializable]
public class MyObject : ISerializable
{
    public int n1;
    public int n2;
    public String str;

    public MyObject()
    {
    }

    protected MyObject(SerializationInfo info, StreamingContext context)
    {
      n1 = info.GetInt32("i");
      n2 = info.GetInt32("j");
      str = info.GetString("k");
    }

    [SecurityPermissionAttribute(SecurityAction.Demand, SerializationFormatter = true)]
    public virtual void GetObjectData(SerializationInfo info, StreamingContext context)
    {
        info.AddValue("i", n1);
        info.AddValue("j", n2);
        info.AddValue("k", str);
    }
}
```

```vb
<Serializable()>  _
Public Class MyObject
    Implements ISerializable
    Public n1 As Integer
    Public n2 As Integer
    Public str As String

    Public Sub New()
    End Sub

    Protected Sub New(ByVal info As SerializationInfo, ByVal context As StreamingContext)
        n1 = info.GetInt32("i")
        n2 = info.GetInt32("j")
        str = info.GetString("k")
    End Sub 'New

    <SecurityPermissionAttribute(SecurityAction.Demand, SerializationFormatter := True)> _
    Public Overridable Sub GetObjectData(ByVal info As SerializationInfo, ByVal context As StreamingContext)
        info.AddValue("i", n1)
        info.AddValue("j", n2)
        info.AddValue("k", str)
    End Sub
End Class
```  
  
 序列化期间调用 GetObjectData 时，由用户负责填充随方法调用一起提供的 <xref:System.Runtime.Serialization.SerializationInfo>。 将待序列化变量以名称和值对的形式添加。 任何文本都能用作名称。 如果对足够多的数据进行序列化，以在反序列化期间还原对象，您便可随意决定将哪些成员变量添加到 <xref:System.Runtime.Serialization.SerializationInfo>。 如果基对象实现 <xref:System.Runtime.Serialization.ISerializable>，则派生类应该对该基对象调用 GetObjectData 方法。  
  
 请注意，序列化可以允许其他代码查看或修改用其他方式无法访问的对象实例数据。 因此，执行序列化的代码需使用指定了 <xref:System.Security.Permissions.SecurityPermissionAttribute.SerializationFormatter> 标志的 [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute)。 在默认策略下，通过 Internet 下载的代码或 Intranet 代码不会授予该权限；只有本地计算机上的代码才被授予该权限。 必须采用下列方式显式保护 GetObjectData 方法：要求使用使用指定了 <xref:System.Security.Permissions.SecurityPermissionAttribute.SerializationFormatter> 标志的 [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute)，或者要求具备专门用于帮助保护私有数据的其他权限。  
  
 若私有字段存储的是敏感信息，应该要求对 GetObjectData 具备相应权限，以便保护该数据。 请记住，如果已向代码授予指定了 SerializationFormatter 标志的 [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute)，则该代码可查看和修改私有字段中存储的数据。 被授予此 [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute) 的恶意调用方可以查看相关数据（如隐藏的目录位置或授予的权限），还可以通过这些数据利用计算机中的安全漏洞。 有关可以指定的安全权限标志的完整列表，请参阅 [SecurityPermissionFlag 枚举](xref:System.Security.Permissions.SecurityPermissionFlag)。  
  
 强调何时将 <xref:System.Runtime.Serialization.ISerializable> 添加到必须同时实现 **GetObjectData** 和特殊构造函数的某个类，这一点很重要。 如果缺少 GetObjectData，编译器会发出警告。 但是，鉴于无法强制实现构造函数，如果不存在该构造函数，则不会发出任何警告，但此时如果尝试对某个类进行反序列化，将会引发异常。  
  
 若要解决潜在的安全和版本化问题，当前的设计应该优先于 <xref:System.Runtime.Serialization.ISerializationSurrogate.SetObjectData%2A> 方法。 例如，如果将 `SetObjectData` 方法定义为接口的组成部分，则该方法必须是公共方法。因此，用户必须编写代码，以防多次调用 SetObjectData 方法。 否则，在执行某项操作的过程中，对某个对象调用 SetObjectData 方法的恶意应用程序可能会导致潜在的问题。  
  
 在反序列化期间，使用为这个目的提供的构造函数将 <xref:System.Runtime.Serialization.SerializationInfo> 传递到类。 反序列化该对象时，将会忽略对该构造函数施加的任何可见性限制。因此，可将该类标记为公共类、受保护的类、内部类或私有类。 但是，除非密封了该类，否则最好将构造函数设为受保护的函数；如果密封了该类，应将构造函数标记为私有函数。 构造函数还应该执行彻底的输入验证。 为避免被恶意代码误用，构造函数应该强制实施安全性检查和权限，而这些安全性检查和权限也是使用其他任何构造函数获取该类的实例所必需的。 如果不采纳上述建议，恶意代码会跳过使用公共构造函数在标准实例构造期间应用的所有安全性，预序列化对象，获取对使用指定了 <xref:System.Security.Permissions.SecurityPermissionAttribute.SerializationFormatter> 标志的 [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute) 的控制，并反序列化客户端计算机上的对象。  
  
 若要还原对象的状态，只需使用序列化期间采用的名称从 <xref:System.Runtime.Serialization.SerializationInfo> 中检索变量值即可。 如果基类实现 <xref:System.Runtime.Serialization.ISerializable>，则应调用基构造函数，以使基对象可以还原其变量。  
  
 从实现 <xref:System.Runtime.Serialization.ISerializable> 的类派生新类时，如果派生类的变量需要进行序列化，则该派生类必须同时实现构造函数和 GetObjectData 方法。 下面的代码示例演示如何使用前面说明的 `MyObject` 类完成此项操作。  
  
```csharp
[Serializable]
public class ObjectTwo : MyObject
{
    public int num;

    public ObjectTwo()
      : base()
    {
    }

    protected ObjectTwo(SerializationInfo si, StreamingContext context)
      : base(si, context)
    {
        num = si.GetInt32("num");
    }

    [SecurityPermissionAttribute(SecurityAction.Demand, SerializationFormatter = true)]
    public override void GetObjectData(SerializationInfo si, StreamingContext context)
    {
        base.GetObjectData(si,context);
        si.AddValue("num", num);
    }
}
```

```vb
<Serializable()>  _
Public Class ObjectTwo
    Inherits MyObject
    Public num As Integer

    Public Sub New()

    End Sub

    Protected Sub New(ByVal si As SerializationInfo, _
    ByVal context As StreamingContext)
        MyBase.New(si, context)
        num = si.GetInt32("num")
    End Sub

    <SecurityPermissionAttribute(SecurityAction.Demand, SerializationFormatter := True)> _
    Public Overrides Sub GetObjectData(ByVal si As SerializationInfo, ByVal context As StreamingContext)
        MyBase.GetObjectData(si, context)
        si.AddValue("num", num)
    End Sub
End Class
```  
  
 不要忘记在反序列化构造函数中调用基类。 如果没有完成此操作，决不会调用基类上的构造函数，也不会在反序列化之后完全构造该对象。  
  
 对象是从内到外重新构造的；在反序列化期间调用方法时，可能会产生非预期的副作用，原因是调用的方法引用的可能是执行调用时尚未反序列化的对象引用。 如果正在被反序列化的类实现 <xref:System.Runtime.Serialization.IDeserializationCallback>，则反序列化整个对象图后，将会自动调用 <xref:System.Runtime.Serialization.IDeserializationCallback.OnDeserialization%2A> 方法。 此时，便已完全还原引用的所有子对象。 哈希表是类的典型示例，在不使用事件侦听器的情况下，很难对哈希表进行反序列化。 虽然在反序列化期间易于检索键和值对，但是，将这些对象重新添加到哈希表时，可能会引起问题，原因是不能保证派生自哈希表的类已被反序列化。 因此，不建议在这个阶段对哈希表调用方法。  
  
## <a name="see-also"></a>请参阅

- [二进制序列化](binary-serialization.md)
- [XML 和 SOAP 序列化](xml-and-soap-serialization.md)
- [安全性和序列化](../../framework/misc/security-and-serialization.md)
