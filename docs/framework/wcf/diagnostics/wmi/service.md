---
title: 服务
ms.date: 03/30/2017
ms.assetid: 999806e1-6376-409e-b998-b0af391adfe7
ms.openlocfilehash: aa4eecbcc8a2ef818cd99d777b0e3c2f1f222e46
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96273277"
---
# <a name="service"></a>服务

服务  
  
## <a name="syntax"></a>语法  
  
```csharp
class Service  
{  
  string BaseAddresses[];  
  Behavior Behaviors[];  
  string ConfigurationName;  
  string CounterInstanceName;  
  string DistinguishedName;  
  string Extensions[];  
  string Metadata[];  
  string Name;  
  string Namespace;  
  datetime Opened;  
  Channel OutgoingChannels[];  
  sint32 ProcessId;  
};  
```  
  
## <a name="methods"></a>方法  

 Service 类未定义任何方法。  
  
## <a name="properties"></a>属性  

 Service 类具有以下属性：  
  
### <a name="baseaddresses"></a>BaseAddresses  

 数据类型：String array  
  
 访问类型：只读  
  
 服务使用的基址。  
  
### <a name="behaviors"></a>行为  

 数据类型：Behavior array  
  
 访问类型：只读  
  
 与此服务关联的行为。  
  
### <a name="configurationname"></a>ConfigurationName  

 数据类型：字符串  
  
 访问类型：只读  
  
 ServiceElement_BehaviorConfiguration  
  
### <a name="counterinstancename"></a>CounterInstanceName  

 数据类型：字符串  
  
 访问类型：只读  
  
 此服务的性能计数器实例的实例名称。  
  
### <a name="distinguishedname"></a>DistinguishedName  

 数据类型：字符串  
  
 访问类型：只读  
  
 该地址处的服务名称。  
  
### <a name="extensions"></a>扩展  

 数据类型：String array  
  
 访问类型：只读  
  
 服务实例扩展的实例上下文。  
  
### <a name="metadata"></a>元数据  

 数据类型：String array  
  
 访问类型：只读  
  
 服务元数据设置。  
  
### <a name="name"></a>“属性”  

 数据类型：字符串  
  
 访问类型：只读  
  
 此服务的唯一名称。  
  
### <a name="namespace"></a>命名空间  

 数据类型：字符串  
  
 访问类型：只读  
  
 服务的命名空间。  
  
### <a name="opened"></a>已打开  

 数据类型：DateTime  
  
 访问类型：只读  
  
 服务打开的时间。  
  
### <a name="outgoingchannels"></a>OutgoingChannels  

 数据类型：Channel array  
  
 访问类型：只读  
  
 从服务实例传出的通道。  
  
### <a name="processid"></a>ProcessId  

 数据类型：sint32  
  
 访问类型：只读  
  
 承载服务的进程的进程 ID。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|
