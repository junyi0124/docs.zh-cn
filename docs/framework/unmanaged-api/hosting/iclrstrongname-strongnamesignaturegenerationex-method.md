---
title: ICLRStrongName::StrongNameSignatureGenerationEx 方法
ms.date: 03/30/2017
api_name:
- ICLRStrongName.StrongNameSignatureGenerationEx
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::StrongNameSignatureGenerationEx
helpviewer_keywords:
- ICLRStrongName::StrongNameSignatureGenerationEx method [.NET Framework hosting]
- StrongNameSignatureGenerationEx method, ICLRStrongName interface [.NET Framework hosting]
ms.assetid: c3f34584-c6e2-41fd-bb44-e44da8546309
topic_type:
- apiref
ms.openlocfilehash: 78cc043953e6288df136b43590831569d112afef
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95674510"
---
# <a name="iclrstrongnamestrongnamesignaturegenerationex-method"></a>ICLRStrongName::StrongNameSignatureGenerationEx 方法

根据指定的标志，为指定的程序集生成强名称签名。  
  
## <a name="syntax"></a>语法  
  
```cpp
HRESULT StrongNameSignatureGenerationEx (  
    [in]  LPCWSTR   wszFilePath,  
    [in]  LPCWSTR   wszKeyContainer,  
    [in]  BYTE      *pbKeyBlob,  
    [in]  ULONG     cbKeyBlob,  
    [out] BYTE      **ppbSignatureBlob,  
    [out] ULONG     *pcbSignatureBlob,  
    [in]  DWORD     dwFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `wszFilePath`  
 中包含要为其生成强名称签名的程序集清单的文件的路径。  
  
 `wszKeyContainer`  
 中包含公钥/私钥对的密钥容器的名称。  
  
 如果 `pbKeyBlob` 为 null，则 `wszKeyContainer` 必须在加密服务提供程序 (CSP) 中指定有效容器。 在这种情况下，存储在容器中的密钥对用于对文件进行签名。  
  
 如果不为 `pbKeyBlob` null，则假定密钥对包含在关键的二进制大型对象 (BLOB) 中。  
  
 `pbKeyBlob`  
 中指向公钥/私钥对的指针。 此对采用 Win32 函数创建的格式 `CryptExportKey` 。 如果 `pbKeyBlob` 为 null，则假定指定的密钥容器 `wszKeyContainer` 包含密钥对。  
  
 `cbKeyBlob`  
 中的大小（以字节为单位） `pbKeyBlob` 。  
  
 `ppbSignatureBlob`  
 弄指向公共语言运行时返回签名的位置的指针。 如果 `ppbSignatureBlob` 为 null，则运行时将签名存储在指定的文件中 `wszFilePath` 。  
  
 如果 `ppbSignatureBlob` 不为 null，则公共语言运行时将分配要在其中返回签名的空间。 调用方必须使用 [ICLRStrongName：： StrongNameFreeBuffer](iclrstrongname-strongnamefreebuffer-method.md) 方法释放此空间。  
  
 `pcbSignatureBlob`  
 弄返回签名的大小（以字节为单位）。  
  
 `dwFlags`  
 中以下一个或多个值：  
  
- `SN_SIGN_ALL_FILES` (0x00000001) -重新计算链接模块的所有哈希。  
  
- `SN_TEST_SIGN` (0x00000002) -对程序集进行测试签名。  
  
## <a name="return-value"></a>返回值  

 `S_OK` 如果该方法已成功完成，则为;否则，表示失败的 HRESULT 值 (参阅) 列表的 [常见 HRESULT 值](/windows/win32/seccrypto/common-hresult-values) 。  
  
## <a name="remarks"></a>注解  

 指定 null 以 `wszFilePath` 计算签名大小，而不创建签名。  
  
 签名可以直接存储在文件中，也可以返回给调用方。  
  
 如果 `SN_SIGN_ALL_FILES` 指定了，但没有包含公钥，则 (`pbKeyBlob` 和 `wszFilePath` 均为 null) ，则会重新计算链接模块的哈希值，但不会对程序集进行重新签名。  
  
 如果 `SN_TEST_SIGN` 指定了，则不会修改公共语言运行时标头以指示程序集使用强名称进行签名。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameSignatureGeneration 方法](iclrstrongname-strongnamesignaturegeneration-method.md)
- [ICLRStrongName 接口](iclrstrongname-interface.md)
