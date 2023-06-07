---
title: ICLRDataTarget3::GetExceptionRecord 方法
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ICLRDataTarget3.GetExceptionRecord
api_location:
- mscordbi.dll
api_type:
- COM
ms.assetid: 6643c2af-2ee6-4789-aa25-1d8eaf500c94
topic_type:
- apiref
ms.openlocfilehash: 8f6eaa6ad310e9a01b2307bff091b670c3e1d6cb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723605"
---
# <a name="iclrdatatarget3getexceptionrecord-method"></a>ICLRDataTarget3::GetExceptionRecord 方法

由公共语言运行时 (CLR) 数据访问服务调用，以检索与目标进程关联的异常记录。 例如，对于转储目标，此操作等效于通过 `ExceptionParam` Windows 调试帮助库中的 [MiniDumpWriteDump](/windows/desktop/api/minidumpapiset/nf-minidumpapiset-minidumpwritedump) 函数的参数传入的异常记录 (dbghelp.dll) 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetExceptionRecord(  
    [in] ULONG32 bufferSize,  
    [out] ULONG32* bufferUsed,  
    [out, size_is(bufferSize] BYTE* buffer  
);  
```  
  
## <a name="parameters"></a>参数  

 `bufferSize`  
 [in] 输入缓冲区大小（以字节为单位）。 这必须等于 `sizeof(` [MINIDUMP_EXCEPTION](/windows/win32/api/minidumpapiset/ns-minidumpapiset-minidump_exception) `)` 。  
  
 `bufferUsed`  
 [out] 指向接收实际写入缓冲区的字节数的 `ULONG32` 类型的指针。  
  
 `buffer`  
 [out] 指向接收异常记录副本的内存缓冲区的指针。 异常记录作为 [MINIDUMP_EXCEPTION](/windows/win32/api/minidumpapiset/ns-minidumpapiset-minidump_exception) 类型返回。  
  
## <a name="return-value"></a>返回值  

 如果成功，则返回值是 `S_OK`；如果失败，则返回失败 `HRESULT` 代码。 `HRESULT` 代码可以包括但不限于以下代码：  
  
|返回代码|描述|  
|-----------------|-----------------|  
|`S_OK`|方法成功。 已将异常记录复制到输出缓冲区。|  
|`HRESULT_FROM_WIN32(ERROR_NOT_FOUND)`|没有与目标关联的异常记录。|  
|`HRESULT_FROM_WIN32(ERROR_BAD_LENGTH)`|输入缓冲区大小不等于 `sizeof(MINIDUMP_EXCEPTION)`。|  
  
## <a name="remarks"></a>注解  

 [MINIDUMP_EXCEPTION](/windows/win32/api/minidumpapiset/ns-minidumpapiset-minidump_exception) 是在 Windows SDK 中的 dbghelp.dll 和 imagehlp.dll 中定义的结构。  
  
 此方法由调试应用程序的编写器实现。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** ClrData，ClrData  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[v451_update](../../../../includes/net-current-v451-nov-plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRDataTarget3 接口](iclrdatatarget3-interface.md)
- [GetExceptionContextRecord 方法](iclrdatatarget3-getexceptioncontextrecord-method.md)
- [GetExceptionThreadID 方法](iclrdatatarget3-getexceptionthreadid-method.md)
