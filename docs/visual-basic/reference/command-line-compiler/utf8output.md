---
title: -utf8output
ms.date: 07/20/2015
helpviewer_keywords:
- -utf8output compiler option [Visual Basic]
- utf8output compiler option [Visual Basic]
- /utf8output compiler option [Visual Basic]
ms.assetid: 8ab36b1e-027a-49ac-85b4-f48997d9e4d6
ms.openlocfilehash: 2faebb7870cbc019d479215563261f331f9fddcf
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085069"
---
# <a name="-utf8output-visual-basic"></a>-utf8output (Visual Basic)

显示使用 UTF-8 编码的编译器输出。  
  
## <a name="syntax"></a>语法  
  
```console  
-utf8output[+ | -]  
```  
  
## <a name="arguments"></a>自变量  

 `+` &#124; `-`  
 可选。 此选项的默认值为 `-utf8output-`，这意味着编译器输出不使用 UTF-8 编码。 指定 `-utf8output` 与指定 `-utf8output+` 相同。  
  
## <a name="remarks"></a>备注  

 在某些国际配置中，编译器输出无法在控制台上正确显示。 在这种情况下，请使用 `-utf8output`，并将编译器输出重定向到文件。  
  
> [!NOTE]
> `-utf8output` 选项在 Visual Studio 开发环境内无法使用；仅当从命令行编译时才可用。  
  
## <a name="example"></a>示例  

 下面的代码编译 `In.vb`，并指示编译器使用 UTF-8 编码显示输出。  
  
```console  
vbc -utf8output in.vb  
```  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
