---
title: -win32resource
ms.date: 03/13/2018
f1_keywords:
- -win32resource
- win32resource
helpviewer_keywords:
- /win32resource compiler option [Visual Basic]
- -win32resource compiler option [Visual Basic]
- win32resource compiler option [Visual Basic]
ms.assetid: e226946d-19ce-4cc9-91f5-aed24f77aa2b
ms.openlocfilehash: d146f5967058b05795026cd7726ed5eda7ba3153
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91095403"
---
# <a name="-win32resource"></a>-win32resource

在输出文件中插入 Win32 资源文件。  
  
## <a name="syntax"></a>语法  
  
```console  
-win32resource:filename  
```  
  
## <a name="arguments"></a>自变量  

 `filename`  
 要添加到输出文件的资源文件的名称。 如果文件名包含空格，则将名称括在引号 (" ") 内。  
  
## <a name="remarks"></a>备注  

 可以使用 Microsoft Windows 资源编译器 (RC) 创建 Win32 资源文件。  
  
 Win32 资源可以包含版本或位图（图标）信息，这些信息有助于在文件资源管理器  中标识你的应用程序。 如果不指定 `-win32resource`，编译器将根据程序集版本生成版本信息。 `-win32resource` 和 `-win32icon` 选项互斥。  
  
 请参阅 [-linkresource (Visual Basic)](linkresource.md) 以引用 .NET Framework 资源文件，或参阅 [-resource (Visual Basic)](resource.md) 以附加 .NET Framework 资源文件。  
  
> [!NOTE]
> `-win32resource` 选项在 Visual Studio 开发环境内无法使用；仅当从命令行编译时才可用。  
  
## <a name="example"></a>示例  

 下面的代码编译 `In.vb` 并附加 Win32 资源文件 `Rf.res`：  
  
```console  
vbc -win32resource:rf.res in.vb  
```  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
