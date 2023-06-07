---
title: OperationBehaviorAttribute
ms.date: 03/30/2017
ms.assetid: 8c9b0755-9e83-411f-bdcb-61a586022797
ms.openlocfilehash: 76cc619aed4ba2b944a8d11dc454a40368a4068c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96269075"
---
# <a name="operationbehaviorattribute"></a>OperationBehaviorAttribute

OperationBehaviorAttribute  
  
## <a name="syntax"></a>语法  
  
```csharp
class OperationBehaviorAttribute : Behavior  
{  
  boolean AutoDisposeParameters;  
  string Impersonation;  
  string ReleaseInstanceMode;  
  boolean TransactionAutoComplete;  
  boolean TransactionScopeRequired;  
};  
```  
  
## <a name="methods"></a>方法  

 OperationBehaviorAttribute 类不定义任何方法。  
  
## <a name="properties"></a>属性  

 OperationBehaviorAttribute 类具有下列属性：  
  
### <a name="autodisposeparameters"></a>AutoDisposeParameters  

 数据类型：Boolean  
  
 访问类型：只读  
  
 参数自动释放功能的状态。  
  
### <a name="impersonation"></a>模拟  

 数据类型：字符串  
  
 访问类型：只读  
  
 指示操作支持的调用方模拟级别。  
  
### <a name="releaseinstancemode"></a>ReleaseInstanceMode  

 数据类型：字符串  
  
 访问类型：只读  
  
 指示操作调用过程中何时回收对象。  
  
### <a name="transactionautocomplete"></a>TransactionAutoComplete  

 数据类型：Boolean  
  
 访问类型：只读  
  
 指示在未发生未处理的异常的情况下是否自动提交当前事务。  
  
### <a name="transactionscoperequired"></a>TransactionScopeRequired  

 数据类型：Boolean  
  
 访问类型：只读  
  
 指示操作是否需要事务处理。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.OperationBehaviorAttribute>
