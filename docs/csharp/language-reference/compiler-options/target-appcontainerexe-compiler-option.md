---
description: -target:appcontainerexe（C# 编译器选项）
title: -target:appcontainerexe（C# 编译器选项）
ms.date: 07/20/2015
ms.assetid: e7e62229-23ea-4e53-bef5-380d951bf95f
ms.openlocfilehash: e4aa60ebc9dcc1a63b63863385b0ee9f13d6d78d
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91193733"
---
# <a name="-targetappcontainerexe-c-compiler-options"></a>-target:appcontainerexe（C# 编译器选项）

如果使用 -target:appcontainerexe 编译器选项，则编译器会创建一个 Windows 可执行 (.exe) 文件，该文件必须在应用容器中运行。 此选项与 [-target:winexe](./target-winexe-compiler-option.md) 等效，但专门用于 Windows 8.x 应用商店应用。  
  
## <a name="syntax"></a>语法  
  
```console  
-target:appcontainerexe  
```  
  
## <a name="remarks"></a>备注  

 为了要求应用在应用容器中运行，此选项在[可移植可执行](/windows/desktop/Debug/pe-format) (PE) 文件中设置了一个位。 设置该位时，如果 CreateProcess 方法尝试在应用容器外启动该可执行文件，就会发生错误。  
  
 除非使用 [-out](./out-compiler-option.md) 选项，否则输出文件名采用包含 [Main](../../programming-guide/main-and-command-args/index.md) 方法的输入文件的名称。  
  
 如果在命令提示符处指定此选项，则在下一个 -out 或 -target 选项之前，会使用所有文件来创建可执行文件。  
  
### <a name="to-set-this-compiler-option-in-the-ide"></a>在 IDE 中设置此编译器选项  
  
1. 在“解决方案资源管理器”  中，打开项目的快捷菜单，然后选择“属性”  。  
  
2. 在“应用程序”选项卡上，在“输出类型”列表中选择“Windows 应用商店应用”。  
  
     此选项仅可用于 Windows 8.x 应用商店应用模板。  
  
 有关如何以编程方式设置此编译器选项的信息，请参阅 <xref:VSLangProj80.ProjectProperties3.OutputType%2A>。  
  
## <a name="example"></a>示例  

 以下命令将 `filename.cs` 编译为一个只能在应用容器中运行的 Windows 可执行文件。  
  
```console  
csc -target:appcontainerexe filename.cs  
```  
  
## <a name="see-also"></a>另请参阅

- [-target（C# 编译器选项）](./target-compiler-option.md)
- [-target:winexe（C# 编译器选项）](./target-winexe-compiler-option.md)
- [C# 编译器选项](./index.md)
