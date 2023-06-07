---
title: CallbackBehavior
ms.date: 03/30/2017
ms.assetid: 42acd302-2b62-4849-a2d1-a03084343ecd
ms.openlocfilehash: cec9005a099247671dbebaacc0b242ca8d7a0144
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96269643"
---
# <a name="callbackbehavior"></a>CallbackBehavior

CallbackBehavior  
  
## <a name="syntax"></a>语法  
  
```csharp
class CallbackBehavior : Behavior  
{  
  boolean AutomaticSessionShutdown;  
  string ConcurrencyMode;  
  boolean IgnoreExtensionDataObject;  
  boolean IncludeExceptionDetailInFaults;  
  boolean MaxItemsInObjectGraph;  
  boolean UseSynchronizationContext;  
  boolean ValidateMustUnderstand;  
};  
```  
  
## <a name="methods"></a>方法  

 CallbackBehavior 类未定义任何方法。  
  
## <a name="properties"></a>属性  

 CallbackBehavior 类具有以下属性：  
  
### <a name="automaticsessionshutdown"></a>AutomaticSessionShutdown  

 数据类型：Boolean  
  
 访问类型：只读  
  
 如果为 true，则会话在服务关闭双工会话时自动关闭。  
  
### <a name="concurrencymode"></a>ConcurrencyMode  

 数据类型：字符串  
访问类型：只读  
  
 指定服务是支持一个线程、多个线程还是支持可重入调用。  
  
### <a name="ignoreextensiondataobject"></a>IgnoreExtensionDataObject  

 数据类型：Boolean  
  
 访问类型：只读  
  
 一个值，指定是否将未知序列化数据发送到网络上。  
  
### <a name="includeexceptiondetailinfaults"></a>IncludeExceptionDetailInFaults  

 数据类型：Boolean  
  
 访问类型：只读  
  
 如果启用，则回调异常的详细信息被附加到服务所返回的错误中。  
  
### <a name="maxitemsinobjectgraph"></a>MaxItemsInObjectGraph  

 数据类型：Boolean  
  
 访问类型：只读  
  
 序列化对象中允许的最大项数。  
  
### <a name="usesynchronizationcontext"></a>UseSynchronizationContext  

 数据类型：Boolean  
  
 访问类型：只读  
  
 指定是否使用当前同步上下文来选择执行的线程。  
  
### <a name="validatemustunderstand"></a>ValidateMustUnderstand  

 数据类型：Boolean  
  
 访问类型：只读  
  
 指定系统或应用程序是否强制执行 SOAP MustUnderstand 标头处理。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.CallbackBehaviorAttribute>
