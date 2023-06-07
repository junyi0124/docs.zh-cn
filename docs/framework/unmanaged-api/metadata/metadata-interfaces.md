---
title: 元数据接口
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged interfaces [.NET Framework], metadata
- metadata interfaces [.NET Framework]
- interfaces (.NET Framework metadata]
ms.assetid: f5cdac93-a28c-48ef-8a19-5773376e9e7c
ms.openlocfilehash: 5d9b48df740668797a7c901219401e9ea304a8f8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672876"
---
# <a name="metadata-interfaces"></a>元数据接口

本节描述非托管接口，这些接口提供对由 .NET Framework 类型、方法、字段等公开的元数据的访问。  
  
## <a name="in-this-section"></a>本节内容  

 [ICeeGen 接口](iceegen-interface.md)  
 提供用于动态代码编译的方法。  
  
 [IHostFilter 接口](ihostfilter-interface.md)  
 提供运行时主机用于标记待处理的元数据标记的方法。  
  
 [IMapToken 接口](imaptoken-interface.md)  
 提供导入的和发出的元数据签名之间的映射功能。  
  
 [IMetaDataAssemblyEmit 接口](imetadataassemblyemit-interface.md)  
 提供支持公共语言运行时 (CLR) 用于解析和使用资源的自描述模型的方法。  
  
 [IMetaDataAssemblyImport 接口](imetadataassemblyimport-interface.md)  
 提供访问和检查程序集清单内容的方法。  
  
 [IMetaDataConverter 接口](imetadataconverter-interface.md)  
 提供将类型库映射到其元数据签名并进行相互转换的方法。  
  
 [IMetaDataDispenser 接口](imetadatadispenser-interface.md)  
 `IMetaDataDispenser` 已过时。 请改用 `IMetaDataDispenserEx`。  
  
 [IMetaDataDispenserEx 接口](imetadatadispenserex-interface.md)  
 提供映射用于创建或修改元数据的内存区域的方法。  
  
 [IMetaDataEmit 接口](imetadataemit-interface.md)  
 提供创建、修改和存储与当前定义的范围中的程序集相关的元数据的方法。  
  
 [IMetaDataEmit2 接口](imetadataemit2-interface.md)  
 提供用于定义和修改方法的元数据签名和带有 <xref:System.Type?displayProperty=nameWithType> 类型的参数的构造函数的方法。  
  
 [IMetaDataError 接口](imetadataerror-interface.md)  
 提供用于在解析程序集的元数据签名期间报告错误的回调机制。  
  
 [IMetaDataFilter 接口](imetadatafilter-interface.md)  
 提供用于标记和筛选元数据标记以避免重复已进行的操作的方法。  
  
 [IMetaDataImport 接口](imetadataimport-interface.md)  
 提供用于从其他程序集导入和操作类型的方法。  
  
 [IMetaDataImport2 接口](imetadataimport2-interface.md)  
 扩展 `IMetaDataImport` 以提供使用泛型类型的功能。  
  
 [IMetaDataInfo 接口](imetadatainfo-interface.md)  
 提供一种方法，使用此方法可获取关于将元数据从磁盘文件映射到内存中的信息。  
  
 [IMetaDataTables 接口](imetadatatables-interface.md)  
 提供存储和检索表中元数据信息的方法。  
  
 [IMetaDataTables2 接口](imetadatatables2-interface.md)  
 扩展 `IMetaDataTables` 以包含用于处理元数据流的方法。  
  
 [IMetaDataValidate 接口](imetadatavalidate-interface.md)  
 提供验证元数据签名的方法。  
  
## <a name="related-sections"></a>相关章节  

 [元数据全局静态函数](metadata-global-static-functions.md)  
  
 [元数据枚举](metadata-enumerations.md)  
  
 [元数据结构](metadata-structures.md)  
  
 [元数据联合](metadata-unions.md)
