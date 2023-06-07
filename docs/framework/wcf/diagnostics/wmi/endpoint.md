---
title: 终结点
ms.date: 03/30/2017
ms.assetid: fe63370d-81a1-40f3-97c2-59cb357c78d2
ms.openlocfilehash: ceb4e4b41502b00d7bb21f1ecbd8249fccf1ce3b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96288809"
---
# <a name="endpoint"></a>终结点

终结点  
  
## <a name="syntax"></a>语法  
  
```csharp
class Endpoint  
{  
  string Address;  
  string AddressHeaders[];  
  string AddressIdentity;  
  sint32 AppDomainId;  
  Behavior Behaviors[];  
  Binding Binding;  
  string ContractName;  
  string CounterInstanceName;  
  string ListenUri;  
  string Name;  
  sint32 ProcessId;  
  Contract ref;  
};  
```  
  
## <a name="methods"></a>方法  

 该终结点类定义以下方法。  
  
|方法|描述|  
|------------|-----------------|  
|[GetOperationCounterInstanceName](getoperationcounterinstancename.md)|检索操作性能计数器实例名称|  
  
## <a name="properties"></a>属性  

 该终结点类具有以下属性：  
  
### <a name="address"></a>地址  

 数据类型：字符串  
  
 访问类型：只读  
  
 一个包含终结点地址的 URI。  
  
### <a name="addressheaders"></a>AddressHeaders  

 数据类型：String array  
  
 访问类型：只读  
  
 附加到此终结点的地址头的集合。  
  
### <a name="addressidentity"></a>AddressIdentity  

 数据类型：字符串  
  
 访问类型：只读  
  
 终结点的标识。  
  
### <a name="appdomainid"></a>AppDomainId  

 数据类型：sint32  
  
 访问类型：只读  
  
 承载终结点的 AppDomain 的 AppDomain ID。  
  
### <a name="behaviors"></a>行为  

 数据类型：Behavior array  
  
 访问类型：只读  
  
 此终结点所实现的行为的集合。  
  
### <a name="binding"></a>绑定  

 数据类型：绑定  
  
 访问类型：只读  
  
 此终结点所使用的绑定。  
  
### <a name="contractname"></a>ContractName  

 数据类型：字符串  
  
 访问类型：只读  
  
 一个字符串，指定此终结点公开了哪个协定。  
  
### <a name="counterinstancename"></a>CounterInstanceName  

 数据类型：字符串  
  
 访问类型：只读  
  
 该终结点的性能计数器的实例名称。  
  
### <a name="listenuri"></a>ListenUri  

 数据类型：字符串  
  
 访问类型：只读  
  
 终结点在其上侦听的 Uri。  
  
### <a name="name"></a>“属性”  

 数据类型：字符串  
  
 访问类型：只读  
  
 此终结点的唯一名称。  
  
### <a name="processid"></a>ProcessId  

 数据类型：sint32  
  
 访问类型：只读  
  
 承载该终结点的进程的进程 ID。  
  
### <a name="ref"></a>ref  

 数据类型：Contract  
  
 访问类型：只读  
  
 此终结点公开的协定。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|
