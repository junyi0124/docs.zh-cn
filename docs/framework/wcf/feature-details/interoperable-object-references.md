---
title: 可互操作的对象引用
ms.date: 04/15/2019
ms.assetid: cb8da4c8-08ca-4220-a16b-e04c8f527f1b
ms.openlocfilehash: bf395c187c46e88406bfb81798c7e359b48255e3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263225"
---
# <a name="interoperable-object-references"></a>可互操作的对象引用

默认情况下， <xref:System.Runtime.Serialization.DataContractSerializer> 按值对对象进行序列化。 您可以使用 <xref:System.Runtime.Serialization.DataContractAttribute.IsReference%2A> 属性指示数据协定序列化程序在序列化对象时保留对象引用。  
  
## <a name="generated-xml"></a>生成的 XML  

 例如，请考虑下面的对象：  
  
```csharp  
[DataContract]  
public class X  
{  
    SomeClass someInstance = new SomeClass();  
    [DataMember]  
    public SomeClass A = someInstance;  
    [DataMember]  
    public SomeClass B = someInstance;  
}  
  
public class SomeClass
{  
}  
```  
  
 当 <xref:System.Runtime.Serialization.DataContractSerializer.PreserveObjectReferences%2A> 设置为 `false`（默认值）时，会生成下面的 XML：  
  
```xml  
<X>  
   <A>contents of someInstance</A>  
   <B>contents of someInstance</B>  
</X>  
```  
  
 当 <xref:System.Runtime.Serialization.DataContractSerializer.PreserveObjectReferences%2A> 设置为 `true` 时，会生成下面的 XML：  
  
```xml  
<X>  
   <A id="1">contents of someInstance</A>  
   <B ref="1"></B>  
</X>  
```  
  
 但是， <xref:System.Runtime.Serialization.XsdDataContractExporter> `id` `ref` 即使属性设置为，也不会在架构中描述和特性 `preserveObjectReferences` `true` 。  
  
## <a name="using-isreference"></a>使用 IsReference  

 若要生成根据描述的架构有效的对象引用信息，请将 <xref:System.Runtime.Serialization.DataContractAttribute> 属性应用到类型，并将 <xref:System.Runtime.Serialization.DataContractAttribute.IsReference%2A> 标志设置为 `true` 。 下面的示例通过添加以下示例修改 `X` 了上一示例中的类 `IsReference` ：  
  
```csharp
[DataContract(IsReference=true)]
public class X
{  
     SomeClass someInstance = new SomeClass();
     [DataMember]
     public SomeClass A = someInstance;
     [DataMember]
     public SomeClass B = someInstance;
}
  
public class SomeClass
{
}  
````

 生成的 XML 如下：  

```xml
<X>  
    <A id="1">
        <Value>contents of A</Value>  
    </A>
    <B ref="1"></B>  
</X>
```  
  
 使用 `IsReference` 可确保消息往返时遵从架构要求。 如果没有此方法，则在从架构生成类型时，该类型的 XML 输出不一定与最初假设的架构兼容。 换言之，虽然 `id` 和 `ref` 属性进行了序列化，但原始架构可能禁止这些属性（或所有属性）在 XML 中出现。 `IsReference`应用于数据成员后，在往返时，将继续将成员识别为 *可引用*。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.CollectionDataContractAttribute>
- <xref:System.Runtime.Serialization.DataContractAttribute.IsReference?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.CollectionDataContractAttribute.IsReference?displayProperty=nameWithType>
