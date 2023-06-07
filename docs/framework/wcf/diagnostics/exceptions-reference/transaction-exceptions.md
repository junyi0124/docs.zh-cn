---
title: 事务异常
ms.date: 03/30/2017
ms.assetid: 1d27ed51-7eda-477f-9eca-94fa129f3e07
ms.openlocfilehash: dcdf825699368617335f2d59a05f8826884a8e9e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96285676"
---
# <a name="transaction-exceptions"></a>事务异常

本主题列出了 Windows Communication Foundation (WCF) 事务生成的所有异常。  
  
## <a name="exception-list"></a>异常列表  
  
|资源代码|资源字符串|  
|-------------------|---------------------|  
|SFxCannotHaveDifferentTransactionProtocolsInOneBinding|从元数据导入的策略信息指定各个操作的不同 TransactionProtocol 值。 每个终结点仅支持一个 TransactionProtocol。|  
|SFxTransactionAutoCompleteFalseAndInstanceContextMode|TransactionAutoComplete 不能为 false，除非服务的 InstanceContextMode 为 PerSession。 在指定约定和操作的实现上发现错误。|  
|SFxTransactionInvalidSetTransactionComplete|仅当 TransactionAutoComplete 设置为 false 且 TransactionScopeRequired 设置为 true 时，才能在操作中调用 OperationContext.SetTransactionComplete。 这种方案无效，当前事务已终止。|  
|TransactionFlowRequiredIssuedTokens|若要对事务进行流处理，还必须支持对已颁发的令牌进行流处理。|  
|TrustDriverVersionDoesNotSupportIssuedTokens|配置的 Trust 版本不支持已颁发的令牌。 请使用 WSTrustFeb2005 或更高版本。|
