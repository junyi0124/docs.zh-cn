---
title: CorpubPublish Coclass
ms.date: 03/30/2017
api_name:
- CorpubPublish Coclass
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorpubPublish
helpviewer_keywords:
- CorpubPublish coclass [.NET Framework debugging]
ms.assetid: 191015da-f54a-4bac-a28a-1de7ab3c3428
topic_type:
- apiref
ms.openlocfilehash: c73eab14bf6f9f9599bed79f4c5f85ed035c0518
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722344"
---
# <a name="corpubpublish-coclass"></a>CorpubPublish Coclass

提供用于发布应用程序域和进程相关信息的接口。  
  
## <a name="syntax"></a>语法  
  
```cpp  
coclass CorpubPublish {  
    [default] interface ICorPublish;  
    interface           ICorPublishProcess;  
    interface           ICorPublishAppDomain;  
    interface           ICorPublishProcessEnum;  
    interface           ICorPublishAppDomainEnum;  
};  
```  
  
## <a name="interfaces"></a>界面  
  
|接口|说明|  
|---------------|-----------------|  
|[ICorPublish 接口](icorpublish-interface.md)|提供用于在这些进程中发布有关进程和应用程序域的信息的方法。|  
|[ICorPublishAppDomain 接口](icorpublishappdomain-interface.md)|表示并提供有关进程中的应用程序域的信息。|  
|[ICorPublishAppDomainEnum 接口](icorpublishappdomainenum-interface.md)|提供遍历进程中当前存在的应用程序域集合的方法。|  
|[ICorPublishProcess 接口](icorpublishprocess-interface.md)|表示在计算机上运行的进程。|  
|[ICorPublishProcessEnum 接口](icorpublishprocessenum-interface.md)|提供遍历计算机上运行的进程集合的方法。|  
  
## <a name="remarks"></a>注解  

 典型的发布方案涉及到需要调试在应用程序域中的计算机上运行的托管代码的开发人员。 宿主环境可能会在一个进程内运行多个应用程序域。 开发人员希望使用图形用户界面或其他一些方法列出计算机上运行的所有进程，并选择特定的进程。 此列表应包括正在运行托管代码的进程中的所有应用程序域。 然后，开发人员可以确定特定的应用程序域，并将调试器附加到该域。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** CorPub .idl  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**  [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试](index.md)
