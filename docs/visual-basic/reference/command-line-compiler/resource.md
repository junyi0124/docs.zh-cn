---
title: -resource
ms.date: 03/13/2018
helpviewer_keywords:
- /resource compiler option [Visual Basic]
- -resource compiler option [Visual Basic]
- /res compiler option [Visual Basic]
- res compiler option [Visual Basic]
- -res compiler option [Visual Basic]
- resource compiler option [Visual Basic]
ms.assetid: eee2f227-91f2-4f2b-a9d6-1c51c5320858
ms.openlocfilehash: 726f3dd179aedb39b578c8580c9632182af2d5e0
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085121"
---
# <a name="-resource-visual-basic"></a>-resource (Visual Basic)

将托管资源嵌入程序集。  
  
## <a name="syntax"></a>语法  
  
```console  
-resource:filename[,identifier[,public|private]]  
```

or  

```console
-res:filename[,identifier[,public|private]]  
```  
  
## <a name="arguments"></a>自变量  
  
|术语|定义|  
|---|---|  
|`filename`|必需。 要嵌入到输出文件中的资源文件的名称。 默认情况下，`filename` 在程序集中是公共的。 如果文件名包含空格，则将名称括在引号 (" ") 内。|  
|`identifier`|可选。 资源的逻辑名称；用于加载资源的名称。 默认值是文件的名称。 可以选择在程序集清单中指定资源是公共还是专用的，如下所示：`-res:filename.res, myname.res, public`|  
  
## <a name="remarks"></a>备注  

 使用 `-linkresource` 将资源链接到程序集，而不将资源文件置于输出文件中。  
  
 例如，如果 `filename` [是由资源文件生成器 (Resgen.exe)](../../../framework/tools/resgen-exe-resource-file-generator.md) 创建的或在开发环境中创建的 .NET Framework 资源文件，则可使用 <xref:System.Resources> 命名空间中的成员来访问它（有关详细信息，请参阅 <xref:System.Resources.ResourceManager>）。 若要在运行时访问所有其他资源，请使用以下方法之一：<xref:System.Reflection.Assembly.GetManifestResourceInfo%2A>、<xref:System.Reflection.Assembly.GetManifestResourceNames%2A> 或 <xref:System.Reflection.Assembly.GetManifestResourceStream%2A>。  
  
 `-resource` 的缩写形式是 `-res`。  
  
 有关如何在 Visual Studio IDE 中设置 `-resource` 的信息，请参阅[管理应用程序资源 (.NET)](/visualstudio/ide/managing-application-resources-dotnet)。  
  
## <a name="example"></a>示例  

 下面的代码编译 `In.vb` 并附加资源文件 `Rf.resource`。  
  
```console
vbc -res:rf.resource in.vb  
```  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [-win32resource](win32resource.md)
- [-linkresource (Visual Basic)](linkresource.md)
- [-target (Visual Basic)](target.md)
- [示例编译命令行](sample-compilation-command-lines.md)
