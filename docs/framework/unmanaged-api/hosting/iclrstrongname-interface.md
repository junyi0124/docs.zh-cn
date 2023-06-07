---
title: ICLRStrongName 接口
ms.date: 03/30/2017
api_name:
- ICLRStrongName
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName
helpviewer_keywords:
- ICLRStrongName interface [.NET Framework hosting]
ms.assetid: 2fac66fd-6b3b-4dbd-8baf-86038bd85526
topic_type:
- apiref
ms.openlocfilehash: 691cc3cf4ec8d036a4de04247f243d99daa887d4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733628"
---
# <a name="iclrstrongname-interface"></a>ICLRStrongName 接口

提供用于对具有强名称的程序集进行签名的基本全局静态函数。 所有 `ICLRStrongName` 方法都返回标准 COM hresult。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetHashFromAssemblyFile 方法](iclrstrongname-gethashfromassemblyfile-method.md)|使用指定的哈希算法获取指定程序集文件的哈希。|  
|[GetHashFromAssemblyFileW 方法](iclrstrongname-gethashfromassemblyfilew-method.md)|使用指定的哈希算法获取指定为 Unicode 字符串的程序集文件的哈希。|  
|[GetHashFromBlob 方法](iclrstrongname-gethashfromblob-method.md)|使用指定的哈希算法获取指定内存地址处的程序集的哈希。|  
|[GetHashFromFile 方法](iclrstrongname-gethashfromfile-method.md)|生成指定文件内容的哈希。|  
|[GetHashFromFileW 方法](iclrstrongname-gethashfromfilew-method.md)|生成由 Unicode 字符串指定的文件内容的哈希。|  
|[GetHashFromHandle 方法](iclrstrongname-gethashfromhandle-method.md)|使用指定的哈希算法，生成具有指定文件句柄的文件内容的哈希。|  
|[StrongNameCompareAssemblies 方法](iclrstrongname-strongnamecompareassemblies-method.md)|确定两个程序集是否仅是强名称签名不同。|  
|[StrongNameFreeBuffer 方法](iclrstrongname-strongnamefreebuffer-method.md)|释放以前对强名称方法（如 [StrongNameGetPublicKey](iclrstrongname-strongnamegetpublickey-method.md)、 [StrongNameTokenFromPublicKey](iclrstrongname-strongnametokenfrompublickey-method.md)或 [StrongNameSignatureGeneration](iclrstrongname-strongnamesignaturegeneration-method.md)）的调用所分配的内存。|  
|[StrongNameGetBlob 方法](iclrstrongname-strongnamegetblob-method.md)|使用指定地址处可执行文件的二进制表示形式填充指定的缓冲区。|  
|[StrongNameGetBlobFromImage 方法](iclrstrongname-strongnamegetblobfromimage-method.md)|获取指定内存地址处程序集映像的二进制表示形式。|  
|[StrongNameGetPublicKey 方法](iclrstrongname-strongnamegetpublickey-method.md)|从私钥/公钥对中获取公钥。|  
|[StrongNameHashSize 方法](iclrstrongname-strongnamehashsize-method.md)|使用指定的哈希算法获取哈希所需的缓冲区大小。|  
|[StrongNameKeyDelete 方法](iclrstrongname-strongnamekeydelete-method.md)|删除指定的密钥容器。|  
|[StrongNameKeyGen 方法](iclrstrongname-strongnamekeygen-method.md)|创建新的公钥/私钥对，以便强名称使用。|  
|[StrongNameKeyGenEx 方法](iclrstrongname-strongnamekeygenex-method.md)|生成具有指定密钥大小的新公钥/私钥对，以便强名称使用。|  
|[StrongNameKeyInstall 方法](iclrstrongname-strongnamekeyinstall-method.md)|将公钥/私钥对导入容器。|  
|[StrongNameSignatureGeneration 方法](iclrstrongname-strongnamesignaturegeneration-method.md)|为指定的程序集生成强名称签名。|  
|[StrongNameSignatureGenerationEx 方法](iclrstrongname-strongnamesignaturegenerationex-method.md)|根据指定标志为指定的程序集生成强名称签名。|  
|[StrongNameSignatureSize 方法](iclrstrongname-strongnamesignaturesize-method.md)|返回强名称签名的大小。|  
|[StrongNameSignatureVerification 方法](iclrstrongname-strongnamesignatureverification-method.md)|获取一个值，该值指示提供的路径中的程序集清单是否包含根据指定标志验证的强名称签名。|  
|[StrongNameSignatureVerificationEx 方法](iclrstrongname-strongnamesignatureverificationex-method.md)|获取一个值，该值指示提供的路径中的程序集清单是否包含强名称签名。|  
|[StrongNameSignatureVerificationFromImage 方法](iclrstrongname-strongnamesignatureverificationfromimage-method.md)|验证已映射到内存的程序集对关联的公钥是否有效。|  
|[StrongNameTokenFromAssembly 方法](iclrstrongname-strongnametokenfromassembly-method.md)|从指定的程序集文件创建强名称令牌。|  
|[StrongNameTokenFromAssemblyEx 方法](iclrstrongname-strongnametokenfromassemblyex-method.md)|从指定的程序集文件创建强名称令牌并返回公钥。|  
|[StrongNameTokenFromPublicKey 方法](iclrstrongname-strongnametokenfrompublickey-method.md)|获取表示公钥的令牌。|  
  
## <a name="remarks"></a>注解  

 可以 `ICLRStrongName` 通过使用和作为参数调用 [ICLRRuntimeInfo：： GetInterface](iclrruntimeinfo-getinterface-method.md) 方法来获取的实例 `CLSID_CLRStrongName` `IID_ICLRStrongName` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载接口](hosting-interfaces.md)
- [承载](index.md)
