---
title: IMetaDataInfo::GetFileMapping 方法
ms.date: 03/30/2017
api_name:
- IMetaDataInfo.GetFileMapping
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataInfo::GetFileMapping
helpviewer_keywords:
- IMetaDataInfo::GetFileMapping method [.NET Framework metadata]
- GetFileMapping method [.NET Framework metadata]
ms.assetid: 2868dfec-c992-4606-88bb-a8e0b6b18271
topic_type:
- apiref
ms.openlocfilehash: 8823f3cc016072d3f20100c29532459da5e97492
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95682384"
---
# <a name="imetadatainfogetfilemapping-method"></a>IMetaDataInfo::GetFileMapping 方法

获取映射文件的内存区域和映射类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetFileMapping (  
    [out] const void           **ppvData,
    [out] ULONGLONG            *pcbData,
    [out] DWORD                *pdwMappingType  
);  
```  
  
## <a name="parameters"></a>参数  

 `ppvData`  
 弄指向映射文件的开头的指针。  
  
 `pcbData`  
 弄映射区域的大小。 如果 `pdwMappingType` 为 `fmFlat` ，则这是文件的大小。  
  
 `pdwMappingType`  
 弄指示映射类型的 [CorFileMapping](corfilemapping-enumeration.md) 值。 公共语言运行时 (CLR) 的当前实现始终返回 `fmFlat` 。 保留其他值以供将来使用。 但是，应始终验证返回的值，因为在将来的版本或服务版本中可能会启用其他值。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|`S_OK`|所有输出都已填充。|  
|`E_INVALIDARG`|将 NULL 作为参数值传递。|  
|`COR_E_NOTSUPPORTED`|CLR 实现无法提供有关内存区域的信息。 这可能是由以下原因引起的：<br /><br /> -元数据范围已用 `ofWrite` 或标记打开 `ofCopyMemory` 。<br />-元数据范围在未带标志的情况下打开 `ofReadOnly` 。<br />- [IMetaDataDispenser：： OpenScopeOnMemory](imetadatadispenser-openscopeonmemory-method.md) 方法仅用于打开文件的元数据部分。<br />-此文件不是可移植的可执行文件 (PE) 文件。 **注意：**  这些条件依赖于 CLR 实现，并且可能会在 CLR 的未来版本中减弱。|  
  
## <a name="remarks"></a>注解  

 `ppvData`仅当基础元数据范围打开时，指向的内存才有效。  
  
 为了使此方法正常工作，当你通过调用 [IMetaDataDispenser：： OpenScope](imetadatadispenser-openscope-method.md) 方法将磁盘上的文件的元数据映射到内存中时，你必须指定 `ofReadOnly` 标志，并且不得指定 `ofWrite` 或 `ofCopyMemory` 标志。  
  
 每个作用域的文件映射类型选择特定于 CLR 的给定实现。 它不能由用户设置。 CLR 的当前实现始终返回 `fmFlat` `pdwMappingType` ，但这可能会在将来版本的 clr 或未来版本的特定版本中发生更改。 应始终在中检查返回的值 `pdwMappingType` ，因为不同的类型将具有不同的布局和偏移量。  
  
 不支持为这三个参数中的任何一个传递 NULL。 方法返回 `E_INVALIDARG` ，但不填充任何输出。 忽略映射类型或区域大小可能会导致程序终止异常。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataInfo 接口](imetadatainfo-interface.md)
- [CorFileMapping 枚举](corfilemapping-enumeration.md)
