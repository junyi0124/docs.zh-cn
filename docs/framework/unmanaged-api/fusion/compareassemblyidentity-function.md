---
title: CompareAssemblyIdentity 函数
ms.date: 03/30/2017
api_name:
- CompareAssemblyIdentity
api_location:
- fusion.dll
- clr.dll
api_type:
- COM
f1_keywords:
- CompareAssemblyIdentity
helpviewer_keywords:
- CompareAssemblyIdentity function [.NET Framework fusion]
ms.assetid: 8b364ae1-8efa-4744-a7da-81fd093d84d6
topic_type:
- apiref
ms.openlocfilehash: da32ce6a40378a6f88cf71bd7707be2079d71068
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95717586"
---
# <a name="compareassemblyidentity-function"></a>CompareAssemblyIdentity 函数

比较两个程序集标识以确定它们是否等效。  
  
## <a name="syntax"></a>语法  
  
```cpp  
STDAPI CompareAssemblyIdentity (  
    [in]  LPCWSTR                  pwzAssemblyIdentity1,  
    [in]  BOOL                     fUnified1,  
    [in]  LPCWSTR                  pwzAssemblyIdentity2,  
    [in]  BOOL                     fUnified2,  
    [out] BOOL                     *pfEquivalent,  
    [out] AssemblyComparisonResult *pResult  
 );  
```  
  
## <a name="parameters"></a>参数  

 `pwzAssemblyIdentity1`  
 中比较中第一个程序集的文本标识。  
  
 `fUnified1`  
 中指示的用户指定的统一的布尔标志 `pwzAssemblyIdentity1` 。  
  
 `pwzAssemblyIdentity2`  
 中比较中第二个程序集的文本标识。  
  
 `fUnified2`  
 中指示的用户指定的统一的布尔标志 `pwzAssemblyIdentity2` 。  
  
 `pfEquivalent`  
 弄指示两个程序集是否等效的布尔型标志。  
  
 `pResult`  
 弄 [AssemblyComparisonResult](assemblycomparisonresult-enumeration.md) 枚举，其中包含有关比较的详细信息。  
  
## <a name="return-value"></a>返回值  

 `pfEquivalent` 返回一个布尔值，该值指示两个程序集是否等效。 `pResult` 返回 `AssemblyComparisonResult` 值之一，以便为的值提供更详细的原因 `pfEquivalent` 。  
  
## <a name="remarks"></a>注解  

 `CompareAssemblyIdentity` 检查 `pwzAssemblyIdentity1` 和是否 `pwzAssemblyIdentity2` 等效。 `pfEquivalent``true`在以下一个或多个条件下，设置为：  
  
- 这两个程序集标识是等效的。 对于强名称程序集，等效要求程序集名称、版本、公钥标记和区域性完全相同。 对于简单命名的程序集，等效要求程序集名称和区域性匹配。  
  
- 这两个程序集标识都是指在 .NET Framework 上运行的程序集。 `true`即使程序集版本号不匹配，此条件也将返回。  
  
- 这两个程序集不是托管程序集，而是 `fUnified1` `fUnified2` 设置为 `true` 。  
  
 该 `fUnified` 标志指示在强名称程序集的版本号之前的所有版本号都被视为等效于强名称程序集。 例如，如果的值 `pwzAssemblyIndentity1` 为 "MyAssembly，version = 3.0.0.0，culture = 中立，publicKeyToken = ..."，并且的值 `fUnified1` 为 `true` ，则表示从版本0.0.0.0 到3.0.0.0 的 MyAssembly 的所有版本均应视为等效。 在这种情况下，如果 `pwzAssemblyIndentity2` 引用与相同的程序集，则将 `pwzAssemblyIndentity1` 设置为，但该程序集的版本号较低 `pfEquivalent` `true` 。 如果 `pwzAssemblyIdentity2` 引用较高的版本号， `pfEquivalent` 则 `true` 仅当的值为时，才设置为 `fUnified2` `true` 。  
  
 `pResult`参数包含有关两个程序集被视为等效或不等效的原因的具体信息。 有关详细信息，请参阅 [AssemblyComparisonResult 枚举](assemblycomparisonresult-enumeration.md)。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [合成全局静态函数](fusion-global-static-functions.md)
- [AssemblyComparisonResult 枚举](assemblycomparisonresult-enumeration.md)
