---
description: -win32manifest（C# 编译器选项）
title: -win32manifest（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /win32manifest
helpviewer_keywords:
- /win32manifest compiler option [C#]
- win32manifest compiler option [C#]
- -win32manifest compiler option [C#]
ms.assetid: 9460ea1b-6c9f-44b8-8f73-301b30a01de1
ms.openlocfilehash: 1d2eefdab433f67e1cba5f709a2db8ec6b9a5dc7
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91171307"
---
# <a name="-win32manifest-c-compiler-options"></a>-win32manifest（C# 编译器选项）

使用 -win32manifest 选项可以指定要嵌入到项目的可迁移可执行 (PE) 文件中的用户定义的 Win32 应用程序清单文件。  
  
## <a name="syntax"></a>语法  
  
```console  
-win32manifest: filename  
```  
  
## <a name="arguments"></a>自变量  

 `filename`  
 自定义清单文件的名称和位置。  
  
## <a name="remarks"></a>备注  

 默认情况下，Visual C# 编译器嵌入指定“asInvoker”的请求执行级别的应用程序清单。 它在生成该可执行文件的同一文件夹中创建清单，如果使用 Visual Studio，该文件夹通常为 bin\Debug 或 bin\Release。 如果要提供自定义清单（例如，指定“highestAvailable”或“requireAdministrator”的请求执行级别的清单），请使用此选项指定文件名。  
  
> [!NOTE]
> 此选项和 [-win32res（C# 编译器选项）](./win32res-compiler-option.md)选项是互斥的。 如果尝试在同一命令行中同时使用这两个选项，将收到一个生成错误。  
  
 如果应用程序没有用于指定请求执行级别的应用程序清单，将受到 Windows 用户帐户控制功能下的文件/注册表虚拟化的影响。 有关详细信息，请参阅[用户帐户控制](/windows/access-protection/user-account-control/user-account-control-overview)。  
  
 如果满足下列任一条件，则应用程序会受到虚拟化的影响：  
  
- 使用 -nowin32manifest 选项，并且在随后的生成步骤中未提供清单，或者没有通过使用 -win32res 选项将其包含在 Windows 资源 (.res) 文件中。  
  
- 提供的自定义清单未指定请求执行级别。  
  
 Visual Studio 创建默认 .manifest 文件，并将它与可执行文件一起存储在“调试”和“发布”目录中。 可以用任意文本编辑器创建一个清单，然后将该文件添加到项目中，从而添加自定义清单。 或者，也可以右键单击“解决方案资源管理器”中的“项目”图标，单击“添加新项”，然后单击“应用程序清单文件”。 添加完新的或现有清单文件后，该文件将显示在“清单”下拉列表中。 有关详细信息，请参阅[“项目设计器”->“应用程序”页 (C#)](/visualstudio/ide/reference/application-page-project-designer-csharp)。  
  
 提供应用程序清单的操作，可以作为自定义后期生成步骤，也可以通过使用 [-nowin32manifest（C# 编译器选项）](./nowin32manifest-compiler-option.md)选项作为 Win32 资源文件的组成部分。 如果希望应用程序受到 Windows Vista 的文件或注册表虚拟化的影响，请使用该选项。 这将阻止编译器在可迁移可执行 (PE) 文件中创建和嵌入默认清单。  
  
## <a name="example"></a>示例  

 下例演示了 Visual C# 编译器插入到 PE 中的默认清单。  
  
> [!NOTE]
> 编译器将一个标准应用程序名称“MyApplication.app”插入到 xml 中。 这是一种使应用程序可以在 Windows Server 2003 Service Pack 3 上运行的解决方法。  
  
```xml  
<?xml version="1.0" encoding="utf-8" standalone="yes"?>  
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">  
  <assemblyIdentity version="1.0.0.0" name="MyApplication.app"/>  
  <trustInfo xmlns="urn:schemas-microsoft-com:asm.v2">  
    <security>  
      <requestedPrivileges xmlns="urn:schemas-microsoft-com:asm.v3">  
        <requestedExecutionLevel level="asInvoker"/>  
      </requestedPrivileges>  
    </security>  
  </trustInfo>  
</assembly>  
```  
  
## <a name="see-also"></a>请参阅

- [C# 编译器选项](./index.md)
- [-nowin32manifest（C# 编译器选项）](./nowin32manifest-compiler-option.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
