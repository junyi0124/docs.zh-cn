---
title: IMetaDataAssemblyEmit::SetFileProps 方法
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyEmit.SetFileProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyEmit::SetFileProps
helpviewer_keywords:
- IMetaDataAssemblyEmit::SetFileProps method [.NET Framework metadata]
- SetFileProps method [.NET Framework metadata]
ms.assetid: 85667d38-611c-45a9-938d-930ac7a7b681
topic_type:
- apiref
ms.openlocfilehash: 9cf5f3d926c1e742dd9134e7bf292df53e1a4909
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720173"
---
# <a name="imetadataassemblyemitsetfileprops-method"></a>IMetaDataAssemblyEmit::SetFileProps 方法

修改指定的 `File` 元数据结构。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetFileProps (  
    [in] mdFile        file,  
    [in] const void    *pbHashValue,
    [in] ULONG         cbHashValue,  
    [in] DWORD         dwFileFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `file`  
 中 `File` 用于指定要修改的元数据结构的元数据标记。  
  
 `pbHashValue`  
 中指向与该文件关联的哈希数据的指针。  
  
 `cbHashValue`  
 中的大小（以字节为单位） `pbHashValue` 。  
  
 `dwFileFlags`  
 中 [CorFileFlags](corfileflags-enumeration.md) 值的按位组合，用于指定文件的各种属性。  
  
## <a name="remarks"></a>注解  

 若要创建 `File` 元数据结构，请使用 [IMetaDataAssemblyEmit：:D efinefile](imetadataassemblyemit-definefile-method.md) 方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataAssemblyEmit 接口](imetadataassemblyemit-interface.md)
