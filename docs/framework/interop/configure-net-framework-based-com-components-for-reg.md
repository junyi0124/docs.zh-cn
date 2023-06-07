---
title: 如何：配置基于 .NET Framework 的 COM 组件以进行免注册激活
description: 配置基于 .NET 的 COM 组件以进行免注册激活。 安装程序需要 Win32 样式的应用程序清单和 .NET 组件清单。
ms.date: 03/30/2017
helpviewer_keywords:
- components [.NET Framework], manifest
- application manifests [.NET Framework]
- manifests [.NET Framework]
- registration-free COM interop, configuring .NET-based components
- activation, registration-free
ms.assetid: 32f8b7c6-3f73-455d-8e13-9846895bd43b
ms.openlocfilehash: ffeb342f286663ee7fe733ee617741e0ab30d0d8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96282868"
---
# <a name="how-to-configure-net-framework-based-com-components-for-registration-free-activation"></a>如何：配置基于 .NET Framework 的 COM 组件以进行免注册激活

基于 .NET Framework 的组件的免注册激活略复杂于 COM 组件的免注册激活。 安装需要两个清单：  
  
- COM 应用程序必须具有 Win32 样式的应用程序清单，以标识托管的组件。  
  
- 基于 .NET Framework 的组件必须具有组件清单，以提供运行时所需的激活信息。  
  
 本主题将介绍如何应用程序清单与应用程序关联，如何将组件清单与组件关联，以及如何在程序集中嵌入组件清单。  
  
## <a name="create-an-application-manifest"></a>创建应用程序清单  
  
1. 使用 XML 编辑器创建（或修改）COM 应用程序拥有的应用程序清单，该应用程序与一个或多个托管组件进行交互操作。  
  
2. 在文件开头插入以下标准标头：  
  
    ```xml  
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>  
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    </assembly>
    ```  
  
     有关清单元素及其属性的信息，请参阅[应用程序清单](/windows/desktop/SbsCs/application-manifests)。  
  
3. 标识清单的所有者。 以下示例中，`myComApp` 版本 1 拥有清单文件。  
  
    ```xml  
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>  
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">  
      <assemblyIdentity type="win32"
                        name="myOrganization.myDivision.myComApp"
                        version="1.0.0.0"
                        processorArchitecture="msil"
      />
    </assembly>  
    ```  
  
4. 标识依赖程序集。 以下示例中，`myComApp` 依赖于 `myManagedComp`。  
  
    ```xml  
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>  
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">  
      <assemblyIdentity type="win32"
                        name="myOrganization.myDivision.myComApp"
                        version="1.0.0.0"
                        processorArchitecture="x86"
                        publicKeyToken="8275b28176rcbbef"  
      />  
      <dependency>  
        <dependentAssembly>  
          <assemblyIdentity type="win32"
                        name="myOrganization.myDivision.myManagedComp"
                        version="6.0.0.0"
                        processorArchitecture="X86"
                        publicKeyToken="8275b28176rcbbef"  
          />  
        </dependentAssembly>  
      </dependency>  
    </assembly>  
    ```  
  
5. 保存并命名清单文件。 应用程序清单的名称是程序集可执行文件的名称后加 .manifest 扩展名。 例如，myComApp.exe 的应用程序清单文件名称是 myComApp.exe.manifest。  
  
可在与 COM 应用程序相同的目录中安装应用程序清单。 或者，可将其作为资源添加到应用程序的 .exe 文件。 有关详细信息，请参阅[关于并行程序集](/windows/desktop/SbsCs/about-side-by-side-assemblies-)。  
  
## <a name="create-a-component-manifest"></a>创建组件清单  
  
1. 使用 XML 编辑器创建组件清单，描述托管程序集。  
  
2. 在文件开头插入以下标准标头：  
  
    ```xml  
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>  
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    </assembly>
    ```  
  
3. 标识文件的所有者。 应用程序清单文件中 `<dependentAssembly>` 元素的 `<assemblyIdentity>` 元素必须与组件清单中的该元素相匹配。 以下示例中，`myManagedComp` 版本 1.2.3.4 拥有清单文件。  
  
    ```xml  
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>  
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">  
           <assemblyIdentity  
                        name="myOrganization.myDivision.myManagedComp"  
                        version="1.2.3.4"  
                        publicKeyToken="8275b28176rcbbef"  
                        processorArchitecture="msil"  
           />
    </assembly>
    ```  
  
4. 标识程序集中的每个类。 使用 `<clrClass>` 元素来唯一地标识托管程序集中的每个类。 该元素是 `<assembly>` 元素的子元素，具有下表中描述的属性。  
  
    |特性|描述|必需|  
    |---------------|-----------------|--------------|  
    |`clsid`|用于指定要激活的类的标识符。|是|  
    |`description`|用于通知用户组件相关信息的字符串。 空字符串为默认值。|否|  
    |`name`|用于表示托管类的字符串。|是|  
    |`progid`|用于后期绑定激活的标识符。|否|  
    |`threadingModel`|COM 线程模型。 “Both”为默认值。|否|  
    |`runtimeVersion`|指定要使用的公共语言运行时 (CLR) 版本。 如未指定此属性，并且尚未加载 CLR，将使用最近安装的早于 CLR 版本 4 的 CLR 版本加载组件。 如果指定 v1.0.3705、v1.1.4322 或 v2.0.50727，版本将自动向前滚到最近安装的早于 CLR 版本 4 的 CLR 版本（通常为 v2.0.50727）。 如果已加载其他 CLR 版本，并且可在进程内并行加载指定版本，那么将加载指定版本；否则使用已加载的 CLR。 这可能会导致加载失败。|否|  
    |`tlbid`|包含有关该类的类型信息的类型库的标识符。|否|  
  
     所有属性标记都区分大小写。 通过使用 OLE/COM ObjectViewer (Oleview.exe) 查看程序集的已导出类型库，可以获取 CLSID、ProgID、线程模型和运行时版本。  
  
     以下组件清单标识两个类 - `testClass1` 和 `testClass2`。  
  
    ```xml  
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>  
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">  
           <assemblyIdentity  
                        name="myOrganization.myDivision.myManagedComp"  
                        version="1.2.3.4"
                        publicKeyToken="8275b28176rcbbef"  
           />  
           <clrClass  
                        clsid="{65722BE6-3449-4628-ABD3-74B6864F9739}"  
                        progid="myManagedComp.testClass1"  
                        threadingModel="Both"  
                        name="myManagedComp.testClass1"  
                        runtimeVersion="v1.0.3705">  
           </clrClass>  
           <clrClass  
                        clsid="{367221D6-3559-3328-ABD3-45B6825F9732}"  
                        progid="myManagedComp.testClass2"  
                        threadingModel="Both"  
                        name="myManagedComp.testClass2"  
                        runtimeVersion="v1.0.3705">  
           </clrClass>  
           <file name="MyManagedComp.dll">  
           </file>  
    </assembly>  
    ```  
  
5. 保存并命名清单文件。 组件清单名称是程序集库名称后加 .manifest 扩展名。 例如，myManagedComp.dll 的名称是 myManagedComp.manifest。  
  
 必须将组件清单作为资源嵌入程序集。  
  
#### <a name="to-embed-a-component-manifest-in-a-managed-assembly"></a>将组件清单嵌入托管程序集  
  
1. 创建包含以下语句的资源脚本：  
  
     `RT_MANIFEST 1 myManagedComp.manifest`  
  
     在此语句中，`myManagedComp.manifest` 是正在嵌入的组件清单的名称。 对于此示例，脚本文件名称是 `myresource.rc`。  
  
2. 使用 Microsoft Windows 资源编译器 (rc.exe) 编译脚本。 在命令提示符处，键入下列命令：  
  
     `rc myresource.rc`  
  
     Rc.exe 生成 `myresource.res` 资源文件。  
  
3. 再次编译该程序集的源文件，并使用 /win32res 选项指定资源文件：  
  
    `/win32res:myresource.res`  
  
     同样，`myresource.res` 是包含嵌入资源的资源文件的名称。  
  
## <a name="see-also"></a>请参阅

- [免注册 COM 互操作](registration-free-com-interop.md)
- [免注册 COM 互操作的需求](/previous-versions/dotnet/netframework-4.0/f8h7012w(v=vs.100))
- [将 COM 组件配置为免注册激活](/previous-versions/dotnet/netframework-4.0/x65a421a(v=vs.100))
- [基于 .NET 组件的免注册激活：演练](/previous-versions/dotnet/articles/ms973915(v=msdn.10))
