---
title: SslStreamSecurityBindingElement
ms.date: 03/30/2017
ms.assetid: 18130d50-8996-4257-9c60-bc457f8654d8
ms.openlocfilehash: fcb6cb304c7435f8b954ed5aeea7179c7cb40be8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96282088"
---
# <a name="sslstreamsecuritybindingelement"></a>SslStreamSecurityBindingElement

SslStreamSecurityBindingElement  
  
## <a name="syntax"></a>语法  
  
```csharp
class SslStreamSecurityBindingElement : BindingElement  
{  
  boolean RequireClientCertificate;  
};  
```  
  
## <a name="methods"></a>方法  

 SslStreamSecurityBindingElement 类不定义任何方法。  
  
## <a name="properties"></a>属性  

 SslStreamSecurityBindingElement 类具有以下属性：  
  
### <a name="requireclientcertificate"></a>RequireClientCertificate  

 数据类型：Boolean  
  
 访问类型：只读  
  
 指定此绑定是否需要客户端证书。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement>
