---
title: 在使用 "Object" 类型的自变量时，请使用 "FilePutObject"，而不要使用 "FilePut"
ms.date: 07/20/2015
f1_keywords:
- vbrUseFilePutObject
ms.assetid: d207b9b7-5898-4c13-8b03-9feefac5f726
ms.openlocfilehash: c6ae755ad3eca4b1c50b83049885b6cf66f13c07
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91100329"
---
# <a name="use-fileputobject-instead-of-fileput-when-using-argument-of-type-object"></a>在使用 "Object" 类型的自变量时，请使用 "FilePutObject"，而不要使用 "FilePut"

`FilePut` 方法包含 `Object`类型的参数。 应使用`FilePutObject` 替代 `FilePut` ，以避免多义性。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
- 将 `FilePut` 替换为 `FilePutObject`。  
  
- 将 `Object` 参数强制转换为一个更明确的类型。  
  
- 使用 `My.Computer.FileSystem` 对象中的可用功能。  
  
## <a name="see-also"></a>请参阅

- [文件系统文件](xref:Microsoft.VisualBasic.FileIO.FileSystem)
- [文件 My.computer.filesystem.writeallbytes](xref:Microsoft.VisualBasic.MyServices.FileSystemProxy.WriteAllBytes%2A)
