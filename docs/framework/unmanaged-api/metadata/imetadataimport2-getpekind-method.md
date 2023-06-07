---
title: IMetaDataImport2::GetPEKind 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport2.GetPEKind
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport2::GetPEKind
helpviewer_keywords:
- GetPEKind method [.NET Framework metadata]
- IMetaDataImport2::GetPEKind method [.NET Framework metadata]
ms.assetid: d91c3d89-8022-4a4c-a2a2-a8af2c387507
topic_type:
- apiref
ms.openlocfilehash: d335beecc12e0c1c895e42888ad7172f78062ff7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95702532"
---
# <a name="imetadataimport2getpekind-method"></a>IMetaDataImport2::GetPEKind 方法

获取一个值，该值标识可移植可执行文件中代码的性质 (PE) 文件（通常为 DLL 或 EXE 文件），该文件在当前的元数据范围内定义。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetPEKind (  
   [out] DWORD *pdwPEKind,  
   [out] DWORD *pdwMachine  
);  
```  
  
## <a name="parameters"></a>参数  

 `pdwPEKind`  
 弄一个指针，指向描述 PE 文件的 [CorPEKind](corpekind-enumeration.md) 枚举的值。  
  
 `pdwMachine`  
 弄一个指针，指向用于标识计算机体系结构的值。 有关可能的值，请参阅下一节。  
  
## <a name="remarks"></a>注解  

 参数引用的值 `pdwMachine` 可以是下列值之一。  
  
|Value|计算机体系结构|  
|-----------|--------------------------|  
|IMAGE_FILE_MACHINE_I386<br /><br /> 0x014C|x86|  
|IMAGE_FILE_MACHINE_IA64<br /><br /> 0x0200|Intel IPF|  
|IMAGE_FILE_MACHINE_AMD64<br /><br /> 0x8664|X64|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport2 接口](imetadataimport2-interface.md)
- [IMetaDataImport 接口](imetadataimport-interface.md)
- [CorPEKind 枚举](corpekind-enumeration.md)
