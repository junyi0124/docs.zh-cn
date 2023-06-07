---
title: XML 序列化程序生成器工具 (Sgen.exe)
description: XML 序列化程序生成器为程序集中的类型创建 XML 序列化程序集，从而提高 XmlSerializer 的启动性能。
ms.date: 03/30/2017
ms.assetid: cc1d1f1c-fb26-4be9-885a-3fe84c81cec6
ms.openlocfilehash: c2f33236e39f61638118f45f0d5ab5385df27ac3
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95676512"
---
# <a name="xml-serializer-generator-tool-sgenexe"></a>XML 序列化程序生成器工具 (Sgen.exe)

XML 序列化程序生成器创建指定程序集中的类型的 XML 序列化程序集。 序列化程序集在序列化或反序列化指定类型的对象时，将改进 <xref:System.Xml.Serialization.XmlSerializer> 的启动性能。
  
## <a name="syntax"></a>语法

从命令行运行该工具。
  
```console  
sgen [options]  
```
  
> [!TIP]
> 要使 .NET Framework 工具正常发挥作用，必须正确设置 `Path`、`Include` 和 `Lib` 环境变量。 可以通过运行 SDKVars.bat（位于 \<SDK>\\\<version>\Bin 目录中）来设置这些环境变量。 必须在每个命令 shell 程序中执行 SDKVars.bat。
  
## <a name="parameters"></a>参数  
  
|选项|描述|  
|------------|-----------------|  
|**/a\[ssembly\]:** _filename_|为由 filename 指定的程序集或可执行文件中包含的所有类型生成序列化代码。 只能提供一个文件名。 如果该参数重复，将使用最后一个文件名。|  
|**/c\[ompiler\]:** _options_|指定要传递给 C# 编译器的选项。 支持所有传递到编译器的 csc.exe 选项。 这可用于指定应该对程序集进行签名，以及用于指定密钥文件。|  
|**/d\[ebug\]**|生成一个可用于调试器的映像。|  
|**/f\[orce\]**|强制覆盖同名的现有程序集。 默认值为 false。|  
|**/help 或 /?**|显示该工具的命令语法和选项。|  
|**/k\[eep\]**|取消在生成的源文件和其他临时文件编译到序列化程序集内之后对它们的删除操作。 这可用于确定工具是否正在为某个特定类型生成序列化代码。|  
|**/n\[ologo\]**|取消显示 Microsoft 启动版权标志。|  
|**/o\[ut\]:** _path_|指定要在其中保存生成的程序集的目录。 **注意：** 生成的程序集的名称由输入程序集的名称加上“xmlSerializers.dll”组成。|  
|**/p\[roxytypes\]**|仅生成 XML Web services 代理类型的序列化代码。|  
|**/r\[eference\]:** _assemblyfiles_|指定由需要 XML 序列化的类型引用的程序集。 接受多个程序集文件（由逗号分隔）。|  
|**/s\[ilent\]**|取消显示成功消息。|  
|**/t\[ype\]:** _type_|仅生成指定类型的序列化代码。|  
|**/v\[erbose\]**|显示详细输出，以进行调试。 列出目标程序集中无法使用 <xref:System.Xml.Serialization.XmlSerializer> 进行序列化的类型。|  
|**/?**|显示该工具的命令语法和选项。|  
  
## <a name="remarks"></a>备注  

 不使用 XML 序列化程序生成器时，<xref:System.Xml.Serialization.XmlSerializer> 在应用程序每次运行时为每个类型生成序列化代码和一个序列化程序集。 若要改进 XML 序列化的启动性能，请预先使用 Sgen.exe 工具生成那些程序集。 然后可以使用应用程序部署这些程序集。  
  
 XML 序列化程序生成器还可以改进使用 XML Web services 代理与服务器通信的客户端的性能，因为在第一次加载类型时，序列化进程将不会导致性能受损。  
  
 这些生成的程序集无法在 Web 服务的服务器端使用。 该工具仅能用于 Web 服务客户端和手动序列化方案。  
  
 如果包含要序列化的类型的程序集名为 MyType.dll，则关联的序列化程序集的名称将为 MyType.XmlSerializers.dll。  
  
## <a name="examples"></a>示例  

 下面的命令创建一个名为 Data.XmlSerializers.dll 的程序集，用于序列化名为 Data.dll 的程序集中包含的所有类型。  
  
```console  
sgen Data.dll
```  
  
 可以从代码中引用需要序列化和反序列化 Data.dll 中的类型的 Data.XmlSerializers.dll 程序集。  
  
## <a name="see-also"></a>请参阅

- [工具](../../framework/tools/index.md)
- [命令提示](../../framework/tools/developer-command-prompt-for-vs.md)
