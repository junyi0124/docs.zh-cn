---
title: IMetaDataAssemblyEmit::DefineAssembly 方法
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyEmit.DefineAssembly
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyEmit::DefineAssembly
helpviewer_keywords:
- IMetaDataAssemblyEmit::DefineAssembly method [.NET Framework metadata]
- DefineAssembly method [.NET Framework metadata]
ms.assetid: a0637d66-74bf-4f2d-8137-9ff838bccece
topic_type:
- apiref
ms.openlocfilehash: 6d783e27c60db7381025f3b2382728e3996323ae
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725724"
---
# <a name="imetadataassemblyemitdefineassembly-method"></a>IMetaDataAssemblyEmit::DefineAssembly 方法

创建 `Assembly` 包含指定程序集的元数据的结构，并返回关联的元数据标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DefineAssembly (  
    [in]  void                 *pbPublicKey,  
    [in]  ULONG                cbPublicKey,  
    [in]  ULONG                uHashAlgId,  
    [in]  LPCWSTR              szName,
    [in]  ASSEMBLYMETADATA     *pMetaData,  
    [in]  DWORD                dwAssemblyFlags,  
    [out] mdAssembly           *pmda  
);  
```  
  
## <a name="parameters"></a>参数  

 `pbPublicKey`  
 中标识程序集发行者的公钥; 如果程序集没有强名称，则为 NULL。  
  
 `cbPublicKey`  
 中的大小（以字节为单位） `pbPublicKey` 。  
  
 `uHashAlgId`  
 中用于对程序集中的文件进行加密的哈希算法的标识符; 如果为 NULL，则指定 SHA-1 算法。  
  
 `szName`  
 中程序集的用户可读文本名称。 此值不能超过1024个字符。  
  
 `pMetaData`  
 中指向 ASSEMBLYMETADATA 实例的指针，其中包含程序集的版本、平台和区域设置信息。  
  
 `dwAssemblyFlags`  
 中 [CorAssemblyFlags](corassemblyflags-enumeration.md) 值的组合，用于描述程序集的功能。  
  
 `pmda`  
 弄指向元数据标记的指针。  
  
## <a name="remarks"></a>注解  

 `Assembly`清单中只能定义一个元数据结构。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataAssemblyEmit 接口](imetadataassemblyemit-interface.md)
