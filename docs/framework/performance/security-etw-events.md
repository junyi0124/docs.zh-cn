---
title: 安全 ETW 事件
description: 了解在 .NET 中强名称验证和验证码验证期间引发的安全 ETW 事件。
ms.date: 03/30/2017
helpviewer_keywords:
- security events [.NET Framework]
- ETW, security events (CLR)
ms.assetid: 0ed69f73-5c01-4514-bd63-979c6e38d41d
ms.openlocfilehash: 4402bf5690a53ce518077268a3e20a95aeb14e8a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96272509"
---
# <a name="security-etw-events"></a>安全 ETW 事件

在强名称验证和验证码验证期间会引发安全事件。  

## <a name="strongnameverificationstart_v1-and-strongnameverificationstop_v1-events"></a>StrongNameVerificationStart_V1 和 StrongNameVerificationStop_V1 事件  

 下表显示了关键字和级别。 （有关详细信息，请参阅 [CLR ETW Keywords and Levels](clr-etw-keywords-and-levels.md)。）  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`SecurityKeyword` (0x400)|信息性 (4)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|在发生以下情况时引发|  
|-----------|--------------|-----------------|  
|`StrongNameVerificationStart_V1`|181|强名称验证开始。|  
|`StrongNameVerificationStop_V1`|182|强名称验证结束。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|VerificationFlags|win:UInt32|验证标志。|  
|ErrorCode|win:UInt32|HResult 错误代码。|  
|FullyQualifiedAssemblyName|win:UnicodeString|完全限定程序集名称。|  
|ClrInstanceID|win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  

## <a name="authenticodeverificationstart_v1-and-authenticodeverificationstop_v1-events"></a>AuthenticodeVerificationStart_V1 and AuthenticodeVerificationStop_V1 事件  

 下表显示了关键字和级别。  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`SecurityKeyword` (0x400)|信息性 (4)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|在发生以下情况时引发|  
|-----------|--------------|-----------------|  
|`AuthenticodeVerificationStart_V1`|183|验证码验证开始。|  
|`AuthenticodeVerificationStop_V1`|184|验证码验证结束。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|VerificationFlags|win:UInt32|验证标志。|  
|ErrorCode|win:UInt32|HResult 错误代码。|  
|ModulePath|win:UnicodeString|模块路径。|  
|ClrInstanceID|win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  
  
## <a name="see-also"></a>另请参阅

- [CLR ETW 事件](clr-etw-events.md)
