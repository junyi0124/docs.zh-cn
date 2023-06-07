---
title: ICLRDebuggingLibraryProvider::ProvideLibrary 方法
ms.date: 03/30/2017
api_name:
- ICLRDebuggingLibraryProvider.ProvideLibrary Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRDebuggingLibraryProvider::ProvideLibrary
helpviewer_keywords:
- ProvideLibrary method [.NET Framework debugging]
- ICLRDebuggingLibraryProvider::ProvideLibrary method [.NET Framework debugging]
ms.assetid: 86f06245-9517-49be-8d8c-ca5deaf34c02
topic_type:
- apiref
ms.openlocfilehash: 7bbb49dc6ee9b1d29dd61ccdcfdacb62740133ed
ms.sourcegitcommit: d9c7ac5d06735a01c1fafe34efe9486734841a72
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2020
ms.locfileid: "82860274"
---
# <a name="iclrdebugginglibraryproviderprovidelibrary-method"></a>ICLRDebuggingLibraryProvider::ProvideLibrary 方法

获取一个库提供程序回调接口，该接口允许根据需要定位和加载特定于公共语言运行时（CLR）版本的调试库。

## <a name="syntax"></a>语法

```cpp
HRESULT ProvideLibrary(
     [in] const WCHAR* pwszFileName,
     [in] DWORD dwTimestamp,
     [in] DWORD dwSizeOfImage,
     [out] HMODULE* hModule);
```

## <a name="parameters"></a>参数

`pwszFilename` \
中请求的模块的名称。

`dwTimestamp` \
中存储在 PE 文件的 COFF 文件头中的日期时间戳。

`pLibraryProvider` \
中存储`SizeOfImage`在 PE 文件的 COFF 可选文件头中的字段。

`hModule` \
弄请求的模块的句柄。

## <a name="return-value"></a>返回值

此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。

|HRESULT|说明|
|-------------|-----------------|
|S_OK|该方法已成功完成。|

## <a name="exceptions"></a>异常

## <a name="remarks"></a>备注

`ProvideLibrary`允许调试器提供调试特定 CLR 文件（如 mscordbi.dll 和 mscordacwks）所需的模块。 模块句柄必须保持有效，直到对[ICLRDebugging：： CanUnloadNow](iclrdebugging-canunloadnow-method.md)方法的调用指示它们可能已被释放，此时调用方负责释放句柄。

调试器可以使用任何可用的方法来查找或获取调试模块。

> [!IMPORTANT]
> 此功能允许 API 调用方提供包含可执行文件的模块，并提供可能的恶意代码。 作为一种安全预防措施，调用方不`ProvideLibrary`应使用分发任何不愿意自行执行的代码。
>
> 如果在已发布的库（如 mscordbi.dll 或 mscordacwks）中发现了严重的安全问题，则可以对填充程序进行修补，以识别文件的不正确版本。 然后，填充程序可以为文件的已修补版本发出请求，并拒绝不正确的版本（如果提供这些文件以响应任何请求）。 仅当用户已修补新的填充码版本时，才会发生这种情况。 未修补的版本将仍然容易受到攻击。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头**：CorDebug.idl、CorDebug.h

**库：** CorGuids.lib

**.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]

## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
