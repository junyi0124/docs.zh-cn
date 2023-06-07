---
title: TcpConnectionPoolSettings
ms.date: 03/30/2017
ms.assetid: 19acfba3-c057-4dbc-bac7-8674d7844d83
ms.openlocfilehash: de00cac851e4c6d0fd6df16f3a01b65bb5f43415
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294672"
---
# <a name="tcpconnectionpoolsettings"></a>TcpConnectionPoolSettings

TcpConnectionPoolSettings  
  
## <a name="syntax"></a>语法  
  
```csharp
class TcpConnectionPoolSettings  
{  
  string GroupName;  
  datetime IdleTimeout;  
  datetime LeaseTimeout;  
  sint32 MaxOutboundConnectionsPerEndpoint;  
};  
```  
  
## <a name="methods"></a>方法  

 TcpConnectionPoolSettings 类不定义任何方法。  
  
## <a name="properties"></a>属性  

 TcpConnectionPoolSettings 类具有以下属性：  
  
### <a name="groupname"></a>GroupName  

 数据类型：字符串  
  
 访问类型：只读  
  
 绑定元素使用的连接池的组名。  
  
### <a name="idletimeout"></a>IdleTimeout  

 数据类型：DateTime  
  
 访问类型：只读  
  
 连接在断开前可以空闲的最长时间。  
  
### <a name="leasetimeout"></a>LeaseTimeout  

 数据类型：DateTime  
  
 访问类型：只读  
  
 超时前完成租约操作的最大时间。  
  
### <a name="maxoutboundconnectionsperendpoint"></a>MaxOutboundConnectionsPerEndpoint  

 数据类型：sint32  
  
 访问类型：只读  
  
 每个终结点的最大出站连接数。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.TcpConnectionPoolSettings>
