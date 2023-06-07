---
title: <UseRandomizedStringHashAlgorithm> 元素
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- UseRandomizedStringHashAlgorithm element
- <UseRandomizedStringHashAlgorithm> element
ms.assetid: c08125d6-56cc-4b23-b482-813ff85dc630
ms.openlocfilehash: 148d55c8b8a63737867c4bfdf3ab118dfdefd6f9
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91174090"
---
# <a name="userandomizedstringhashalgorithm-element"></a>\<UseRandomizedStringHashAlgorithm> 元素

确定公共语言运行时是否按应用程序域计算字符串的哈希代码。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<UseRandomizedStringHashAlgorithm>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<UseRandomizedStringHashAlgorithm
   enabled=0|1 />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`enabled`|必需的特性。<br /><br /> 指定是否基于每个应用程序域计算字符串的哈希代码。|  
  
## <a name="enabled-attribute"></a>enabled 特性  
  
|值|描述|  
|-----------|-----------------|  
|`0`|公共语言运行时不会为每个应用程序域计算字符串的哈希代码;单个算法用于计算字符串哈希代码。 这是默认设置。|  
|`1`|公共语言运行时基于每个应用程序域计算字符串的哈希代码。 不同应用程序域和不同进程中的相同字符串具有不同的哈希代码。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`runtime`|包含有关运行时初始化选项的信息。|  
  
## <a name="remarks"></a>备注  

 默认情况下， <xref:System.StringComparer> 类和 <xref:System.String.GetHashCode%2A?displayProperty=nameWithType> 方法使用单个哈希算法，该算法可跨应用程序域生成一致的哈希代码。 这等效于将元素的 `enabled` 特性设置 `<UseRandomizedStringHashAlgorithm>` 为 `0` 。 这是 .NET Framework 4 中使用的哈希算法。  
  
 <xref:System.StringComparer>类和 <xref:System.String.GetHashCode%2A?displayProperty=nameWithType> 方法也可以使用不同的哈希算法来计算每个应用程序域的哈希代码。 因此，相同字符串的哈希代码将在应用程序域之间有所不同。 这是一项可选功能;若要利用它，必须将 `enabled` 元素的属性设置 `<UseRandomizedStringHashAlgorithm>` 为 `1` 。  
  
 哈希表中的字符串查找通常是 O (1) 操作。 但是，当发生大量冲突时，查找可能会成为 O (n<sup>2</sup>) 操作。 您可以使用 `<UseRandomizedStringHashAlgorithm>` configuration 元素为每个应用程序域生成随机哈希算法，这反过来会限制潜在冲突的数目，特别是当从中计算哈希代码的键基于用户输入的数据时。  
  
## <a name="example"></a>示例  

 下面的示例定义了一个 `DisplayString` 类，该类包含一个私有字符串常量， `s` 其值为 "This is a string"。 它还包括显示字符串值及其哈希代码的 `ShowStringHashCode` 方法以及该方法在其中执行的应用程序域的名称。  
  
 [!code-csharp[System.String.GetHashCode#2](../../../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.String.GetHashCode/CS/perdomain.cs#2)]
 [!code-vb[System.String.GetHashCode#2](../../../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.String.GetHashCode/VB/perdomain.vb#2)]  
  
 当您在未提供配置文件的情况下运行该示例时，它会显示类似下面的输出。 请注意，字符串的散列码在两个应用程序域中是相同的。  
  
```console
String 'This is a string.' in domain 'PerDomain.exe': 941BCEAC  
String 'This is a string.' in domain 'NewDomain': 941BCEAC  
```  
  
 但是，如果将以下配置文件添加到示例目录，然后运行该示例，则同一个字符串的哈希代码将通过应用程序域进行区分。  
  
```xml  
<?xml version ="1.0"?>  
<configuration>  
   <runtime>  
      <UseRandomizedStringHashAlgorithm enabled="1" />  
   </runtime>  
</configuration>  
```  
  
 存在配置文件时，示例会显示以下输出：  
  
```console
String 'This is a string.' in domain 'PerDomain.exe': 5435776D  
String 'This is a string.' in domain 'NewDomain': 75CC8236  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType>
- <xref:System.String.GetHashCode%2A?displayProperty=nameWithType>
- <xref:System.Object.GetHashCode%2A?displayProperty=nameWithType>
