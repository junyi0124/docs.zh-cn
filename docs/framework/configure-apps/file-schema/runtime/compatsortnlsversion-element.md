---
title: <CompatSortNLSVersion> 元素
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- <CompatSortNLSVersion> element
- CompatSortNLSVersion element
ms.assetid: 782cc82e-83f7-404a-80b7-6d3061a8b6e3
ms.openlocfilehash: 27d532633f08a5a560da61e904917c1faa35126c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91151358"
---
# <a name="compatsortnlsversion-element"></a>\<CompatSortNLSVersion> 元素

指定在执行字符串比较时，运行时应使用旧排序顺序。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<CompatSortNLSVersion>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<CompatSortNLSVersion
   enabled="4096"/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`enabled`|必需的特性。<br /><br /> 指定要使用其排序顺序的区域设置 ID。|  
  
## <a name="enabled-attribute"></a>enabled 特性  
  
|值|描述|  
|-----------|-----------------|  
|4096|表示备选排序顺序的区域设置 ID。 在这种情况下，4096表示 .NET Framework 3.5 及更早版本的排序顺序。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`runtime`|包含有关运行时初始化选项的信息。|  
  
## <a name="remarks"></a>备注  

 由于 .NET Framework 4 中的类执行的字符串比较、排序和大小写操作 <xref:System.Globalization.CompareInfo?displayProperty=nameWithType> 符合 Unicode 5.1 标准，因此和等字符串比较方法的结果 <xref:System.String.Compare%28System.String%2CSystem.String%29?displayProperty=nameWithType> <xref:System.String.LastIndexOf%28System.String%29?displayProperty=nameWithType> 可能与以前版本的 .NET Framework 不同。 如果你的应用程序依赖于旧行为，则可以通过在 `<CompatSortNLSVersion>` 应用程序配置文件中包括元素，来还原 .NET Framework 3.5 及更早版本中使用的字符串比较和排序规则。  
  
> [!IMPORTANT]
> 还原旧的字符串比较和排序规则还要求 sort00001000.dll 动态链接库在本地系统上可用。  
  
 此外，通过在创建应用程序域时将字符串“NetFx40_Legacy20SortingBehavior”传递到 <xref:System.AppDomainSetup.SetCompatibilitySwitches%2A> 方法，可以在特定的应用程序域中使用旧的字符串排序和比较规则。  
  
## <a name="example"></a>示例  

 下面的示例实例化两个 <xref:System.String> 对象，并调用 <xref:System.String.Compare%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType> 方法以通过使用当前区域性的约定对它们进行比较。  
  
 [!code-csharp[String.BreakingChanges#1](../../../../../samples/snippets/csharp/VS_Snippets_CLR/string.breakingchanges/cs/example1.cs#1)]
 [!code-vb[String.BreakingChanges#1](../../../../../samples/snippets/visualbasic/VS_Snippets_CLR/string.breakingchanges/vb/example1.vb#1)]  
  
 在 .NET Framework 4 上运行该示例时，它会显示以下输出：
  
```console
sta follows a in the sort order.  
```  
  
 这与在 .NET Framework 3.5 上运行示例时显示的输出完全不同：
  
```console
sta equals a in the sort order.  
```  
  
 但是，如果将下面的配置文件添加到示例的目录中，然后在 .NET Framework 4 上运行此示例，则输出将与示例在 .NET Framework 3.5 上运行时所生成的输出相同。  
  
```xml  
<?xml version ="1.0"?>  
<configuration>  
   <runtime>  
      <CompatSortNLSVersion enabled="4096"/>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
