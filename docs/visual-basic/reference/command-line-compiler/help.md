---
title: -help，-?
ms.date: 03/10/2018
helpviewer_keywords:
- /? compiler option [Visual Basic]
- -help compiler option [Visual Basic]
- /help compiler option [Visual Basic]
- help compiler option [Visual Basic]
- -? compiler option [Visual Basic]
- '? compiler option [Visual Basic]'
ms.assetid: eb984aa5-ac98-4d0b-a0d2-24238d7bc8dc
ms.openlocfilehash: 3cf70e45b169eca43db5b5c377dd3364db0188ab
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91058087"
---
# <a name="-help---visual-basic"></a>-help，-? (Visual Basic)

显示编译器选项。  
  
## <a name="syntax"></a>语法  
  
```console  
-help  
```

or  

```console
-?  
```  
  
## <a name="remarks"></a>备注  

 如果在编译中包含此选项，则不会创建任何输出文件，且不会发生任何编译。  
  
> [!NOTE]
> `-help` 选项在 Visual Studio 开发环境内无法使用；仅当从命令行编译时才可用。  
  
## <a name="example"></a>示例  

 下面的代码显示来自命令行的帮助。  
  
```console  
vbc -help  
```  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
