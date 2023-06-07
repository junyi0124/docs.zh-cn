---
title: NamedPipeTransportBindingElement
ms.date: 03/30/2017
ms.assetid: c201309c-c528-4b92-a53c-4d48151c5749
ms.openlocfilehash: 8e403ec02b09d0dbc165c40c2bd28d54d0219325
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96250406"
---
# <a name="namedpipetransportbindingelement"></a>NamedPipeTransportBindingElement

NamedPipeTransportBindingElement  
  
## <a name="syntax"></a>语法  
  
```csharp
class NamedPipeTransportBindingElement : ConnectionOrientedTransportBindingElement  
{  
  NamedPipeConnectionPoolSettings ConnectionPoolSettings;  
};  
```  
  
## <a name="methods"></a>方法  

 NamedPipeTransportBindingElement 类不定义任何方法。  
  
## <a name="properties"></a>属性  

 NamedPipeTransportBindingElement 类具有以下属性：  
  
### <a name="connectionpoolsettings"></a>ConnectionPoolSettings  

 数据类型：NamedPipeConnectionPoolSettings  
  
 访问类型：只读  
  
 连接池设置。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.NamedPipeTransportBindingElement>
