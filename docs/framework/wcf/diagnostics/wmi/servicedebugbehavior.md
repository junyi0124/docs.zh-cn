---
title: ServiceDebugBehavior
ms.date: 03/30/2017
ms.assetid: a5ec9061-1e95-43fb-b0d9-dbd0a7bc3c44
ms.openlocfilehash: dba4abd74cdddeb2b641feec5902413fe0704b1f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262289"
---
# <a name="servicedebugbehavior"></a>ServiceDebugBehavior

ServiceDebugBehavior  
  
## <a name="syntax"></a>语法  
  
```csharp
class ServiceDebugBehavior : Behavior  
{  
  boolean HttpHelpPageEnabled;  
  string HttpHelpPageUrl;  
  boolean HttpsHelpPageEnabled;  
  string HttpsHelpPageUrl;  
  boolean IncludeExceptionDetailInFaults;  
};  
```  
  
## <a name="methods"></a>方法  

 ServiceDebugBehavior 类未定义任何方法。  
  
## <a name="properties"></a>属性  

 ServiceDebugBehavior 类具有以下属性：  
  
### <a name="httphelppageenabled"></a>HttpHelpPageEnabled  

 数据类型：Boolean  
  
 访问类型：只读  
  
 控制服务是否在 `HttpGetUrl` 属性控制的地址上发布其 WSDL。  
  
### <a name="httphelppageurl"></a>HttpHelpPageUrl  

 数据类型：字符串  
  
 访问类型：只读  
  
 设置位置，在该位置发布服务 WSDL 以便使用 HTTP 进行检索。  
  
### <a name="httpshelppageenabled"></a>HttpsHelpPageEnabled  

 数据类型：Boolean  
  
 访问类型：只读  
  
 控制服务是否在 `HttpsGetUrl` 属性控制的地址上通过 HTTPS 发布其 WSDL。  
  
### <a name="httpshelppageurl"></a>HttpsHelpPageUrl  

 数据类型：字符串  
  
 访问类型：只读  
  
 设置位置，在该位置发布服务 WSDL 以便使用 HTTPS 进行检索。  
  
### <a name="includeexceptiondetailinfaults"></a>IncludeExceptionDetailInFaults  

 数据类型：Boolean  
  
 访问类型：只读  
  
 指定是否在返回给客户端的 SOAP 错误详细信息中包含托管异常信息以供调试。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Description.ServiceDebugBehavior>
