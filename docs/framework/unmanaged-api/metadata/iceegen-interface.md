---
title: ICeeGen 接口
ms.date: 03/30/2017
api_name:
- ICeeGen
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICeeGen
helpviewer_keywords:
- ICeeGen interface [.NET Framework metadata]
ms.assetid: 383d20b0-efe9-4e71-8fb8-1186b2e7d0c0
topic_type:
- apiref
ms.openlocfilehash: 2c180a135608350b0feec3f419be98f4f428b186
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95704508"
---
# <a name="iceegen-interface"></a>ICeeGen 接口

提供用于动态代码编译的方法。  
  
 此接口已过时，不应使用。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[AddSectionReloc 方法](iceegen-addsectionreloc-method.md)|已过时。 将 .reloc 指令添加到基本代码。|  
|[AllocateMethodBuffer 方法](iceegen-allocatemethodbuffer-method.md)|已过时。 为方法创建指定大小的缓冲区，并获取该方法的相对虚拟地址。|  
|[ComputePointer 方法](iceegen-computepointer-method.md)|已过时。 确定指定代码节的缓冲区。|  
|[EmitString 方法](iceegen-emitstring-method.md)|已过时。 向基本代码发出指定的字符串。|  
|[GenerateCeeFile 方法](iceegen-generateceefile-method.md)|已过时。 生成一个代码基文件，其中包含当前加载到此中的代码库 `ICeeGen` 。|  
|[GenerateCeeMemoryImage 方法](iceegen-generateceememoryimage-method.md)|已过时。 为基本代码在内存中生成一个图像。|  
|[GetIlSection 方法](iceegen-getilsection-method.md)|已过时。 获取由指定句柄引用的中间语言代码库的部分。|  
|[GetIMapTokenIface 方法](iceegen-getimaptokeniface-method.md)|已过时。 获取指定的标记所引用的接口。|  
|[GetMethodBuffer 方法](iceegen-getmethodbuffer-method.md)|已过时。 获取指定的相对虚拟地址处的方法的适当大小的缓冲区。|  
|[GetSectionBlock 方法](iceegen-getsectionblock-method.md)|已过时。 获取代码库的节块。|  
|[GetSectionCreate 方法](iceegen-getsectioncreate-method.md)|已过时。 使用指定的名称和标志值生成并获取代码部分。|  
|[GetSectionDataLen 方法](iceegen-getsectiondatalen-method.md)|已过时。 获取指定节的长度。|  
|[GetString 方法](iceegen-getstring-method.md)|已过时。 获取存储在指定的相对虚拟地址的字符串。|  
|[GetStringSection 方法](iceegen-getstringsection-method.md)|已过时。 获取由指定句柄引用的代码段的字符串表示形式。|  
|[TruncateSection 方法](iceegen-truncatesection-method.md)|已过时。 按指定长度截断指定的代码段。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据接口](metadata-interfaces.md)
