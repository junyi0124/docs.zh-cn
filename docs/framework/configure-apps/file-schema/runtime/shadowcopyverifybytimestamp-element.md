---
title: <shadowCopyVerifyByTimestamp> 元素
ms.date: 03/30/2017
helpviewer_keywords:
- <shadowCopyTimeStampVerification> element
- shadowCopyTimeStampVerification element
ms.assetid: 2f1648e5-997b-435e-a4f9-d236c574c66c
ms.openlocfilehash: c0dc190e69ca9650d518ee297b12f79f8c47d58b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183970"
---
# <a name="shadowcopyverifybytimestamp-element"></a>\<shadowCopyVerifyByTimestamp> 元素

指定卷影复制是否使用 .NET Framework 4 中引入的默认启动行为，或恢复为 .NET Framework 早期版本的启动行为。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<shadowCopyVerifyByTimestamp>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<shadowCopyVerifyByTimestamp enabled="true|false" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|enabled|必需的特性。<br /><br /> 指定在启动时，使用卷影复制的应用程序域是否对程序集时间戳进行比较，以确定在卷影复制程序集之前是否更新了程序集。|  
  
## <a name="enabled-attribute"></a>enabled 特性  
  
|值|描述|  
|-----------|-----------------|  
|是|在启动时，仅复制自上次复制到卷影复制目录以来已更新的程序集。 这是 .NET Framework 4 的默认值。|  
|false|恢复到 .NET Framework 以前版本的启动行为，该行为是在启动时复制所有文件。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`runtime`|包含有关程序集绑定和垃圾回收的信息。|  
  
## <a name="remarks"></a>备注  

 从 .NET Framework 4 开始，仅当程序集的时间戳指示它们自上次复制到卷影复制目录后发生了更改时，才会对程序集进行卷影复制。 这会缩短使用卷影复制的许多应用程序的启动时间，如 [卷影复制程序集](../../../app-domains/shadow-copy-assemblies.md)中所述。 对于程序集更新百分比和频率都很高的应用程序，可能不会从此行为改变中获益。 在此情况下，可以使用此元素存储 .NET Framework 早先版本的行为。  
  
## <a name="example"></a>示例  

 下面的示例演示如何在 .NET Framework 4 中禁用卷影复制的默认启动行为，并还原为以前版本的 .NET Framework 的启动行为。  
  
```xml  
<configuration>  
   <runtime>  
      <shadowCopyVerifyByTimestamp enabled="false" />  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
- [卷影复制程序集](../../../app-domains/shadow-copy-assemblies.md)
