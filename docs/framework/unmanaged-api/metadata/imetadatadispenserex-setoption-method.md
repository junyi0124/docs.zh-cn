---
title: IMetaDataDispenserEx::SetOption 方法
ms.date: 03/30/2017
api_name:
- IMetaDataDispenserEx.SetOption
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataDispenserEx::SetOption
helpviewer_keywords:
- IMetaDataDispenserEx::SetOption method [.NET Framework metadata]
- SetOption method [.NET Framework metadata]
ms.assetid: 9f1c7ccd-7fb2-41d8-aa00-24b823376527
topic_type:
- apiref
ms.openlocfilehash: 4216658eb562c5c57b75c3c257cd8e53a7a34221
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95700582"
---
# <a name="imetadatadispenserexsetoption-method"></a>IMetaDataDispenserEx::SetOption 方法

为当前元数据范围将指定的选项设置为给定的值。 选项控制如何处理对当前元数据范围的调用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetOption (  
    [in] REFGUID optionId,
    [in] const VARIANT *pValue  
);  
```  
  
## <a name="parameters"></a>参数  

 `optionId`  
 中指向 GUID 的指针，该 GUID 指定要设置的选项。  
  
 `pValue`  
 中用于设置选项的值。 此值的类型必须是指定选项的类型的变体。  
  
## <a name="remarks"></a>注解  

 下表列出了参数可以指向的可用 Guid， `optionId` 以及相应参数的有效值 `pValue` 。  
  
|GUID|描述|`pValue` 参数|  
|----------|-----------------|------------------------|  
|MetaDataCheckDuplicatesFor|控制检查哪些项是否有重复项。 每次调用创建新项的 [IMetaDataEmit](imetadataemit-interface.md) 方法时，都可以要求方法检查当前范围中是否已存在该项。 例如，你可以检查项是否存在 `mdMethodDef` ; 在这种情况下，当你调用 [IMetaDataEmit：:D efinemethod](imetadataemit-definemethod-method.md)时，它将检查当前范围中是否尚不存在该方法。 此检查使用唯一标识给定方法的键：父类型、名称和签名。|必须是 UI4 类型的变量，并且必须包含 [CorCheckDuplicatesFor](corcheckduplicatesfor-enumeration.md) 枚举值的组合。|  
|MetaDataRefToDefCheck|控制哪些引用项将被转换为定义。 默认情况下，如果在当前范围内实际定义了引用项，则元数据引擎将通过将引用项转换为其定义来优化代码。|必须是 UI4 类型的变量，并且必须包含 [CorRefToDefCheck](correftodefcheck-enumeration.md) 枚举值的组合。|  
|MetaDataNotificationForTokenMovement|控制在元数据合并期间发生的标记重新映射生成回调。 使用 [IMetaDataEmit：： SetHandler](imetadataemit-sethandler-method.md) 方法建立 [IMapToken](imaptoken-interface.md) 接口。|必须是 UI4 类型的变量，并且必须包含 [CorNotificationForTokenMovement](cornotificationfortokenmovement-enumeration.md) 枚举值的组合。|  
|MetaDataSetENC|控制 (ENC) 的 "编辑并继续" 的行为。 一次只能设置一种行为模式。|必须是 UI4 类型的变体，并且必须包含 [CorSetENC](corsetenc-enumeration.md) 枚举的值。 该值不是位掩码。|  
|MetaDataErrorIfEmitOutOfOrder|控制哪些发出的有序错误生成回调。 发出元数据的顺序不是致命的;但是，如果您以元数据引擎所采用的顺序发出元数据，则元数据将更紧凑，因而可以更有效地搜索。 使用 `IMetaDataEmit::SetHandler` 方法建立 [IMetaDataError](imetadataerror-interface.md) 接口。|必须是 UI4 类型的变量，并且必须包含 [CorErrorIfEmitOutOfOrder](corerrorifemitoutoforder-enumeration.md) 枚举值的组合。|  
|MetaDataImportOption|控制在 ENC 中删除的哪些类型的项由枚举器检索。|必须是 UI4 类型的变体，并且必须包含 [CorImportOptions 枚举](corimportoptions-enumeration.md) 枚举的值的组合。|  
|MetaDataThreadSafetyOptions|控制元数据引擎是否获取读取器/写入器锁，从而确保线程安全。 默认情况下，引擎假定调用方对访问是单线程的，因此不会获取任何锁。 使用元数据 API 时，客户端负责维护适当的线程同步。|必须是 UI4 类型的变体，并且必须包含 [CorThreadSafetyOptions](corthreadsafetyoptions-enumeration.md) 枚举的值。 该值不是位掩码。|  
|MetaDataGenerateTCEAdapters|控制类型库导入程序是否应为 COM 连接点容器 (TCE) 适配器生成紧密耦合的事件。|必须是 BOOL 类型的变体。 如果 `pValue` 设置为 `true` ，则类型库导入程序将生成 TCE 适配器。|  
|MetaDataTypeLibImportNamespace|指定要导入的类型库的非默认命名空间。|必须为 null 值或类型为 BSTR 的变体。 如果 `pValue` 是 null 值，则当前命名空间设置为 null; 否则，当前命名空间将设置为以变量的 BSTR 类型保存的字符串。|  
|MetaDataLinkerOptions|控制链接器是否应生成程序集或 .NET Framework 模块文件。|必须是 UI4 类型的变量，并且必须包含 [CorLinkerOptions](corlinkeroptions-enumeration.md) 枚举值的组合。|  
|MetaDataRuntimeVersion|指定生成此映像所基于的公共语言运行时版本。 版本存储为字符串，如 "v1.0.3705"。|必须为 null 值、VT_EMPTY 值或类型为 BSTR 的变体。 如果 `pValue` 为 null，则运行时版本设置为 null。 如果 `pValue` VT_EMPTY，则将版本设置为默认值，该值将从运行元数据的 Mscorwks.dll 版本中提取。 否则，运行时版本设置为以变量的 BSTR 类型保存的字符串。|  
|MetaDataMergerOptions|指定用于合并元数据的选项。|必须是 UI4 类型的变体，并且必须包含枚举的值的组合 `MergeFlags` ，这在 corhdr.h 文件中进行了介绍。|  
|MetaDataPreserveLocalRefs|禁止优化对定义的本地引用。|必须包含 [CorLocalRefPreservation](corlocalrefpreservation-enumeration.md) 枚举值的组合。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataDispenserEx 接口](imetadatadispenserex-interface.md)
- [IMetaDataDispenser 接口](imetadatadispenser-interface.md)
