---
title: AppDomainInfo
ms.date: 03/30/2017
ms.assetid: 6610b7d8-81eb-4bec-a543-9b72ad7b6f73
ms.openlocfilehash: c5c44f4d8f6d93443802d5e1950c4d850976c5b6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96291123"
---
# <a name="appdomaininfo"></a>AppDomainInfo

应用程序域信息  
  
## <a name="syntax"></a>语法  
  
```csharp
class AppDomainInfo  
{  
  sint32 AppDomainId;  
  boolean IsDefault;  
  boolean LogMalformedMessages;  
  boolean LogMessagesAtServiceLevel;  
  boolean LogMessagesAtTransportLevel;  
  TraceListener MessageLoggingTraceListeners[];  
  string Name;  
  string PerformanceCounters;  
  sint32 ProcessId;  
  string ServiceConfigPath;  
  string TraceLevel;  
  TraceListener wmiTraceListeners[];  
};  
```  
  
## <a name="methods"></a>方法  

 AppDomainInfo 类未定义任何方法。  
  
## <a name="properties"></a>属性  

 AppDomainInfo 类具有以下属性：  
  
### <a name="appdomainid"></a>AppDomainId  

 数据类型：sint32  
  
 访问类型：只读  
  
 Appdomain 的 ID。  
  
### <a name="isdefault"></a>IsDefault  

 数据类型：Boolean  
  
 访问类型：只读  
  
 指示该 appdomain 是否为默认 appdomain。  
  
### <a name="logmalformedmessages"></a>LogMalformedMessages  

 数据类型：Boolean  
  
 访问类型：读/写  
  
 一个值，指定是否记录格式不正确的消息。  
  
### <a name="logmessagesatservicelevel"></a>LogMessagesAtServiceLevel  

 数据类型：Boolean  
  
 访问类型：读/写  
  
 一个值，指定是否在服务级别跟踪消息（在与加密和传输有关的转换之前）。  
  
### <a name="logmessagesattransportlevel"></a>LogMessagesAtTransportLevel  

 数据类型：Boolean  
  
 访问类型：读/写  
  
 一个值，指定是否在传输级别跟踪消息。  
  
### <a name="messageloggingtracelisteners"></a>MessageLoggingTraceListeners  

 数据类型：TraceListener 数组  
  
 访问类型：只读  
  
 侦听 System.Wmi.MessageLogging 跟踪源的集合跟踪侦听器。  
  
### <a name="name"></a>“属性”  

 数据类型：字符串  
  
 访问类型：只读  
  
 Appdomain 的名称。  
  
### <a name="performancecounters"></a>PerformanceCounters  

 数据类型：字符串  
  
 访问类型：只读  
  
 Appdomain 中的活动性能计数器的范围。  
  
### <a name="processid"></a>ProcessId  

 数据类型：sint32  
  
 访问类型：只读  
  
 进程 ID。  
  
### <a name="serviceconfigpath"></a>ServiceConfigPath  

 数据类型：字符串  
  
 访问类型：只读  
  
 服务配置的路径。  
  
### <a name="tracelevel"></a>TraceLevel  

 数据类型：字符串  
  
 访问类型：读/写  
  
 System.Wmi 跟踪源的跟踪级别。  
  
### <a name="servicemodeltracelisteners"></a>ServiceModelTraceListeners  

 数据类型：TraceListener 数组  
  
 访问类型：只读  
  
 一个来自 System.ServiceModel 跟踪源的侦听器的集合。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|
