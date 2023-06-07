---
title: XML 架构定义工具和 XML 序列化
description: XML 架构定义工具为 XSD 架构生成 C# 或 Visual Basic 类文件，并从库或可执行文件生成 XML 架构。
ms.date: 03/30/2017
helpviewer_keywords:
- Xsd.exe
- XML serialization, XML Schema Definition tool
- XML Schema Definition tool
- serialization, XML Schema Definition tool
ms.assetid: 3c03f855-f931-47ff-bbc6-50c0367a16e4
ms.openlocfilehash: 6451f3b5f6e8ec2c1454e5cf7ffb95a96b620214
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90554978"
---
# <a name="the-xml-schema-definition-tool-and-xml-serialization"></a>XML 架构定义工具和 XML 序列化

XML 架构定义工具（[XML 架构定义工具 (Xsd.exe)](xml-schema-definition-tool-xsd-exe.md)）是作为 Windows&reg; 软件开发工具包 (SDK) 的一部分随 .NET Framework 工具一起安装的。 该工具主要有下面两个用途：  
  
- 生成 C# 或 Visual Basic 类文件，而这些文件符合特定的 XML 架构定义语言 (XSD) 架构。 该工具将 XML 架构用作参数，并输出一个包含若干类的文件，在使用 <xref:System.Xml.Serialization.XmlSerializer> 进行序列化时，这些类符合这种架构。 有关如何使用该工具生成符合特定架构的类的信息，请参阅[如何：使用 XML 架构定义工具生成类和 XML 架构文档](xml-schema-def-tool-gen.md)。  
  
- 通过 .dll 或 .exe 文件生成 XML 架构文档。 若要查看已经创建或已使用特性进行修改的一组文件的架构，可以将 DLL 或 EXE 作为参数传递到该工具，以便生成 XML 架构。 有关如何使用该工具从一组类生成 XML 架构文档的信息，请参阅[如何：使用 XML 架构定义工具生成类和 XML 架构文档](xml-schema-def-tool-gen.md)。  
  
有关使用此工具的详细信息，请参阅 [XML 架构定义工具 (Xsd.exe)](xml-schema-definition-tool-xsd-exe.md)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataSet>
- [XML 序列化简介](introducing-xml-serialization.md)
- [XML 架构定义工具 (Xsd.exe)](xml-schema-definition-tool-xsd-exe.md)
- <xref:System.Xml.Serialization.XmlSerializer>
- [如何：序列化对象](how-to-serialize-an-object.md)
- [如何：反序列化对象](how-to-deserialize-an-object.md)
- [如何：使用 XML 架构定义工具生成类和 XML 架构文档](xml-schema-def-tool-gen.md)
- [XML 架构绑定支持](/previous-versions/dotnet/netframework-4.0/sh1e66zd(v=vs.100))
