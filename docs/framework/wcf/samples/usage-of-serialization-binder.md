---
title: 序列化联编程序的用法
ms.date: 03/30/2017
ms.assetid: ab46c087-200c-45bf-9c95-5a6cda6e8b98
ms.openlocfilehash: 061afb94d97e3d8a1222e6de9932344fb3ebbe59
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294893"
---
# <a name="usage-of-serialization-binder"></a>序列化联编程序的用法

此示例演示如何使用 <xref:System.Runtime.Serialization.SerializationBinder> 在序列化泛型类型时更改其版本。  
  
## <a name="demonstrates"></a>演示  

 <xref:System.Runtime.Serialization.SerializationBinder>, <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>  
  
## <a name="discussion"></a>讨论 (Discussion)  

 此示例显示面向不同版本的 .NET Framework 的两个实体如何使用二进制格式化程序和序列化联编程序进行通信。  
  
此示例是使用 .NET 远程处理开发的。 它包含目标为 .NET Framework 4 的服务器，该服务器可实现一个具有泛型类型的协定，以及两个不同的客户端，一个目标 .NET Framework 2.0，另一个目标 .NET Framework 4。  
  
 服务器将一个 <xref:System.Runtime.Serialization.SerializationBinder> 附加到二进制格式化程序，以便在序列化时相应地更改类型版本，从而使两个客户端都可以正确反序列化这些类型。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 若要执行该客户端，请右键单击该解决方案，SBGenericsVTS (6 项目) ，然后选择 " **属性**"。  
  
2. 在 " **通用属性**" 中，选择 " **启动项目**"，然后选择 " **多启动项目**"。  
  
3. 依次选择 " **Server** "、" **Client20** " 和 " **Client40**"。 选择这三个项目的 " **启动** " 操作，并将 rest 设置为 " **无**"。  
  
4. 单击 **"确定"** ，然后按 F5 运行示例。
