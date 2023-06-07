---
title: LocalServiceSecuritySettings
ms.date: 03/30/2017
ms.assetid: 490aa0e5-5242-4f8d-b505-5ec6287633b4
ms.openlocfilehash: eecf2b0bf459fd14236c550e393149553183b3ac
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96267918"
---
# <a name="localservicesecuritysettings"></a>LocalServiceSecuritySettings

LocalServiceSecuritySettings  
  
## <a name="syntax"></a>语法  
  
```csharp
class LocalServiceSecuritySettings  
{  
  boolean DetectReplays;  
  datetime InactivityTimeout;  
  datetime IssuedCookieLifetime;  
  sint32 MaxCachedCookies;  
  datetime MaxClockSkew;  
  sint32 MaxPendingSessions;  
  sint32 MaxStatefulNegotiations;  
  datetime NegotiationTimeout;  
  boolean ReconnectTransportOnFailure;  
  sint32 ReplayCacheSize;  
  datetime ReplayWindow;  
  datetime SessionKeyRenewalInterval;  
  datetime SessionKeyRolloverInterval;  
  datetime TimestampValidityDuration;  
};  
```  
  
## <a name="methods"></a>方法  

 LocalServiceSecuritySettings 类未定义任何方法。  
  
## <a name="properties"></a>属性  

 LocalServiceSecuritySettings 类具有以下属性：  
  
### <a name="detectreplays"></a>DetectReplays  

 数据类型：Boolean  
  
 访问类型：只读  
  
 一个布尔值，指定是否自动检测和处理针对通道的重放攻击。  
  
### <a name="inactivitytimeout"></a>InactivityTimeout  

 数据类型：DateTime  
  
 访问类型：只读  
  
 服务支持的最大挂起安全会话数。  
  
### <a name="issuedcookielifetime"></a>IssuedCookieLifetime  

 数据类型：DateTime  
  
 访问类型：只读  
  
 一个时间跨度，指定颁发给所有新的安全 Cookie 的生存期。  
  
### <a name="maxcachedcookies"></a>MaxCachedCookies  

 数据类型：sint32  
  
 访问类型：只读  
  
 可以缓存的最大 Cookie 数。  
  
### <a name="maxclockskew"></a>MaxClockSkew  

 数据类型：DateTime  
  
 访问类型：只读  
  
 一个时间跨度，指定通信双方系统时钟之间的最大时间差。  
  
### <a name="maxpendingsessions"></a>MaxPendingSessions  

 数据类型：sint32  
  
 访问类型：只读  
  
 服务上的最大挂起连接数。  
  
### <a name="maxstatefulnegotiations"></a>MaxStatefulNegotiations  

 数据类型：sint32  
  
 访问类型：只读  
  
 可以同时处于活动状态的安全协商数。  
  
### <a name="negotiationtimeout"></a>NegotiationTimeout  

 数据类型：DateTime  
  
 访问类型：只读  
  
 一个时间跨度，指定服务器和客户端之间的安全协商阶段的最大持续时间。  
  
### <a name="reconnecttransportonfailure"></a>ReconnectTransportOnFailure  

 数据类型：Boolean  
  
 访问类型：只读  
  
 一个布尔值，指定使用 WS-ReliableMessaging 的连接是否将在传输失败后尝试重新连接。  
  
### <a name="replaycachesize"></a>ReplayCacheSize  

 数据类型：sint32  
  
 访问类型：只读  
  
 用于重播检测的缓存 Nonce 的数目。  
  
### <a name="replaywindow"></a>ReplayWindow  

 数据类型：DateTime  
  
 访问类型：只读  
  
 一个时间跨度，指定单个消息 Nonce 的有效持续时间。  
  
### <a name="sessionkeyrenewalinterval"></a>SessionKeyRenewalInterval  

 数据类型：DateTime  
  
 访问类型：只读  
  
 一个时间跨度，指定一个持续时间，发起方将在此段时间之后续订安全会话的密钥。  
  
### <a name="sessionkeyrolloverinterval"></a>SessionKeyRolloverInterval  

 数据类型：DateTime  
  
 访问类型：只读  
  
 一个时间跨度，指定在密钥续订期间，上一个会话密钥对于传入消息有效的时间间隔。  
  
### <a name="timestampvalidityduration"></a>TimestampValidityDuration  

 数据类型：DateTime  
  
 访问类型：只读  
  
 一个时间跨度，指定时间戳的有效持续时间。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings>
