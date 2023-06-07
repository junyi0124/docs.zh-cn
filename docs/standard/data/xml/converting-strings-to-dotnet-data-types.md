---
title: 将字符串转换为 .NET 数据类型
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 65455ef3-9120-412c-819b-d0f59f88ac09
ms.openlocfilehash: 0cee7481f9c002f860bff7f12b8be0bb763dadb1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95701466"
---
# <a name="convert-strings-to-net-data-types"></a>将字符串转换为 .NET 数据类型

若要将字符串转换为 .NET 数据类型，请使用满足应用程序要求的 XmlConvert 方法。 有关 XmlConvert  类提供的所有转换方法的列表，请参阅 <xref:System.Xml.XmlConvert>。  
  
 从 ToString  方法返回的字符串是传入数据的字符串版本。 此外，还有若干 .NET 类型仍使用 XmlConvert 类进行转换，但它们不使用 System.Convert 类中的方法 。 XmlConvert  类遵循 XML 架构 (XSD) 数据类型规范，并有 XMLConvert  可以映射到的数据类型。  
  
 下表列出了 .NET 数据类型和使用 XML 架构 (XSD) 数据类型映射返回的字符串类型。 不能使用 System.Convert 处理这些 .NET 类型。  
  
|.NET 类型|返回的字符串|  
|-------------------------|---------------------|  
|Boolean|“true”、“false”|  
|Single.PositiveInfinity|“INF”|  
|Single.NegativeInfinity|“-INF”|  
|Double.PositiveInfinity|“INF”|  
|Double.NegativeInfinity|“-INF”|  
|DateTime|格式为“yyyy-MM-ddTHH:mm:sszzzzzz”及其子集。|  
|Timespan|格式是 PnYnMnTnHnMnS，例如 `P2Y10M15DT10H30M20S` 表示长 2 年 10 个月 15 天 10 小时 30 分钟 20 秒的持续时间。|  
  
> [!NOTE]
> 如果使用 ToString 方法将表中所列的任何 .NET 类型转换为字符串，返回的字符串不是基类型，而是 XML 架构 (XSD) 字符串类型。  
  
 DateTime  和 Timespan  值类型的区别在于，DateTime  表示时间上的某个时刻，而 TimeSpan  则表示时间间隔。 XML 架构 (XSD) 数据类型规范中指定了 DateTime  和 Timespan  格式。 例如：  
  
```vb  
Dim writer As New XmlTextWriter("myfile.xml", Nothing)  
Dim [date] As New DateTime(2001, 8, 4)  
writer.WriteElementString("Date", XmlConvert.ToString([date]))  
```  
  
```csharp  
XmlTextWriter writer = new XmlTextWriter("myfile.xml", null);  
DateTime date = new DateTime (2001, 08, 04);  
writer.WriteElementString("Date", XmlConvert.ToString(date));  
```  
  
 **输出**  
  
 `<Date>2001-08-04T00:00:00</Date>`。  
  
 下面的代码将整数转换为字符串：  
  
```vb  
Dim writer As New XmlTextWriter("myfile.xml", Nothing)  
Dim value As Int32 = 200  
writer.WriteElementString("Number", XmlConvert.ToString(value))  
```  
  
```csharp  
XmlTextWriter writer = new XmlTextWriter("myfile.xml", null);  
Int32 value = 200;  
writer.WriteElementString("Number", XmlConvert.ToString(value));  
```  
  
 **输出**  
  
 `<Number>200</Number>`  
  
 不过，如果将字符串转换为 Boolean、Single 或 Double，返回的 .NET 类型与使用 System.Convert 类返回的类型不同   。  
  
## <a name="string-to-boolean"></a>将字符串转换为 Boolean  

 下表列出了使用 ToBoolean  方法将字符串转换为 Boolean  时，针对给定输入字符串生成的类型。  
  
|有效的字符串输入参数|.NET 输出类型|  
|----------------------------------|--------------------------------|  
|“true”|Boolean.True|  
|"1"|Boolean.True|  
|“false”|Boolean.False|  
|“0”|Boolean.False|  
  
 例如，给定以下 XML：  
  
 **输入**  
  
```xml  
<Boolean>true</Boolean>  
<Boolean>1</Boolean>
```  
  
 两者均可通过下列代码理解，其中 bvalue  为 System.Boolean.True  ：  
  
```vb  
Dim bvalue As Boolean = _  
   XmlConvert.ToBoolean(reader.ReadElementString())  
Console.WriteLine(bvalue)  
```  
  
```csharp  
Boolean bvalue = XmlConvert.ToBoolean(reader.ReadElementString());  
Console.WriteLine(bvalue);  
```  
  
## <a name="string-to-single"></a>将字符串转换为 Single  

 下表列出了使用 ToSingle  方法将字符串转换为 Single  时，针对给定输入字符串生成的类型。  
  
|有效的字符串输入参数|.NET 输出类型|  
|----------------------------------|--------------------------------|  
|“INF”|Single.PositiveInfinity|  
|“-INF”|Single.NegativeInfinity|  
  
## <a name="string-to-double"></a>将字符串转换为 Double  

 下表列出了使用 ToDouble  方法将字符串转换为 Single  时，针对给定输入字符串生成的类型。  
  
|有效的字符串输入参数|.NET 输出类型|  
|----------------------------------|--------------------------------|  
|“INF”|Double.PositiveInfinity|  
|“-INF”|Double.NegativeInfinity|  
  
 下面的代码写出 `<Infinity>INF</Infinity>`：  
  
```vb  
Dim value As Double = Double.PositiveInfinity  
writer.WriteElementString("Infinity", XmlConvert.ToString(value))  
```  
  
```csharp  
Double value = Double.PositiveInfinity;  
writer.WriteElementString("Infinity", XmlConvert.ToString(value));  
```  
  
## <a name="see-also"></a>请参阅

- [XML 数据类型转换](conversion-of-xml-data-types.md)
- [将 .NET 类型转换为字符串](converting-dotnet-types-to-strings.md)
