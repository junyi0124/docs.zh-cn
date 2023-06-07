---
title: 如何：使用 EdmGen.exe 验证模型和映射文件
ms.date: 03/30/2017
ms.assetid: 2641906a-971a-4d0b-8aee-13fabc02a1cc
ms.openlocfilehash: 62a3bde9d2431b9e9e86e2a8d8896520f3456590
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91198114"
---
# <a name="how-to-use-edmgenexe-to-validate-model-and-mapping-files"></a>如何：使用 EdmGen.exe 验证模型和映射文件

本主题演示如何使用 [EDM 生成器 ( # A0) ](edm-generator-edmgen-exe.md) 工具来验证模型和映射文件。 有关详细信息，请参阅 [实体数据模型](../entity-data-model.md)。  
  
### <a name="to-validate-the-school-model-using-edmgenexe"></a>使用 EdmGen.exe 验证 School 模型  
  
1. 创建 School 数据库。 有关详细信息，请参阅 [创建 School 示例数据库](/previous-versions/dotnet/netframework-4.0/bb399731(v=vs.100))。  
  
2. 生成 School 模型。 有关详细信息，请参阅 [如何：使用 EdmGen.exe 生成模型和映射文件](how-to-use-edmgen-exe-to-generate-the-model-and-mapping-files.md)。  
  
3. 在命令提示符下执行以下命令（无换行符）：  
  
    ```console
    "%windir%\Microsoft.NET\Framework\v4.0.30319\edmgen.exe" /mode:ValidateArtifacts /inssdl:.\School.ssdl /inmsl:.\School.msl /incsdl:.\School.csdl  
    ```  
  
## <a name="see-also"></a>请参阅

- [如何：手动配置实体框架项目](/previous-versions/dotnet/netframework-4.0/bb738546(v=vs.100))
- [ADO.NET 实体数据模型工具](/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100))
- [如何：预生成视图以改进查询性能](/previous-versions/dotnet/netframework-4.0/bb896240(v=vs.100))
- [如何：使用 EdmGen.exe 生成对象层代码](how-to-use-edmgen-exe-to-generate-object-layer-code.md)
