---
title: IMetaDataAssemblyEmit::SetAssemblyRefProps 方法
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyEmit.SetAssemblyRefProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyEmit::SetAssemblyRefProps
helpviewer_keywords:
- SetAssemblyRefProps method [.NET Framework metadata]
- IMetaDataAssemblyEmit::SetAssemblyRefProps method [.NET Framework metadata]
ms.assetid: 70a32bf3-9051-4f96-ae87-11356d06a073
topic_type:
- apiref
ms.openlocfilehash: e28659f3c6912489775dd09951610f19e4400942
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672742"
---
# <a name="imetadataassemblyemitsetassemblyrefprops-method"></a>IMetaDataAssemblyEmit::SetAssemblyRefProps 方法

修改指定的 `AssemblyRef` 元数据结构。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetAssemblyRefProps (  
    [in] mdAssemblyRef              ar,  
    [in] const void                 *pbPublicKeyOrToken,  
    [in] ULONG                      cbPublicKeyOrToken,  
    [in] LPCWSTR                    szName,
    [in] const ASSEMBLYMETADATA     *pMetaData,
    [in] const void                 *pbHashValue,  
    [in] ULONG                      cbHashValue,  
    [in] DWORD                      dwAssemblyRefFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `ar`  
 中 `AssemblyRef` 用于指定要修改的元数据结构的元数据标记。  
  
 `pbPublicKeyOrToken`  
 中所引用程序集的发行者的公钥。  
  
 `cbPublicKeyOrToken`  
 中的大小（以字节为单位） `pbPublicKeyOrToken` 。  
  
 `szName`  
 中程序集的用户可读文本名称。  
  
 `pMetaData`  
 中指向 ASSEMBLYMETADATA 实例的指针，其中包含程序集的版本、平台和区域设置信息。  
  
 `pbHashValue`  
 中指向与程序集关联的哈希数据的指针。  
  
 `cbHashValue`  
 中的大小（以字节为单位） `pbHashValue` 。  
  
 `dwAssemblyRefFlags`  
 中 [AssemblyRefFlags](assemblyrefflags-enumeration.md) 值的按位组合，用于指定所引用程序集的特性。  
  
## <a name="remarks"></a>注解  

 若要创建 `AssemblyRef` 元数据结构，请使用 [IMetaDataAssemblyEmit：:D efineassemblyref](imetadataassemblyemit-defineassemblyref-method.md) 方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataAssemblyEmit 接口](imetadataassemblyemit-interface.md)
