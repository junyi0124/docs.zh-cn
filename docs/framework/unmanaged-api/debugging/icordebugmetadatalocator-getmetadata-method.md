---
title: ICorDebugMetaDataLocator::GetMetaData 方法
ms.date: 03/30/2017
api_name:
- ICorDebugMetaDataLocator.GetMetaData
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugMetaDataLocator::GetMetaData
helpviewer_keywords:
- ICorDebugMetaDataLocator::GetMetaData method [.NET Framework debugging]
- GetMetaData method, ICorDebugMetaDataLocator interface [.NET Framework debugging]
ms.assetid: f9b0ff22-54db-45eb-9cc3-508000a3141d
topic_type:
- apiref
ms.openlocfilehash: 63efb788d8bca84da94921371309704cc7b20ac4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95710436"
---
# <a name="icordebugmetadatalocatorgetmetadata-method"></a>ICorDebugMetaDataLocator::GetMetaData 方法

要求调试器返回模块（完成该调试器请求的操作需要其元数据）的完整路径。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetMetaData(  
      [in] LPCWSTR wszImagePath,  
      [in] DWORD   dwImageTimeStamp,  
      [in] DWORD   dwImageSize,  
      [in] ULONG32 cchPathBuffer,  
      [out] ULONG32 * pcchPathBuffer,  
      [out, size_is(cchPathBuffer), length_is(*pcchPathBuffer)]  
               WCHAR wszPathBuffer[]  
      );  
```  
  
## <a name="parameters"></a>参数  

 `wszImagePath`  
 [in] 以 null 结尾的字符串，表示文件的完整路径。 如果未提供完整路径，则文件的名称和扩展名 (*filename*。*扩展*) 。  
  
 `dwImageTimeStamp`  
 [in] 来自图像 PE 文件头的时间戳。 此参数可能用于符号服务器 ([SymSrv](/windows/desktop/debug/using-symsrv)) lookup。  
  
 `dwImageSize`  
 [in] PE 文件头中的图像大小。 此参数可能可以用于 SymSrv 查找。  
  
 `cchPathBuffer`  
 [in] `wszPathBuffer` 中的字符计数。  
  
 `pcchPathBuffer`  
 [out] 写入 `wszPathBuffer` 的 `WCHAR` 的计数。  
  
 如果该方法返回 E_NOT_SUFFICIENT_BUFFER，则包含存储路径所需的 `WCHAR` 计数。  
  
 `wszPathBuffer`  
 [out] 指向一个缓冲区的指针，调试器会将包含请求的元数据的文件的完整路径复制到该缓冲区中。  
  
 `ofReadOnly` [CorOpenFlags](../metadata/coropenflags-enumeration.md)枚举中的标志用于请求对此文件中元数据的只读访问。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。 所有其他失败的 HRESULT 均指示文件不可检索。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。 `wszPathBuffer` 包含文件的完整路径，以 null 结尾。|  
|E_NOT_SUFFICIENT_BUFFER|`wszPathBuffer` 的当前大小不足以容纳完整路径。 在这种情况下，`pcchPathBuffer` 包含所需的 `WCHAR` 计数（包括终止 null 字符），并且使用请求的缓冲区大小第二次调用 `GetMetaData`。|  
  
## <a name="remarks"></a>注解  

 如果 `wszImagePath` 包含转储中模块的完整路径，则它从收集转储的计算机指定路径。 文件可能不位于此位置，或者具有相同名称的不正确文件可能存储在该路径上。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugThread4 接口](icordebugthread4-interface.md)
- [调试接口](debugging-interfaces.md)
- [调试](index.md)
