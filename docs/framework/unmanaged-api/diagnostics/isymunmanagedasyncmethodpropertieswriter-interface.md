---
title: ISymUnmanagedAsyncMethodPropertiesWriter 接口
ms.date: 03/30/2017
ms.assetid: caa71820-8058-4b6a-93a2-25ee757d92d3
ms.openlocfilehash: 779b737da43f61d1023a0a640dce936e11c4704c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95707030"
---
# <a name="isymunmanagedasyncmethodpropertieswriter-interface"></a>ISymUnmanagedAsyncMethodPropertiesWriter 接口

允许您为每个方法符号定义可选的异步方法信息。 始终使用打开的方法;也就是说，在对 [OpenMethod 方法](isymunmanagedwriter-openmethod-method.md) 和 [CloseMethod 方法](isymunmanagedwriter-closemethod-method.md)的调用之间。  
  
## <a name="syntax"></a>语法  
  
```idl  
[object,uuid(FC073774-1739-4232-BD56-A027294BEC15),pointer_default(unique)]interface ISymUnmanagedAsyncMethodPropertiesWriter : IUnknown  
```  
  
## <a name="methods"></a>方法  

 此接口包含下列方法：  
  
|方法|说明|  
|------------|-----------------|  
|[DefineAsyncStepInfo 方法](isymunmanagedasyncmethodpropertieswriter-defineasyncstepinfo-method.md)|在当前方法中定义一组异步等待操作。<br /><br /> 每个 yield 偏移均与 await 的返回指令匹配，确定潜在的收益率。 每个 `breakpointMethod` / `breakpointOffset` 对都标识异步操作将恢复到的位置; 它可能采用不同的方法。|  
|[DefineCatchHandlerILOffset 方法](isymunmanagedasyncmethodpropertieswriter-definecatchhandleriloffset-method.md)|设置编译器生成的用于包装异步方法的 catch 处理程序的 IL 偏移量。<br /><br /> 调试器使用生成的 catch 的 IL 偏移量来处理 catch，就像它是非用户代码一样，即使它可能出现在用户代码方法中也是如此。 具体而言，它用于响应 **CatchHandlerFound** 异常事件。|  
|[DefineKickoffMethod 方法](isymunmanagedasyncmethodpropertieswriter-definekickoffmethod-method.md)|设置启动异步操作的启动方法。|  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [诊断符号存储区接口](diagnostics-symbol-store-interfaces.md)
