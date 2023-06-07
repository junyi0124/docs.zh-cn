---
title: -win32icon
ms.date: 03/13/2018
helpviewer_keywords:
- win32icon compiler option [Visual Basic]
- -win32icon compiler option [Visual Basic]
- /win32icon compiler option [Visual Basic]
ms.assetid: aecaab01-9353-46c5-941c-6edabd4eff92
ms.openlocfilehash: c6d5e054063592db5777a76fe19da79337ed5034
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91084952"
---
# <a name="-win32icon"></a>-win32icon

将 .ico 文件插入到输出文件。 此 .ico 文件表示文件资源管理器  中的输出文件。  
  
## <a name="syntax"></a>语法  
  
```console  
-win32icon:filename  
```  
  
## <a name="arguments"></a>自变量  
  
|术语|定义|  
|---|---|  
|`filename`|要向输出文件添加的 .ico 文件。 如果文件名包含空格，则将名称括在引号 (" ") 内。|  
  
## <a name="remarks"></a>备注  

 可以使用 Microsoft Windows 资源编译器 (RC) 创建 .ico 文件。 在编译 Visual C++ 程序时会调用资源编译器；.ico 文件是从 .rc 文件创建的。 `-win32icon` 和 `-win32resource` 选项互斥。  
  
 请参阅 [-linkresource (Visual Basic)](linkresource.md) 以引用 .NET Framework 资源文件，或参阅 [-resource (Visual Basic)](resource.md) 以附加 .NET Framework 资源文件。 请参阅 [-win32resource](win32resource.md) 以导入 .res 文件。  
  
|若要在 Visual Studio IDE 中设置 -win32icon|  
|---|  
|1.在 **“解决方案资源管理器”** 中选择一个项目。 在“项目”菜单上，单击“属性”   。 <br />2.单击“应用程序”  选项卡。<br />3.修改“图标”  框中的值。|  
  
## <a name="example"></a>示例  

 下面的代码编译 `In.vb` 并附加 .ico 文件 `Rf.ico`。  
  
```console
vbc -win32icon:rf.ico in.vb  
```  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
