---
title: ICLRErrorReportingManager 接口
ms.date: 03/30/2017
api_name:
- ICLRErrorReportingManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRErrorReportingManager
helpviewer_keywords:
- ICLRErrorReportingManager interface [.NET Framework hosting]
ms.assetid: ea8af0d5-4133-4472-8a1f-50570d7e85fa
topic_type:
- apiref
ms.openlocfilehash: d3816c8a3b6204b053505aa888eb28d696f8990b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95677833"
---
# <a name="iclrerrorreportingmanager-interface"></a>ICLRErrorReportingManager 接口

提供允许主机配置自定义堆栈转储以用于错误报告的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[BeginCustomDump 方法](iclrerrorreportingmanager-begincustomdump-method.md)|指定用于错误报告的自定义堆栈转储配置。|  
|[EndCustomDump 方法](iclrerrorreportingmanager-endcustomdump-method.md)|清除由先前对的调用设置的自定义堆栈转储配置 `BeginCustomDump` 。|  
|[GetBucketParametersForCurrentException 方法](iclrerrorreportingmanager-getbucketparametersforcurrentexception-method.md)|获取调用线程上的当前异常的 Watson 存储桶。|  
  
## <a name="remarks"></a>注解  

 `BeginCustomDump`方法设置自定义堆栈转储配置。 `EndCustomDump`方法清除自定义堆栈转储配置并释放任何关联的状态。 应在自定义转储完成后调用它。  
  
> [!IMPORTANT]
> 调用失败 `EndCustomDump` 会导致内存泄露。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ECustomDumpItemKind 枚举](ecustomdumpitemkind-enumeration.md)
- [承载接口](hosting-interfaces.md)
