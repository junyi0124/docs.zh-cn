---
title: IMetaDataDispenser::OpenScope 方法
ms.date: 03/30/2017
api_name:
- IMetaDataDispenser.OpenScope
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataDispenser::OpenScope
helpviewer_keywords:
- IMetaDataDispenser::OpenScope method [.NET Framework metadata]
- OpenScope method, IMetaDataDispenser interface [.NET Framework metadata]
ms.assetid: 65063ad5-e0d9-4c01-8f8b-9a5950109fa6
topic_type:
- apiref
ms.openlocfilehash: f2a85bafc3e2f25b2ed6116a46a9938d869dbaae
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726075"
---
# <a name="imetadatadispenseropenscope-method"></a>IMetaDataDispenser::OpenScope 方法

打开现有的磁盘上的文件，并将其元数据映射到内存。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT OpenScope (  
    [in]  LPCWSTR     szScope,
    [in]  DWORD       dwOpenFlags,
    [in]  REFIID      riid,
    [out] IUnknown    **ppIUnk  
);  
```  
  
## <a name="parameters"></a>参数  

 `szScope`  
 中要打开的文件的名称。 文件必须包含公共语言运行时 (CLR) 元数据。  
  
 `dwOpenFlags`  
 中 [CorOpenFlags](coropenflags-enumeration.md) 枚举的一个值，用于指定要打开的模式 (读取、写入等) 。  
  
 `riid`  
 中要返回的所需元数据接口的 IID;调用方将使用接口导入 (读取) 或发出 (写入) 元数据。  
  
 的值 `riid` 必须指定一个 "导入" 或 "发出" 接口。 有效值为 IID_IMetaDataEmit、IID_IMetaDataImport、IID_IMetaDataAssemblyEmit、IID_IMetaDataAssemblyImport、IID_IMetaDataEmit2 或 IID_IMetaDataImport2。  
  
 `ppIUnk`  
 弄指向返回的接口的指针。  
  
## <a name="remarks"></a>注解  

 可以使用 "导入" 接口中的方法查询元数据的内存中副本，或使用 "发出" 接口之一中的方法将其添加到中。  
  
 如果目标文件不包含 CLR 元数据，则该 `OpenScope` 方法将失败。  
  
 在 .NET Framework 版本1.0 和1.1 版中，如果在 `dwOpenFlags` 设置为 ofRead 的情况下打开范围，则它有资格进行共享。 也就是说，如果后续调用 `OpenScope` 传入以前打开的文件的名称，则将重用现有范围并不创建新的数据结构集。 但是，这种共享可能导致问题。  
  
 在 .NET Framework 版本2.0 中， `dwOpenFlags` 将不再共享用设置为 ofRead 打开的作用域。 使用 ofReadOnly 值允许共享作用域。 共享作用域时，使用 "读/写" 元数据接口的查询会失败。  
  
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
