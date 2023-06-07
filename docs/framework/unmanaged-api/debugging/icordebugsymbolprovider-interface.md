---
title: ICorDebugSymbolProvider 接口
ms.date: 03/30/2017
ms.assetid: 85b24196-b6c6-4bda-9de3-47180bd6ff96
ms.openlocfilehash: ff1f39be3d3db43f70cfa4a0711a3f42c180bc1a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672079"
---
# <a name="icordebugsymbolprovider-interface"></a>ICorDebugSymbolProvider 接口

提供可用于检索调试符号信息的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetAssemblyImageBytes 方法](icordebugsymbolprovider-getassemblyimagebytes-method.md)|给定合并程序集的相对虚拟地址 (RVA)，读取合并程序集中的数据。|  
|[GetAssemblyImageMetadata 方法](icordebugsymbolprovider-getassemblyimagemetadata-method.md)|返回合并程序集中的元数据。|  
|[GetCodeRange 方法](icordebugsymbolprovider-getcoderange-method.md)|给定方法的相对虚拟地址 (RVA)，获取该方法的起始地址和大小。|  
|[GetInstanceFieldSymbols 方法](icordebugsymbolprovider-getinstancefieldsymbols-method.md)|获取与 Typespec 签名相对应的实例字段符号。|  
|[GetMergedAssemblyRecords 方法](icordebugsymbolprovider-getmergedassemblyrecords-method.md)|获取所有合并程序集的符号记录。|  
|[GetMethodLocalSymbols 方法](icordebugsymbolprovider-getmethodlocalsymbols-method.md)|给定方法的相对虚拟地址 (RVA)，获取该方法的本地符号。|  
|[GetMethodParameterSymbols 方法](icordebugsymbolprovider-getmethodparametersymbols-method.md)|给定方法的相对虚拟地址 (RVA) 后，获取该方法的参数符号。|  
|[GetMethodProps 方法](icordebugsymbolprovider-getmethodprops-method.md)|给定方法的相对虚拟地址 (RVA)，返回有关该方法属性的信息，例如该方法的元数据标记及其泛型参数信息。|  
|[GetObjectSize 方法](icordebugsymbolprovider-getobjectsize-method.md)|基于对象的 TypeSpec 签名返回对象的大小。|  
|[GetStaticFieldSymbols 方法](icordebugsymbolprovider-getstaticfieldsymbols-method.md)|获取与 Typespec 签名相对应的静态字段符号。|  
|[GetTypeProps 方法](icordebugsymbolprovider-gettypeprops-method.md)|给定 vtable 中的相对虚拟地址 (RVA)，返回类型的属性信息（例如其泛型参数的签名数量）。|  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此接口仅适用于 .NET Native。 如果在 .NET Native 外为 ICorDebug 方案实现此接口，则公共语言运行时将忽略此接口。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
