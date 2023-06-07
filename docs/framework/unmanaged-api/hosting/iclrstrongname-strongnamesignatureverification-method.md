---
title: ICLRStrongName::StrongNameSignatureVerification 方法
ms.date: 03/30/2017
api_name:
- ICLRStrongName.StrongNameSignatureVerification
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::StrongNameSignatureVerification
helpviewer_keywords:
- ICLRStrongName::StrongNameSignatureVerification method [.NET Framework hosting]
- StrongNameSignatureVerification method, ICLRStrongName interface [.NET Framework hosting]
ms.assetid: 734dc4d1-0a76-4736-b5ac-cb4253b3dd49
topic_type:
- apiref
ms.openlocfilehash: 2d53eebcc272ab87a2af5b3c081ca37dde5c74b9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95674458"
---
# <a name="iclrstrongnamestrongnamesignatureverification-method"></a>ICLRStrongName::StrongNameSignatureVerification 方法

获取一个值，该值指示所提供的路径处的程序集清单是否包含强名称签名，该签名根据指定的标志进行验证。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT StrongNameSignatureVerification (  
    [in]  LPCWSTR   wszFilePath,  
    [in]  DWORD     dwInFlags,  
    [out] DWORD     *pdwOutFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `wszFilePath`  
 中要验证的程序集的可移植可执行文件 ( .dll 或 .exe) 文件的路径。  
  
 `dwInFlags`  
 中用于修改验证行为的标志。 支持以下值：  
  
- `SN_INFLAG_FORCE_VER` (0x00000001) -即使需要重写注册表设置，也强制进行验证。  
  
- `SN_INFLAG_INSTALL` (0x00000002) -指定这是第一次验证清单。  
  
- `SN_INFLAG_ADMIN_ACCESS` (0x00000004) -指定缓存只允许具有管理权限的用户访问。  
  
- `SN_INFLAG_USER_ACCESS` (0x00000008) -指定只能向当前用户访问程序集。  
  
- `SN_INFLAG_ALL_ACCESS` (0x00000010) -指定缓存将不提供访问限制。  
  
- `SN_INFLAG_RUNTIME` (0x80000000) 为内部调试保留。  
  
 `pdwOutFlags`  
 弄指示强名称签名是否已验证的标志。 支持以下值：  
  
- `SN_OUTFLAG_WAS_VERIFIED` (0x00000001) -将此值设置为， `false` 以指定验证是否由于注册表设置而成功。  
  
## <a name="return-value"></a>返回值  

 `S_OK` 如果该方法已成功完成，则为;否则，表示失败的 HRESULT 值 (参阅) 列表的 [常见 HRESULT 值](/windows/win32/seccrypto/common-hresult-values) 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameSignatureVerificationEx 方法](iclrstrongname-strongnamesignatureverificationex-method.md)
- [ICLRStrongName 接口](iclrstrongname-interface.md)
