---
title: PrivacyNoticeBindingElement
ms.date: 03/30/2017
ms.assetid: 0cf110b1-e25b-4d67-986b-10cb04dc4826
ms.openlocfilehash: c6ebb585454054ca9a03c46cf738001c58f9b18e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96273407"
---
# <a name="privacynoticebindingelement"></a>PrivacyNoticeBindingElement

PrivacyNoticeBindingElement  
  
## <a name="syntax"></a>语法  
  
```csharp
class PrivacyNoticeBindingElement : BindingElement  
{  
  sint32 PrivacyNoticeVersion;  
  string Url;  
};  
```  
  
## <a name="methods"></a>方法  

 PrivacyNoticeBindingElement 类未定义任何方法。  
  
## <a name="properties"></a>属性  

 PrivacyNoticeBindingElement 类具有以下属性：  
  
### <a name="privacynoticeversion"></a>PrivacyNoticeVersion  

 数据类型：sint32  
  
 访问类型：只读  
  
 隐私声明版本。  
  
### <a name="url"></a>URL  

 数据类型：字符串  
  
 访问类型：只读  
  
 隐私声明所在的 URL。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.PrivacyNoticeBindingElement>
