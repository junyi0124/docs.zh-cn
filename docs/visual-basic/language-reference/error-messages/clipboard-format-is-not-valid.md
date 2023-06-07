---
title: 剪贴板格式无效
ms.date: 07/20/2015
f1_keywords:
- vbrID460
ms.assetid: 71a4a045-65bb-417d-b3bd-99a9fa3c53f6
ms.openlocfilehash: 429d1e120a0044152a358a87663eb09989f45b0e
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90874594"
---
# <a name="clipboard-format-is-not-valid"></a>剪贴板格式无效

指定的剪贴板格式与正在执行的方法不兼容。 此错误的可能原因包括：  
  
- 将剪贴板的 `GetText` 或 `SetText` 方法用于或以外的剪贴板格式 `vbCFText` `vbCFLink` 。  
  
- 将剪贴板的 `GetData` 或 `SetData` 方法用于、或以外的剪贴板格式 `vbCFBitmap` `vbCFDIB` `vbCFMetafile` 。  
  
- `GetData` `SetData` 如果在 microsoft windows `DataObject` 中未注册剪贴板格式，则在 microsoft windows 为注册格式 ( # B0 HC000-&HFFFF) 的范围内，使用具有剪贴板格式的或方法作为已注册的格式。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
- 删除无效的格式，并指定一个有效的格式。  
  
## <a name="see-also"></a>另请参阅

- [剪贴板：添加其他格式](/cpp/mfc/clipboard-adding-other-formats)
