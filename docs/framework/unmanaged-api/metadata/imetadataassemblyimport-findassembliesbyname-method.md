---
title: IMetaDataAssemblyImport::FindAssembliesByName 方法
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.FindAssembliesByName
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::FindAssembliesByName
helpviewer_keywords:
- FindAssembliesByName method [.NET Framework metadata]
- IMetaDataAssemblyImport::FindAssembliesByName method [.NET Framework metadata]
ms.assetid: 4db97cf9-e4c1-4233-8efa-cbdc0e14a8e4
topic_type:
- apiref
ms.openlocfilehash: a2bf0335f8d75c7dbd1a651afdb54da8c7be2460
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731605"
---
# <a name="imetadataassemblyimportfindassembliesbyname-method"></a>IMetaDataAssemblyImport::FindAssembliesByName 方法

`szAssemblyName`使用由公共语言运行时使用的标准规则 (CLR) 来解析引用，获取具有指定参数的程序集的数组。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT FindAssembliesByName (  
    [in]  LPCWSTR     szAppBase,
    [in]  LPCWSTR     szPrivateBin,
    [in]  LPCWSTR     szAssemblyName,
    [out] IUnknown    *ppIUnk[],
    [in]  ULONG       cMax,
    [out] ULONG       *pcAssemblies  
);  
```  
  
## <a name="parameters"></a>参数  

 `szAppBase`  
 中要在其中搜索给定程序集的根目录。 如果此值设置为 `null` ， `FindAssembliesByName` 则将仅在程序集的全局程序集缓存中查找。  
  
 `szPrivateBin`  
 中要在其中搜索程序集的根目录下的以分号分隔的子目录列表 (例如 "bin; bin2" ) 。 除了在默认探测规则中指定的目录之外，还会探测这些目录。  
  
 `szAssemblyName`  
 中要查找的程序集的名称。 此字符串的格式在的 "类引用" 页中定义 <xref:System.Reflection.AssemblyName> 。  
  
 `ppIUnk`  
 中要在其中放置接口指针的 [IUnknown](/cpp/atl/iunknown) 类型的数组 `IMetadataAssemblyImport` 。  
  
 `cMax`  
 弄可放置的接口指针的最大数目 `ppIUnk` 。  
  
 `pcAssemblies`  
 弄返回的接口指针的数目。 即，实际置于中的接口指针的数目 `ppIUnk` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|`S_OK`|`FindAssembliesByName` 已成功返回。|  
|`S_FALSE`|没有程序集。|  
  
## <a name="remarks"></a>注解  

 在给定程序集名称的 `FindAssembliesByName` 情况下，方法通过遵循用于解析程序集引用的标准规则查找程序集。  (有关详细信息，请参阅 [运行时如何定位程序集](../../deployment/how-the-runtime-locates-assemblies.md)。 ) `FindAssembliesByName` 使调用方可以配置程序集解析程序上下文的各个方面，例如应用程序基和专用搜索路径。  
  
 `FindAssembliesByName`方法要求在进程中初始化 CLR，以便调用程序集解析逻辑。 因此，在调用之前，必须先调用 [CoInitializeEE](../hosting/coinitializeee-function.md) (传递 COINITEE_DEFAULT) `FindAssembliesByName` ，然后再调用 [CoUninitializeCor](../hosting/couninitializecor-function.md)。  
  
 `FindAssembliesByName` 返回一个 [IMetaDataImport](imetadataimport-interface.md) 指针，该指针指向包含传入的程序集名称的程序集清单的文件。 如果未完全指定给定的程序集名称 (例如，如果不包含版本) ，则可能会返回多个程序集。  
  
 `FindAssembliesByName` 在编译时尝试查找引用的程序集的编译器通常使用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>请参阅

- [运行时如何定位程序集](../../deployment/how-the-runtime-locates-assemblies.md)
- [IMetaDataAssemblyImport 接口](imetadataassemblyimport-interface.md)
