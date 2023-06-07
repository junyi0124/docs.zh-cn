---
title: ICLRValidator 接口
ms.date: 03/30/2017
api_name:
- ICLRValidator
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRValidator
helpviewer_keywords:
- ICLRValidator interface [.NET Framework hosting]
ms.assetid: 2edd0a10-77fb-4173-91eb-f2970cc364d0
topic_type:
- apiref
ms.openlocfilehash: d9ccd5c6c91b1ab2166ff40a0fb2048e15927d3a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723943"
---
# <a name="iclrvalidator-interface"></a>ICLRValidator 接口

提供用于验证 (PE) 映像和报告验证错误的可移植可执行文件的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[FormatEventInfo 方法](iclrvalidator-formateventinfo-method.md)|获取有关指定验证错误的详细消息。|  
|[Validate 方法](iclrvalidator-validate-method.md)|验证指定文件中的可移植可执行文件或 Microsoft 中间语言 (MSIL) 。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** IValidator，IValidator  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRErrorReportingManager 接口](iclrerrorreportingmanager-interface.md)
- [承载接口](hosting-interfaces.md)
- [CLRRuntimeHost 组件类](clrruntimehost-coclass.md)
