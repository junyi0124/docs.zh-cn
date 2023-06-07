---
title: ISymUnmanagedAsyncMethod 接口
ms.date: 03/30/2017
ms.assetid: f2de5224-fd91-45de-9e58-bc600c6d22f1
ms.openlocfilehash: 02b1866f2b9e89cdc8c8795f399ecc0c733c7202
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95707160"
---
# <a name="isymunmanagedasyncmethod-interface"></a>ISymUnmanagedAsyncMethod 接口

此接口是 [ISymUnmanagedAsyncMethodPropertiesWriter 接口](isymunmanagedasyncmethodpropertieswriter-interface.md)的读取补充。  
  
## <a name="syntax"></a>语法  
  
```idl  
[object,uuid(B20D55B3-532E-4906-87E7-25BD5734ABD2),pointer_default(unique)]interface ISymUnmanagedAsyncMethod : IUnknown  
```  
  
## <a name="methods"></a>方法  

 此接口包含下列方法：  
  
|方法|说明|  
|------------|-----------------|  
|[GetAsyncStepInfo 方法](isymunmanagedasyncmethod-getasyncstepinfo-method.md)|请参阅 [DefineAsyncStepInfo 方法](isymunmanagedasyncmethodpropertieswriter-defineasyncstepinfo-method.md)。|  
|[GetAsyncStepInfoCount 方法](isymunmanagedasyncmethod-getasyncstepinfocount-method.md)|请参阅 [DefineAsyncStepInfo 方法](isymunmanagedasyncmethodpropertieswriter-defineasyncstepinfo-method.md)。|  
|[GetCatchHandlerILOffset 方法](isymunmanagedasyncmethod-getcatchhandleriloffset-method.md)|请参阅 [DefineCatchHandlerILOffset 方法](isymunmanagedasyncmethodpropertieswriter-definecatchhandleriloffset-method.md)。|  
|[GetKickoffMethod 方法](isymunmanagedasyncmethod-getkickoffmethod-method.md)|请参阅 [DefineKickoffMethod 方法](isymunmanagedasyncmethodpropertieswriter-definekickoffmethod-method.md)。|  
|[HasCatchHandlerILOffset 方法](isymunmanagedasyncmethod-hascatchhandleriloffset-method.md)|请参阅 [DefineCatchHandlerILOffset 方法](isymunmanagedasyncmethodpropertieswriter-definecatchhandleriloffset-method.md)。|  
|[IsAsyncMethod 方法](isymunmanagedasyncmethod-isasyncmethod-method.md)|检查方法是否有异步信息。<br /><br /> 如果此方法返回，则 `FALSE` 调用此接口中的任何其他方法无效。 `E_UNEXPECTED`在这种情况下，它们都将返回。|  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [诊断符号存储区接口](diagnostics-symbol-store-interfaces.md)
