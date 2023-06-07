---
title: IMetaDataDispenser::OpenScopeOnMemory 方法
ms.date: 03/30/2017
api_name:
- IMetaDataDispenser.OpenScopeOnMemory
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataDispenser::OpenScopeOnMemory
helpviewer_keywords:
- OpenScopeOnMemory method [.NET Framework metadata]
- IMetaDataDispenser::OpenScopeOnMemory method [.NET Framework metadata]
ms.assetid: 14218249-bdec-48ae-b5fc-9f57f7ca8501
topic_type:
- apiref
ms.openlocfilehash: 26293e38a275ca691c7d48dceb12c1e7dd316536
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95713413"
---
# <a name="imetadatadispenseropenscopeonmemory-method"></a>IMetaDataDispenser::OpenScopeOnMemory 方法

打开包含现有元数据的内存区域。 也就是说，此方法将打开一个指定的内存区域，其中的现有数据将被视为元数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT OpenScopeOnMemory (  
    [in]  LPCVOID     pData,
    [in]  ULONG       cbData,
    [in]  DWORD       dwOpenFlags,
    [in]  REFIID      riid,
    [out] IUnknown    **ppIUnk  
);  
```  
  
## <a name="parameters"></a>参数  

 `pData`  
 中指定内存区域起始地址的指针。  
  
 `cbData`  
 中内存区域的大小（以字节为单位）。  
  
 `dwOpenFlags`  
 中 [CorOpenFlags](coropenflags-enumeration.md) 枚举的一个值，用于指定要打开的模式 (读取、写入等) 。  
  
 `riid`  
 中要返回的所需元数据接口的 IID;调用方将使用接口导入 (读取) 或发出 (写入) 元数据。  
  
 的值 `riid` 必须指定一个 "导入" 或 "发出" 接口。 有效值为 IID_IMetaDataEmit、IID_IMetaDataImport、IID_IMetaDataAssemblyEmit、IID_IMetaDataAssemblyImport、IID_IMetaDataEmit2 或 IID_IMetaDataImport2。  
  
 `ppIUnk`  
 弄指向返回的接口的指针。  
  
## <a name="remarks"></a>注解  

 可以使用 "导入" 接口中的方法查询元数据的内存中副本，或使用 "发出" 接口之一中的方法将其添加到中。  
  
 `OpenScopeOnMemory`方法类似于[IMetaDataDispenser：： OpenScope](imetadatadispenser-openscope-method.md)方法，不同之处在于相关元数据已存在于内存中，而不是位于磁盘上的文件中。  
  
 如果内存的目标区域不包含公共语言运行时 (CLR) 元数据，则该 `OpenScopeOnMemory` 方法将失败。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataDispenser 接口](imetadatadispenser-interface.md)
- [IMetaDataDispenserEx 接口](imetadatadispenserex-interface.md)
- [IMetaDataAssemblyEmit 接口](imetadataassemblyemit-interface.md)
- [IMetaDataAssemblyImport 接口](imetadataassemblyimport-interface.md)
- [IMetaDataEmit 接口](imetadataemit-interface.md)
- [IMetaDataEmit2 接口](imetadataemit2-interface.md)
- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
