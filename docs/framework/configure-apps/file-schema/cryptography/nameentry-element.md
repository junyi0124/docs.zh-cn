---
title: <nameEntry> 元素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#nameEntry
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/mscorlib/cryptographySettings/cryptoNameMapping/nameEntry
helpviewer_keywords:
- <nameEntry> element
- nameEntry element
ms.assetid: 7d7535e9-4b4a-4b8c-82e2-e40dff5a7821
ms.openlocfilehash: 4341b1fcd3762e5aa55f0ba988f7f49d4b5cacd6
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201767"
---
# <a name="nameentry-element"></a>\<nameEntry> 元素

将类名称映射到友好算法名称，允许一个类具有多个友好名称。  
  
[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;[**\<mscorlib>**](mscorlib-element-for-cryptography-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;[**\<cryptographySettings>**](cryptographysettings-element.md)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<cryptoNameMapping>**](cryptonamemapping-element.md)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<nameEntry>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<nameEntry name="friendly name" Class="class name" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|name|必需的特性。<br /><br /> 指定密码类实现的算法的友好名称。|  
|**class**|必需的特性。<br /><br /> 指定元素中 **name** 属性的值 [\<cryptoClass>](cryptoclass-element.md) 。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`system.web`|为 ASP.NET 配置节指定根元素。|  
  
## <a name="remarks"></a>备注  

 **Name**属性可以是在命名空间中找到的某个抽象类的名称 <xref:System.Security.Cryptography> 。 对抽象加密类调用 **Create** 方法时，会将抽象类名称传递给 <xref:System.Security.Cryptography.CryptoConfig.CreateFromName%2A> 方法。 **Cryptoconfig.createfromname** 返回 **类** 特性指示的类型的实例。 如果 **name** 属性是一个短名称，例如 RSA，则可以在调用 **cryptoconfig.createfromname** 方法时使用该名称。  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用 **\<nameEntry>** 元素来引用加密类并配置运行时。 然后，你可以将字符串 "RSA" 传递给 <xref:System.Security.Cryptography.CryptoConfig.CreateFromName%2A?displayProperty=nameWithType> 方法，并使用 <xref:System.Security.Cryptography.AsymmetricAlgorithm.Create%2A> 方法返回 `MyCryptoRSAClass` 对象。  
  
```xml  
<configuration>  
   <mscorlib>  
      <cryptographySettings>  
         <cryptoNameMapping>  
            <cryptoClasses>  
               <cryptoClass   MyCryptoRSA="MyCryptoRSAClass, MyAssembly  
                  Culture=neutral, PublicKeyToken=a5d015c7d5a0b012,  
                  Version=1.0.0.0"/>  
            </cryptoClasses>  
            <nameEntry name="RSA" class="MyCryptoRSA"/>  
            <nameEntry name="System.Security.Cryptography.AsymmetricAlgorithm"  
                       class="MyCryptoRSA"/>  
         </cryptoNameMapping>  
      </cryptographySettings>  
   </mscorlib>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- [配置文件架构](../index.md)
- [加密设置架构](index.md)
- [加密服务](../../../../standard/security/cryptographic-services.md)
- [配置加密类](../../configure-cryptography-classes.md)
