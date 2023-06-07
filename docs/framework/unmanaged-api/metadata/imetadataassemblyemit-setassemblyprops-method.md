---
title: IMetaDataAssemblyEmit::SetAssemblyProps 方法
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyEmit.SetAssemblyProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyEmit::SetAssemblyProps
helpviewer_keywords:
- SetAssemblyProps method [.NET Framework metadata]
- IMetaDataAssemblyEmit::SetAssemblyProps method [.NET Framework metadata]
ms.assetid: 91b633d7-9e75-43c3-a8d2-2144984e5f9e
topic_type:
- apiref
ms.openlocfilehash: 3736e7279056e015b157758b1233cf6dc5aa6d8d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720199"
---
# <a name="imetadataassemblyemitsetassemblyprops-method"></a>IMetaDataAssemblyEmit::SetAssemblyProps 方法

修改指定的 `Assembly` 元数据结构。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetAssemblyProps (  
    [in] mdAssembly               pma,  
    [in] const void               *pbPublicKey,  
    [in] ULONG                    cbPublicKey,  
    [in] ULONG                    ulHashAlgId,  
    [in] LPCWSTR                  szName,  
    [in] const ASSEMBLYMETADATA   *pMetaData,  
    [in] DWORD                    dwAssemblyFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `pma`  
 中 `Assembly` 用于指定要修改的元数据结构的元数据标记。  
  
 `pbPublicKey`  
 中指向程序集发布者的公钥的指针。  
  
 `cbPublicKey`  
 中的大小（以字节为单位） `pbPublicKey` 。  
  
 `ulHashAlgId`  
 中用于对程序集文件进行哈希处理的哈希算法的标识符。  
  
 `szName`  
 中程序集的用户可读文本名称。  
  
 `pMetaData`  
 中指向包含程序集的版本、平台和区域设置信息的 ASSEMBLYMETADATA 的指针。  
  
 `dwAssemblyFlags`  
 中 [AssemblyFlags](assemblyflags-enumeration.md) 值的按位组合，用于指定程序集的各种属性。  
  
## <a name="remarks"></a>注解  

 若要创建 `Assembly` 元数据结构，请使用 [IMetaDataAssemblyEmit：:D efineassembly](imetadataassemblyemit-defineassembly-method.md) 方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataAssemblyEmit 接口](imetadataassemblyemit-interface.md)
