---
title: <EnableAmPmParseAdjustment> 元素
ms.date: 03/30/2017
ms.assetid: fda998a5-f538-4f8b-a18c-ee7f35e16938
ms.openlocfilehash: f935f213e1bca8dac7a5401970bc6183575e2301
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91167224"
---
# <a name="enableampmparseadjustment-element"></a>\<EnableAmPmParseAdjustment> 元素

确定日期和时间分析方法是否使用经过调整的规则集来分析包含 day、month、hour 和 AM/PM 指示符的日期字符串。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<EnableAmPmParseAdjustment>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<EnableAmPmParseAdjustment enabled="0"|"1" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`enabled`|必需的特性。<br /><br /> 指定日期和时间分析方法是否使用经过调整的规则集来分析只包含 day、month、hour 和 AM/PM 指示符的日期字符串。|  
  
### <a name="enabled-attribute"></a>enabled 特性  
  
|值|描述|  
|-----------|-----------------|  
|0|日期和时间分析方法不使用调整的规则来分析日期字符串，这些字符串只包含日、月、小时和 AM/PM 指示符。|  
|1|日期和时间分析方法使用经过调整的规则，用于分析日期字符串，这些字符串只包含日、月、小时和 AM/PM 指示符。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`runtime`|包含有关运行时初始化选项的信息。|  
  
## <a name="remarks"></a>备注  

 `<EnableAmPmParseAdjustment>`元素控制以下方法如何分析包含数字日和月后跟一个小时和 AM/PM 指示符的日期字符串， (例如 "4/10 6 AM" ) ：  
  
- <xref:System.DateTime.Parse%2A?displayProperty=nameWithType>  
  
- <xref:System.DateTimeOffset.Parse%2A?displayProperty=nameWithType>  
  
- <xref:System.DateTime.TryParse%2A?displayProperty=nameWithType>  
  
- <xref:System.DateTimeOffset.TryParse%2A?displayProperty=nameWithType>  
  
- <xref:System.Convert.ToDateTime%2A?displayProperty=nameWithType>  
  
 其他模式不受影响。  
  
 `<EnableAmPmParseAdjustment>`元素对 <xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType> 、 <xref:System.DateTime.TryParseExact%2A?displayProperty=nameWithType> 、 <xref:System.DateTimeOffset.ParseExact%2A?displayProperty=nameWithType> 和方法不起作用 <xref:System.DateTimeOffset.TryParseExact%2A?displayProperty=nameWithType> 。  
  
> [!IMPORTANT]
> 在 .NET Core 和 .NET Native 中，将默认启用调整后的 AM/PM 分析规则。  
  
 如果未启用解析调整规则，则会将字符串的第一个数字解释为12小时制的小时，而除 AM/PM 指示符之外的字符串的其余部分将被忽略。 解析方法返回的日期和时间由当前日期和从日期字符串提取的日期中的小时组成。  
  
 如果启用了解析调整规则，则分析方法会将日期和月份解释为属于当前年份，并将时间解释为12小时制的小时。  
  
 下表说明了 <xref:System.DateTime> 当 <xref:System.DateTime.Parse%28System.String%29?displayProperty=nameWithType> 使用方法分析字符串 "4/10 6 AM" （ `<EnableAmPmParseAdjustment>` 元素的 `enabled` 属性设置为 "0" 或 "1"）时，值中的差异。 它假定今天的日期为2017年1月5日，并使用指定的区域性的 "G" 格式字符串来显示日期。  
  
|区域性名称|enabled = "0"|enabled = "1"|  
|------------------|------------------|------------------|  
|zh-CN|上午 1/5/2017 4:00:00|上午 4/10/2017 6:00:00|  
|en-GB|5/1/2017 6:00:00|10/4/2017 6:00:00|  
  
## <a name="see-also"></a>请参阅

- [\<runtime> 元素](runtime-element.md)
- [\<configuration> 元素](../configuration-element.md)
