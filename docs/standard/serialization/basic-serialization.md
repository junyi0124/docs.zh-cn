---
title: 基本序列化
description: 本文说明如何使用 SerializableAttribute 使类可序列化，并包括序列化和反序列化的示例。
ms.date: 03/30/2017
helpviewer_keywords:
- binary serialization, basic serialization
- serialization, basic serialization
ms.assetid: d899d43c-335a-433e-a589-cd187192984f
dev_langs:
- CSharp
ms.openlocfilehash: 2b75ba6875b2a4430b6776c27dead72476884fff
ms.sourcegitcommit: 74d05613d6c57106f83f82ce8ee71176874ea3f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93282074"
---
# <a name="basic-serialization"></a>基本序列化

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]

使类可序列化最简单的方法是按以下方式使用 <xref:System.SerializableAttribute> 对其进行标记。  
  
```csharp  
[Serializable]  
public class MyObject {  
  public int n1 = 0;  
  public int n2 = 0;  
  public String str = null;  
}  
```  
  
下面的代码示例演示如何将此类的实例序列化为文件。  
  
```csharp  
MyObject obj = new MyObject();  
obj.n1 = 1;  
obj.n2 = 24;  
obj.str = "Some String";  
IFormatter formatter = new BinaryFormatter();  
Stream stream = new FileStream("MyFile.bin", FileMode.Create, FileAccess.Write, FileShare.None);  
formatter.Serialize(stream, obj);  
stream.Close();  
```  
  
此示例使用二进制格式化程序执行序列化。 只需创建该流的实例及要使用的格式化程序，然后在该格式化程序上调用 Serialize 方法。 要序列化的流和对象将作为参数提供给此调用。 尽管未在此示例中明确演示，但类的所有成员变量都将被序列化，即使将变量标记为私有也是如此。 在这一方面，二进制序列化与 <xref:System.Xml.Serialization.XmlSerializer> 类不同，后者只序列化公共字段。 有关从二进制序列化排除成员变量的信息，请参阅[选择性的序列化](selective-serialization.md)。  
  
将对象还原回以前的状态同样很容易。 首先，创建用于读取的流和 <xref:System.Runtime.Serialization.Formatter>，然后指导格式化程序对该对象进行反序列化。 下面的代码示例演示如何完成以上过程。  
  
```csharp  
IFormatter formatter = new BinaryFormatter();  
Stream stream = new FileStream("MyFile.bin", FileMode.Open, FileAccess.Read, FileShare.Read);  
MyObject obj = (MyObject) formatter.Deserialize(stream);  
stream.Close();  
  
// Here's the proof.  
Console.WriteLine("n1: {0}", obj.n1);  
Console.WriteLine("n2: {0}", obj.n2);  
Console.WriteLine("str: {0}", obj.str);  
```  
  
上面使用的 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 非常有效，还可生成压缩字节流。 使用此格式化程序序列化的所有对象也可使用它进行反序列化，这使得它成为对将在 .NET 上反序列化的对象进行序列化的理想工具。 需要特别注意的是，反序列化对象时不调用构造函数。 这是出于性能原因而对反序列化进行的约束。 然而，这违反了运行库对对象编写器制定的某些常用协定，并且将对象标记为可序列化时开发人员应确保了解其后果。  
  
如果要求可迁移性，请改用 <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>。 只需将以上代码中的 BinaryFormatter 替换为 SoapFormatter，然后同前面一样调用 Serialize 和 Deserialize   。 此格式化程序针对以上使用的示例产生下面的输出：  
  
```xml  
<SOAP-ENV:Envelope  
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"  
  xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"  
  SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"  
  xmlns:a1="http://schemas.microsoft.com/clr/assem/ToFile">  
  
  <SOAP-ENV:Body>  
    <a1:MyObject id="ref-1">  
      <n1>1</n1>  
      <n2>24</n2>  
      <str id="ref-3">Some String</str>  
    </a1:MyObject>  
  </SOAP-ENV:Body>  
</SOAP-ENV:Envelope>  
```  
  
请注意，无法继承 [Serializable](xref:System.SerializableAttribute) 属性。 如果从 `MyObject` 派生新类，新类也必须标记为以上特性，否则将无法序列化。 例如，尝试序列化以下类的实例时，将收到 <xref:System.Runtime.Serialization.SerializationException> 信息，提示 `MyStuff` 类型未标记为可序列化。  
  
```csharp  
public class MyStuff : MyObject
{  
  public int n3;  
}  
```  
  
 使用 [Serializable](xref:System.SerializableAttribute) 属性非常方便，但有以上所述的限制。 有关何时应标记类以进行序列化的信息，请参阅[序列化准则](serialization-guidelines.md)。 若类已编译，则无法对其进行序列化。  
  
## <a name="see-also"></a>请参阅

- [二进制序列化](binary-serialization.md)
- [XML 和 SOAP 序列化](xml-and-soap-serialization.md)
