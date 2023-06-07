---
title: ICLRStrongName::StrongNameSignatureVerificationFromImage 方法
ms.date: 03/30/2017
api_name:
- ICLRStrongName.StrongNameSignatureVerificationFromImage
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::StrongNameSignatureVerificationFromImage
helpviewer_keywords:
- ICLRStrongName::StrongNameSignatureVerificationFromImage method [.NET Framework hosting]
- StrongNameSignatureVerificationFromImage method, ICLRStrongName interface [.NET Framework hosting]
ms.assetid: da91c138-ee30-4fd4-a040-464d97d7e41a
topic_type:
- apiref
ms.openlocfilehash: 134b32b6b281a08997849f4c23edfc1f357dadcd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95674393"
---
# <a name="iclrstrongnamestrongnamesignatureverificationfromimage-method"></a>ICLRStrongName::StrongNameSignatureVerificationFromImage 方法

验证已映射到内存的程序集对关联的公钥是否有效。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT StrongNameSignatureVerificationFromImage (  
    [in]  BYTE    *pbBase,  
    [in]  DWORD   dwLength,  
    [in]  DWORD   dwInFlags,  
    [out] DWORD   *pdwOutFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `pbBase`  
 中映射的程序集清单的相对虚拟地址。  
  
 `dwLength`  
 中映射的图像的大小（以字节为单位）。  
  
 `dwInFlags`  
 中影响验证行为的标志。 支持以下值：  
  
- `SN_INFLAG_FORCE_VER` (0x00000001) -即使需要重写注册表设置，也强制进行验证。  
  
- `SN_INFLAG_INSTALL` (0x00000002) -指定这是在此映像上执行的第一次验证。  
  
- `SN_INFLAG_ADMIN_ACCESS` (0x00000004) -指定缓存只允许具有管理权限的用户访问。  
  
- `SN_INFLAG_USER_ACCESS` (0x00000008) -指定只能向当前用户访问程序集。  
  
- `SN_INFLAG_ALL_ACCESS` (0x00000010) -指定缓存将不提供访问限制。  
  
- `SN_INFLAG_RUNTIME` (0x80000000) 为内部调试保留。  
  
 `pdwOutFlags`  
 弄用于附加输出信息的标志。 支持以下值：  
  
- `SN_OUTFLAG_WAS_VERIFIED` (0x00000001) -将此值设置为， `false` 以指定验证是否由于注册表设置而成功。  
  
## <a name="return-value"></a>返回值  

 `S_OK` 如果该方法已成功完成，则为;否则，表示失败的 HRESULT 值 (参阅) 列表的 [常见 HRESULT 值](/windows/win32/seccrypto/common-hresult-values) 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRStrongName 接口](iclrstrongname-interface.md)
