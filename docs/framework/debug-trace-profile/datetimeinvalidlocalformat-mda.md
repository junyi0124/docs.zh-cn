---
title: dateTimeInvalidLocalFormat MDA
description: 查看 dateTimeInvalidLocalFormat 托管调试助手 (MDA) ，它在 UTC 存储日期时间值获取仅本地日期时间格式时被激活。
ms.date: 03/30/2017
helpviewer_keywords:
- dates [.NET Framework], formatting
- invalid date time local format
- invalid local formats
- MDAs (managed debugging assistants), invalid local formats
- managed debugging assistants (MDAs), invalid local formats
- dateTimeInvalidLocalFormat MDA
- date formatting
- time formatting
- UTC formatting
ms.assetid: c4a942bb-2651-4b65-8718-809f892a0659
ms.openlocfilehash: ed2cf0b960c0a8f51dc327a5c58770fcf5e2fa17
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286053"
---
# <a name="datetimeinvalidlocalformat-mda"></a>dateTimeInvalidLocalFormat MDA

使用只打算用于本地 <xref:System.DateTime> 实例的格式对存储为协调世界时 (UTC) 的 <xref:System.DateTime> 实例设置格式时，将激活 `dateTimeInvalidLocalFormat` MDA。 对于未指定的或默认的 <xref:System.DateTime> 实例，不激活此 MDA。  
  
## <a name="symptom"></a>症状  

 应用程序使用本地格式手动序列化 UTC <xref:System.DateTime> 实例：  
  
```csharp
DateTime myDateTime = DateTime.UtcNow;  
Serialize(myDateTime.ToString("yyyy-MM-dd'T'HH:mm:ss.fffffffzzz"));  
```  
  
### <a name="cause"></a>原因  

 <xref:System.DateTime.ToString%2A?displayProperty=nameWithType> 方法的“z”格式包括本地时区偏移量，例如，“+10:00”表示悉尼时间。 就这一点而言，只有 <xref:System.DateTime> 值是本地时间时，它才会得出有意义的结果。 如果该值是 UTC 时间，则 <xref:System.DateTime.ToString%2A?displayProperty=nameWithType> 包括本地时区偏移量，但是不显示或调整时区说明符。  
  
### <a name="resolution"></a>解决方法  

 应采用可表明 UTC <xref:System.DateTime> 实例为 UTC 时间的方式设置这些实例的格式。 对于 UTC 时间的格式，建议使用一个“Z”表示 UTC 时间：  
  
```csharp
DateTime myDateTime = DateTime.UtcNow;  
Serialize(myDateTime.ToString("yyyy-MM-dd'T'HH:mm:ss.fffffffZ"));  
```  
  
 还有一种利用正确序列化的 <xref:System.DateTime.Kind%2A> 属性序列化 <xref:System.DateTime> 的“o”格式，而不管实例是本地时间、UTC 时间还是未指定的时间：  
  
```csharp
DateTime myDateTime = DateTime.UtcNow;  
Serialize(myDateTime.ToString("o"));  
```  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 不影响运行时。  
  
## <a name="output"></a>输出  

 不存在作为此 MDA 激活的结果的特殊输出。但是，可使用调用堆栈确定激活此 MDA 的 <xref:System.DateTime.ToString%2A> 调用的位置。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <dateTimeInvalidLocalFormat />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>示例  

 假设应用程序按以下方式使用 <xref:System.Xml.XmlConvert> 或 <xref:System.Data.DataSet> 类间接序列化 UTC <xref:System.DateTime> 值。  
  
```csharp
DateTime myDateTime = DateTime.UtcNow;  
String serialized = XMLConvert.ToString(myDateTime);  
```  
  
 <xref:System.Xml.XmlConvert> 和 <xref:System.Data.DataSet> 序列化默认使用本地格式进行序列化。 序列化其他种类的 <xref:System.DateTime> 值（例如 UTC）需要其他选项。  
  
 对于此特定示例，将 `XmlDateTimeSerializationMode.RoundtripKind` 传递给 `XmlConvert` 上的 `ToString` 调用。 这会将数据序列化为 UTC 时间。  
  
 如果使用 <xref:System.Data.DataSet>，则将 <xref:System.Data.DataColumn> 对象上的 <xref:System.Data.DataColumn.DateTimeMode%2A> 属性设置为 <xref:System.Data.DataSetDateTime.Utc>。  
  
```csharp
DateTime myDateTime = DateTime.UtcNow;  
String serialized = XmlConvert.ToString(myDateTime,
    XmlDateTimeSerializationMode.RoundtripKind);  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Globalization.DateTimeFormatInfo>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
