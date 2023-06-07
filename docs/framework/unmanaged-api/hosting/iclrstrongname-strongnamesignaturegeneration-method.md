---
title: ICLRStrongName::StrongNameSignatureGeneration 方法
ms.date: 03/30/2017
api_name:
- ICLRStrongName.StrongNameSignatureGeneration
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::StrongNameSignatureGeneration
helpviewer_keywords:
- StrongNameSignatureGeneration method, ICLRStrongName interface [.NET Framework hosting]
- ICLRStrongName::StrongNameSignatureGeneration method [.NET Framework hosting]
ms.assetid: 4cdb1284-947a-4ed4-94c1-c5ff5cdfce56
topic_type:
- apiref
ms.openlocfilehash: 9fc517b081a1df48d943d03a9c3ce223a428bde7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95671624"
---
# <a name="iclrstrongnamestrongnamesignaturegeneration-method"></a>ICLRStrongName::StrongNameSignatureGeneration 方法

为指定的程序集生成强名称签名。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT StrongNameSignatureGeneration (
    [in]  LPCWSTR   wszFilePath,  
    [in]  LPCWSTR   wszKeyContainer,  
    [in]  BYTE      *pbKeyBlob,  
    [in]  ULONG     cbKeyBlob,  
    [out] BYTE      **ppbSignatureBlob,  
    [out] ULONG     *pcbSignatureBlob  
);  
```  
  
## <a name="parameters"></a>参数  

 `wszFilePath`  
 中包含要为其生成强名称签名的程序集清单的文件的路径。  
  
 `wszKeyContainer`  
 中包含公钥/私钥对的密钥容器的名称。  
  
 如果 `pbKeyBlob` 为 null，则 `wszKeyContainer` 必须在加密服务提供程序 (CSP) 中指定有效容器。 在这种情况下，存储在容器中的密钥对用于对文件进行签名。  
  
 如果不为 `pbKeyBlob` null，则假定密钥对包含在关键的二进制大型对象 (BLOB) 中。  
  
 密钥必须是1024位 Rivest-Rivest-shamir-adleman (RSA) 签名密钥。 此时不支持其他类型的密钥。  
  
 `pbKeyBlob`  
 中指向公钥/私钥对的指针。 此对采用 Win32 函数创建的格式 `CryptExportKey` 。 如果 `pbKeyBlob` 为 null，则假定指定的密钥容器 `wszKeyContainer` 包含密钥对。  
  
 `cbKeyBlob`  
 中的大小（以字节为单位） `pbKeyBlob` 。  
  
 `ppbSignatureBlob`  
 弄指向公共语言运行时返回签名的位置的指针。 如果 `ppbSignatureBlob` 为 null，则运行时将签名存储在指定的文件中 `wszFilePath` 。  
  
 如果 `ppbSignatureBlob` 不为 null，则公共语言运行时将分配要在其中返回签名的空间。 调用方必须通过使用 [ICLRStrongName：： StrongNameFreeBuffer](iclrstrongname-strongnamefreebuffer-method.md) 方法释放此空间。  
  
 `pcbSignatureBlob`  
 弄返回签名的大小（以字节为单位）。  
  
## <a name="return-value"></a>返回值  

 `S_OK` 如果该方法已成功完成，则为;否则，表示失败的 HRESULT 值 (参阅) 列表的 [常见 HRESULT 值](/windows/win32/seccrypto/common-hresult-values) 。  
  
## <a name="remarks"></a>注解  

 指定 null 以 `wszFilePath` 计算签名大小，而不创建签名。  
  
 签名可以直接存储在文件中，也可以返回给调用方。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameSignatureGenerationEx 方法](iclrstrongname-strongnamesignaturegenerationex-method.md)
- [ICLRStrongName 接口](iclrstrongname-interface.md)
