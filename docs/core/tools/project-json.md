---
title: "project.json 参考"
description: "project.json 引用"
keywords: .NET, .NET Core, project.json
author: aL3891
ms.author: mairaw
manager: wpickett
ms.date: 09/30/2016
ms.topic: article
ms.prod: .net-core
ms.technology: .net-core-technologies
ms.devlang: dotnet
ms.assetid: 3aef32bd-ee2a-4e24-80f8-a2b615e0336d
translationtype: Human Translation
ms.sourcegitcommit: b20713600d7c3ddc31be5885733a1e8910ede8c6
ms.openlocfilehash: f870dc601a1df5dd663cd165bc19f70df9aa57f4

---

# <a name="projectjson-reference"></a>project.json 引用

project.json 文件用于 .NET Core 项目以定义项目元数据、编译信息和依赖项。 在本引用主题中，你将看到可在 project.json 文件中定义的所有属性的列表。

> [!NOTE]
> .NET Core 工具的未来版本将从 project.json 移动到基于 MSBuild 的项目。 对于新的 .NET Core 项目，建议继续使用 project.json 文件，因为发布工具时会提供一个将项目转换为 MSBuild 的路径。
>
> 有关详细信息，请参阅 .NET 博客上发布的 [project.json 的更改](https://blogs.msdn.microsoft.com/dotnet/2016/05/23/changes-to-project-json/)以及[使用 MSBuild 生成 .NET Core 项目](../tutorials/target-dotnetcore-with-msbuild.md)主题。

## <a name="overview"></a>概述

```
{
    "name": String,
    "version": String,
    "description": String,
    "copyright": String,
    "title": String,
    "entryPoint": String,
    "testRunner": String,
    "authors": String[],
    "language": String,
    "embedInteropTypes": Boolean,
    "preprocess": String or String[],
    "shared": String or String[],
    "dependencies": Object {
        version: String,
        type: String,
        target: String,
        include: String,
        exclude: String,
        suppressParent: String
    },
    "tools": Object,
    "scripts": Object,
    "buildOptions": Object {
        "define": String[],
        "nowarn": String[],
        "additionalArguments": String[],
        "warningsAsErrors": Boolean,
        "allowUnsafe": Boolean,
        "emitEntryPoint": Boolean,
        "optimize": Boolean,
        "platform": String,
        "languageVersion": String,
        "keyFile": String,
        "delaySign": Boolean,
        "publicSign": Boolean,
        "debugType": String,
        "xmlDoc": Boolean,
        "preserveCompilationContext": Boolean,
        "outputName": String,
        "compilerName": String,
        "compile": Object {
            "include": String or String[],
            "exclude": String or String[],
            "includeFiles": String or String[],
            "excludeFiles": String or String[],
            "builtIns": Object,
            "mappings": Object
        },
        "embed": Object {
            "include": String or String[],
            "exclude": String or String[],
            "includeFiles": String or String[],
            "excludeFiles": String or String[],
            "builtIns": Object,
            "mappings": Object
        },
        "copyToOutput": Object {
            "include": String or String[],
            "exclude": String or String[],
            "includeFiles": String or String[],
            "excludeFiles": String or String[],
            "builtIns": Object,
            "mappings": Object
        }
    },
    "publishOptions": Object {
        "include": String or String[],
        "exclude": String or String[],
        "includeFiles": String or String[],
        "excludeFiles": String or String[],
        "builtIns": Object,
        "mappings": Object
    },
    "runtimeOptions": Object {
        "configProperties": Object {
            "System.GC.Server": Boolean,
            "System.GC.Concurrent": Boolean,
            "System.GC.RetainVM": Boolean,
            "System.Threading.ThreadPool.MinThreads": Integer,
            "System.Threading.ThreadPool.MaxThreads": Integer
        },
        "framework": Object {
            "name": String,
            "version": String,
        },
        "applyPatches": Boolean
    },
    "packOptions": Object {
        "summary": String,
        "tags": String[],
        "owners": String[],
        "releaseNotes": String,
        "iconUrl": String,
        "projectUrl": String,
        "licenseUrl": String,
        "requireLicenseAcceptance": Boolean,
        "repository": Object {
            "type": String,
            "url": String
        },
        "files": Object {
            "include": String or String[],
            "exclude": String or String[],
            "includeFiles": String or String[],
            "excludeFiles": String or String[],
            "builtIns": Object,
            "mappings": Object
        }
    },
    "analyzerOptions": Object {
        "languageId": String
    },
    "configurations": Object,
    "frameworks": Object {
        "dependencies": Object {
            version: String,
            type: String,
            target: String,
            include: String,
            exclude: String,
            suppressParent: String
        },        
        "frameworkAssemblies": Object,
        "wrappedProject": String,
        "bin": Object {
            assembly: String
        }
    },
    "runtimes": Object,
    "userSecretsId": String
}
```

## <a name="name"></a>name
类型：String

项目名称，用于程序集名和包名。 如果未指定此属性，则使用顶层文件夹名称。

例如: 

```json
{
    "name": "MyLibrary"
}
```

## <a name="version"></a>version
类型：String

项目的 [Semver](http://semver.org/spec/v1.0.0.html) 版本，同样适用于 NuGet 包。

例如: 

```json
{
    "version": "1.0.0-*"
}
```

## <a name="description"></a>说明
类型：String

项目的详细说明。 在程序集属性中使用。

例如: 

```json
{
    "description": "This is my library and it's really great!"
}
```

## <a name="copyright"></a>版权
类型：String

项目的版权信息。 在程序集属性中使用。

例如: 

```json
{
    "copyright": "Fabrikam 2016"
}
```

## <a name="title"></a>标题
类型：String

项目的友好名称，可以包含使用 `name` 属性时不允许的空格和特殊字符。 在程序集属性中使用。

例如: 

```json
{
    "title": "My Library"
}
```

## <a name="entrypoint"></a>entryPoint
类型：String

项目的入口点方法。 默认值为 `Main`。

例如：

```json
{
    "entryPoint": "ADifferentMethod"
}
```
    
## <a name="testrunner"></a>testRunner
类型：String

此项目要使用的测试运行程序名称，例如 [NUnit](http://nunit.org/) 或 [xUnit](http://xunit.github.io/)。 设置此值还可将项目标记为测试项目。

例如: 

```json
{
    "testRunner": "NUnit"
}
```

## <a name="authors"></a>作者
类型：String[]

含有项目作者名的字符串数组。

例如: 

```json
{
    "authors": ["Anne", "Bob"]
}
```

## <a name="language"></a>语言
类型：String

项目的（人类）语言。 对应于“非特定语言”编译器参数。

例如: 

```json
{
    "language": "en-US"
}
```

## <a name="embedinteroptypes"></a>embedInteropTypes
类型：Boolean

若要在程序集中嵌入 COM 互操作类型，则为 `true`；否则为 `false`。 

例如：

```json
{
    "embedInteropTypes": true
}
```

## <a name="preprocess"></a>预处理
类型：String 或带有通配模式的 String[]

指定要包含在预处理中的文件。

例如: 

```json
{
    "preprocess": "compiler/preprocess/**/*.cs"
}
```

## <a name="shared"></a>共享
类型：String 或带有通配模式的 String[]

指定共享的文件，适用于库导出。

例如: 

```json
{
    "shared": "shared/**/*.cs"
}
```

## <a name="dependencies"></a>依赖项
类型：Object

定义项目的包依赖项的一个对象，此对象的每个键是包名称，每个值包含版本信息。
有关详细信息，请参阅 NuGet 文档站点上的 [依赖项解析](https://docs.nuget.org/ndocs/consume-packages/dependency-resolution#dependency-resolution-in-nuget-3-x)文章。

例如: 

```json
    "dependencies": {
        "System.Reflection.Metadata": "1.3.0",
        "Microsoft.Extensions.JsonParser.Sources": {
          "type": "build",
          "version": "1.0.0-rc2-20221"
        },
        "Microsoft.Extensions.HashCodeCombiner.Sources": {
          "type": "build",
          "version": "1.1.0-alpha1-21456"
        },
        "Microsoft.Extensions.DependencyModel": "1.0.0-*"
    }
```

### <a name="version"></a>version
类型：String

指定依赖项的版本或版本范围。 使用 \* 通配符指定[浮动依赖项版本](https://docs.nuget.org/ndocs/consume-packages/dependency-resolution#floating-versions)。

例如: 

```json
"dependencies": { 
    "Newtonsoft.Json": { 
        "version": "9.0.1" 
    }
}
```

### <a name="type"></a>类型
类型：String

指定依赖项的类型。 可以是以下值之一：`default`、`build` 或 `platform`。 默认值为 `default`。

`build` 为开发依赖项，仅适用于生成时。 这意味着不应将包作为依赖项发布或添加到输出 `.nupkg` 文件。 将 [supressParent](#supressparent) 设置为 `all` 具有相同的效果。

`platform` 引用共享 SDK。 有关详细信息，请参阅 [.NET Core 应用程序部署](../deploying/index.md)主题中的“使用第三方依赖项部署依赖于框架的部署”部分。

例如：

```json
 "dependencies": {
   "Microsoft.NETCore.App": {
     "type": "platform",
     "version": "1.0.0"
   }
 }
```

### <a name="target"></a>target
类型：String

限制依赖项以仅匹配 `project` 或 `package`。

### <a name="include"></a>include
类型：String

包括部分依赖项包。 可以使用一个或多个下列标志：`all`、`runtime`、`compile`、`build`、`contentFiles`、`native`、`analyzers` 或 `none`。
多个标志由逗号分隔的列表定义。
有关详细信息，请参阅 NuGet 存储库上的[管理依赖项包资产](https://github.com/NuGet/Home/wiki/%5BSpec%5D-Managing-dependency-package-assets)规范。

例如: 

```json
{
  "dependencies": {
    "packageA": {
      "version": "1.0.0",
      "include": "runtime"
    }
  }
}
```

### <a name="exclude"></a>exclude
类型：String

排除部分依赖项包。 可以是一个或多个下列标志：`all`、`runtime`、`compile`、`build`、`contentFiles`、`native`、`analyzers` 或 `none`。
多个标志由逗号分隔的列表定义。
有关详细信息，请参阅 NuGet 存储库上的[管理依赖项包资产](https://github.com/NuGet/Home/wiki/%5BSpec%5D-Managing-dependency-package-assets)规范。

例如: 

```json
{
  "dependencies": {
    "packageA": {
      "version": "1.0.0",
      "exclude": "contentFiles"
    }
  }
}
```

### <a name="supressparent"></a>supressParent
类型：String

为项目使用者定义其他排除情况。 可以是下列标志之一：`all`、`runtime`、`compile`、`build`、`contentFiles`、`native`、`analyzers` 或 `none`。
有关详细信息，请参阅 NuGet 存储库上的[管理依赖项包资产](https://github.com/NuGet/Home/wiki/%5BSpec%5D-Managing-dependency-package-assets)规范。

```json
{
  "dependencies": {
    "packageA": {
      "version": "1.0.0",
      "suppressParent": "compile"
    }
  }
}
```

## <a name="tools"></a>工具
类型：Object

定义用作当前项目工具（而不是用作引用）的包依赖项的一个对象。 此处定义的包可用于生成过程中运行的脚本，但不可访问项目自身的代码。 例如，工具可以包括代码生成器或执行打包相关任务的后期生成工具。

例如: 

```json
{
    "tools": {
    "MyObfuscator": "1.2.4"
    }
}
```

## <a name="scripts"></a>脚本
类型：Object

定义在生成过程中所运行脚本的一个对象。 此对象中的每个键都可以确定生成过程中该脚本运行的位置。 每个值是具有要运行的脚本的字符串，或者是包含将按顺序运行的脚本的字符串数组。
支持的事件为：
* precompile
* postcompile
* prepublish
* postpublish

例如: 

```json
{
    "scripts": {
        "precompile": "generateCode.cmd",
        "postcompile": [ "obfuscate.cmd", "removeTempFiles.cmd" ]
    }
}
```

## <a name="buildoptions"></a>buildOptions
类型：Object

其属性控制编译各个方面的一个对象。 下面列出了有效属性。 也可以按[框架部分](#frameworks)中所述目标框架进行指定。

例如: 

```json
    "buildOptions": {
      "allowUnsafe": true,
      "emitEntryPoint": true
    }
```

### <a name="define"></a>定义
类型：String[]

可在代码的条件编译中使用的“DEBUG”或“TRACE”等定义的列表。

例如: 

```json
{
    "buildOptions": {
        "define": ["TEST", "OTHERCONDITION"]
    }
}
```

### <a name="nowarn"></a>nowarn
类型：String[]

要忽略的警告列表。

例如：

```json
{
    "buildOptions": {
        "nowarn": ["CS0168", "CS0219"]
    }
}
```

这会忽略警告 `The variable 'var' is assigned but its value is never used` 和 `The variable 'var' is assigned but its value is never used`

### <a name="additionalarguments"></a>additionalArguments
类型：String[]

将传递给编译器的额外参数的列表。

例如: 

```json
{
    "buildOptions": {
        "additionalArguments": ["/parallel", "/nostdlib"]
    }
}
```

### <a name="warningsaserrors"></a>warningsAsErrors
类型：Boolean

若要将警告视为错误，则为 `true`；否则为 `false`。 默认值为 `false`。

例如: 

```json
{
    "buildOptions": {
        "warningsAsErrors": true
    }
}
```

### <a name="allowunsafe"></a>allowUnsafe
类型：Boolean

若要使此项目允许不安全代码，则为 `true`；否则为 `false`。 默认值为 `false`。

例如: 

```json
{
    "buildOptions": {
        "allowUnsafe": true
    }
}
```

### <a name="emitentrypoint"></a>emitEntryPoint
类型：Boolean

若要创建可执行文件，则为 `true`；否则为 `false`。 默认值为 `false`。

例如: 

```json
{
    "buildOptions": {
        "emitEntryPoint": true
    }
}
```

### <a name="optimize"></a>optimize
类型：Boolean

若要启用编译器以优化此项目中的代码，则为 `true`；否则为 `false`。 默认值为 `false`。

例如: 

```json
{
    "buildOptions": {
        "optimize": true
    }
}
```

### <a name="platform"></a>平台
类型：String

目标平台的名称，例如 AnyCpu、x86 或 x64。

例如: 

```json
{
    "buildOptions": {
        "platform": "x64"
    }
}
```

### <a name="languageversion"></a>languageVersion
类型：String

编译器使用的语言版本：ISO-1、ISO-2、3、4、5、6 或默认值。

例如: 

```json
{
    "buildOptions": {
        "languageVersion": "5"
    }
}
```

### <a name="keyfile"></a>keyFile
类型：String

用于对此程序集签名的密钥文件的路径。

例如: 

```json
{
    "buildOptions": {
        "keyFile": "../keyfile.snk"
    }
}
```

### <a name="delaysign"></a>delaySign
类型：Boolean

若要延迟签名，则为 `true`；否则为 `false`。 默认值为 `false`。

例如: 

```json
{
    "buildOptions": {
        "delaySign": true
    }
}
```

### <a name="publicsign"></a>publicSign
类型：Boolean

若要启用对生成程序集的签名，则为 `true`；否则为 `false`。 默认值为 `false`。

例如: 

```json
{
    "buildOptions": {
        "publicSign": true
    }
}
```

### <a name="debugtype"></a>debugType
类型：String

指示要生成的符号文件（PDB 文件）的类型。 选项为“可移植”（适用于 .NET Core 项目）或“完整”（仅适用于传统的 Windows PDB 文件）。

例如: 

```json
{
    "buildOptions": {
        "debugType": "portable"
    }
}
```

### <a name="xmldoc"></a>xmlDoc
类型：Boolean

若要从源代码的三斜杠注释中生成 XML 文档，则为 `true`；否则为 `false`。 默认值为 `false`。

例如: 

```json
{
    "buildOptions": {
        "xmlDoc": true
    }
}
```

### <a name="preservecompilationcontext"></a>preserveCompilationContext
类型：Boolean

若要保留引用程序集和其他上下文数据以实现运行时编译，则为 `true`；否则为 `false`。 默认值为 `false`。

例如: 

```json
{
    "buildOptions": {
        "preserveCompilationContext": true
    }
}
```

### <a name="outputname"></a>outputName
类型：String

更改输出文件的名称。 

例如: 

```json
{
    "buildOptions": {
        "outputName": "MyApp"
    }
}
```

### <a name="compilername"></a>compilerName
类型：String

用于此项目的编译器名称。 默认值为 `csc`。 目前，支持 `csc`（C# 编译器）或 `fsc`（F# 编译器）。
 
例如: 

```json
{
    "compilerName": "fsc"
}
```
    
### <a name="compile"></a>编译
类型：Object

包含编译配置的属性的一个对象。

#### <a name="include"></a>include
类型：String 或带有通配模式的 String[]。

指定要在生成中包括的文件。 这些模式位于项目文件夹中。 默认为“无”。

例如: 

```json
{
    "include":["wwwroot", "Views"]
}
```

#### <a name="exclude"></a>exclude
类型：String 或带有通配模式的 String[]。

指定要从生成中排除的文件。 排除模式的优先级高于包括模式，因此将排除在这两种模式下找到的文件。 这些模式位于项目文件夹中。 默认为“无”。

例如: 

```json
{
    "exclude": ["bin/**", "obj/**"]
}
```

#### <a name="includefiles"></a>includeFiles

类型：String 或带有通配模式的 String[]。

要包括的文件路径的列表。 这些路径位于项目文件夹中。 此列表的优先级高于包括和排除通配模式，因此仍将包括此处列出的文件和排除通配模式中的文件。 默认为“无”。

例如: 

```json
{
    "includeFiles": []
}
```

#### <a name="excludefiles"></a>excludeFiles

类型：String 或带有通配模式的 String[]。

要排除的文件路径的列表。 这些路径位于项目文件夹中。 此列表的优先级高于通配模式和包括路径，因此将排除找到的所有文件。 默认为“无”。

例如: 

```json
{
    "excludeFiles":[],
}
```

#### <a name="builtins"></a>builtIns

类型：Object

系统提供默认值。 可以具有 `include` 和 `exclude` 通配模式，其中合并了 `include` 和 `exclude` 属性的对应值。

例如: 

```json
{
    "builtIns":{}
}
```

#### <a name="mappings"></a>映射
类型：Object

该对象的键表示输出布局中的目标路径。

值为表示要包括的文件所在源路径的字符串或对象。  该对象代表可以拥有其自身的 `include`、`exclude`、`includeFiles` 和 `excludeFiles` 部分。

字符串示例：

```json
{
    "mappings": {
        "dest/path": "./src/path"
    }
}
```

对象示例：

```json
{
    "mappings": {
        "dest/path":{
            "include":"./src/path"
        }
    }
}
```

### <a name="embed"></a>嵌入
类型：Object

包含编译配置的属性的一个对象。

#### <a name="include"></a>include
类型：String 或带有通配模式的 String[]。

```json
{
    "include":["wwwroot", "Views"]
}
```

#### <a name="exclude"></a>exclude
类型：String 或带有通配模式的 String[]。

指定要从生成中排除的文件。

例如: 

```json
{
    "exclude": ["bin/**", "obj/**"]
}
```

#### <a name="includefiles"></a>includeFiles

类型：String 或带有通配模式的 String[]。

```json
{
    "includeFiles":[],
}
```

#### <a name="excludefiles"></a>excludeFiles

类型：String 或带有通配模式的 String[]。

```json
{
    "excludeFiles":[],
}
```

#### <a name="builtins"></a>builtIns
类型：Object

```json
{
    "builtIns":{}
}
```

#### <a name="mappings"></a>映射
类型：Object

该对象的键表示输出布局中的目标路径。

值为表示要包括的文件所在源路径的字符串或对象。  该对象代表可以拥有其自身的 `include`、`exclude`、`includeFiles` 和 `excludeFiles` 部分。

字符串示例：

```json
{
    "mappings": {
        "dest/path": "./src/path"
    }
}
```

对象示例：

```json
{
    "mappings": {
        "dest/path":{
            "include":"./src/path"
        }
    }
}
```

### <a name="copytooutput"></a>copyToOutput
类型：Object

包含编译配置的属性的一个对象。

#### <a name="include"></a>include
类型：String 或带有通配模式的 String[]。

```json
{
    "include":["wwwroot", "Views"]
}
```

#### <a name="exclude"></a>exclude
类型：String 或带有通配模式的 String[]。

指定要从生成中排除的文件。

例如: 

```json
{
    "exclude": ["bin/**", "obj/**"]
}
```

#### <a name="includefiles"></a>includeFiles

类型：String 或带有通配模式的 String[]。

```json
{
    "includeFiles":[],
}
```

#### <a name="excludefiles"></a>excludeFiles

类型：String 或带有通配模式的 String[]。

```json
{
    "excludeFiles":[],
}
```

#### <a name="builtins"></a>builtIns
类型：Object

```json
{
    "builtIns":{}
}
```

#### <a name="mappings"></a>映射
类型：Object

该对象的键表示输出布局中的目标路径。

值为表示要包括的文件所在源路径的字符串或对象。  该对象代表可以拥有其自身的 `include`、`exclude`、`includeFiles` 和 `excludeFiles` 部分。

字符串示例：

```json
{
    "mappings": {
        "dest/path": "./src/path"
    }
}
```

对象示例：

```json
{
    "mappings": {
        "dest/path":{
            "include":"./src/path"
        }
    }
}
```

## <a name="publishoptions"></a>publishOptions
类型：Object

包含编译配置的属性的一个对象。

### <a name="include"></a>include
类型：String 或带有通配模式的 String[]。

```json
{
    "include":["wwwroot", "Views"]
}
```

### <a name="exclude"></a>exclude
类型：String 或带有通配模式的 String[]。

指定要从生成中排除的文件。

例如: 

```json
{
    "exclude": ["bin/**", "obj/**"]
}
```

### <a name="includefiles"></a>includeFiles

类型：String 或带有通配模式的 String[]。

```json
{
    "includeFiles":[],
}
```

### <a name="excludefiles"></a>excludeFiles

类型：String 或带有通配模式的 String[]。

```json
{
    "excludeFiles":[],
}
```

### <a name="builtins"></a>builtIns
类型：Object

```json
{
    "builtIns":{}
}
```

### <a name="mappings"></a>映射
类型：Object

该对象的键表示输出布局中的目标路径。

值为表示要包括的文件所在源路径的字符串或对象。  该对象代表可以拥有其自身的 `include`、`exclude`、`includeFiles` 和 `excludeFiles` 部分。

字符串示例：

```json
{
    "mappings": {
        "dest/file": "./src/file",
        "dest/folder/": "./src/folder/**/*"
    }
}
```

对象示例：

```json
{
    "mappings": {
        "dest/file":{
            "include":"./src/file"
        },
        "dest/folder/":{
            "include":"./src/folder/**/*"
        }
    }
}
```

## <a name="runtimeoptions"></a>runtimeOptions
类型：Object

指定在初始化期间向运行时提供的参数。

### <a name="configproperties"></a>configProperties
类型：Object

包含用于配置运行时和框架的配置属性。

#### <a name="systemgcserver"></a>System.GC.Server
类型：Boolean

若要启用服务器垃圾回收，则为 `true`；否则为 `false`。 默认值为 `false`。

例如: 

```json
{
    "runtimeOptions": {
        "configProperties": {
            "System.GC.Server": true
        }
    }
}
```

#### <a name="systemgcconcurrent"></a>System.GC.Concurrent
类型：Boolean

若要启用并发垃圾回收，则为 `true`；否则为 `false`。 默认值为 `false`。

例如: 

```json
{
    "runtimeOptions": {
        "configProperties": {
            "System.GC.Concurrent": true
        }
    }
}
```

#### <a name="systemgcretainvm"></a>System.GC.RetainVM
类型：Boolean

若要将应删除的段放在备用列表上供将来使用，而不是将其释放回操作系统 (OS)，则为 `true`；否则为 `false`。
默认值为 `false`。

例如: 

```json
{
    "runtimeOptions": {
        "configProperties": {
            "System.GC.RetainVM": true
        }
    }
}
```

#### <a name="systemthreadingthreadpoolminthreads"></a>System.Threading.ThreadPool.MinThreads
类型：Integer

替代 ThreadPool 工作线程池的最小线程数。

```json
{
    "runtimeOptions": {
        "configProperties": {
            "System.Threading.ThreadPool.MinThreads": 4
        }
    }
}
```

#### <a name="systemthreadingthreadpoolmaxthreads"></a>System.Threading.ThreadPool.MaxThreads
类型：Integer

替代 ThreadPool 工作线程池的最大线程数。

```json
{
    "runtimeOptions": {
        "configProperties": {
            "System.Threading.ThreadPool.MaxThreads": 25
        }
    }
}
```

### <a name="framework"></a>框架
类型：Object

包含激活应用程序时要使用的共享框架属性。 本部分指出了该应用程序是旨在使用共享的可再发行框架的可移植应用。

#### <a name="name"></a>name
类型：String

共享框架的名称。

```json
{
    "runtimeOptions": {
        "framework": {
            "name": "Microsoft.DotNetCore"
        }
    }
}
```

#### <a name="version"></a>version
类型：String

共享框架的版本。

```json
{
    "runtimeOptions": {
        "framework": {
            "version": "1.0.1"
        }
    }
}
```

### <a name="applypatches"></a>applyPatches
类型：Boolean

若为 `true`，则使用相同或更高版本的框架，其中仅 `SemVer` 修补程序字段有所不同。 `false` 用于方便主机仅使用确切的框架版本。 默认值为 `true`。

```json
{
    "runtimeOptions": {
        "applyPatches": false
    }
}
```

## <a name="packoptions"></a>packOptions
类型：Object

定义将项目输出打包到 NuGet 包的相关选项。

### <a name="summary"></a>摘要
类型：String

项目的简短说明。

例如: 

```json
{
    "packOptions": {
        "summary": "This is my library."
    }
}
```

### <a name="tags"></a>标记
类型：String[]

含有项目标记的字符串数组，用于在 NuGet 中执行搜索。

例如: 

```json
{
    "packOptions": {
        "tags": ["hyperscale", "cats"]
    }
}
```

### <a name="owners"></a>所有者
类型：String[]

含有项目所有者名称的字符串数组。

例如: 

```json
{
    "packOptions": {
        "owners": ["Fabrikam", "Microsoft"]
    }
}
```

### <a name="releasenotes"></a>releaseNotes
类型：String

项目的发行说明。

例如: 

```json
{
    "packOptions": {
        "releaseNotes": "Initial version, implemented flimflams."
    }
}
```

### <a name="iconurl"></a>iconUrl
类型：String

将在包资源管理器等各种位置使用的图标的 URL。

例如: 

```json
{
    "packOptions": {
        "iconUrl": "http://www.mylibrary.gov/favicon.ico"
    }
}
```

### <a name="projecturl"></a>projectUrl
类型：String

项目主页的 URL。

例如: 

```json
{
    "packOptions": {
        "projectUrl": "http://www.mylibrary.gov"
    }
}
```

### <a name="licenseurl"></a>licenseUrl
类型：String

项目使用的许可证的 URL。

例如: 

```json
{
    "packOptions": {
        "licenseUrl": "http://www.mylibrary.gov/licence"
    }
}
```

### <a name="requirelicenseacceptance"></a>requireLicenseAcceptance
类型：Boolean

若要导致安装包时显示提示接受包许可证，则为 `true`；否则为 `false`。 仅适用于 NuGet 包，其他情况下忽略。 默认值为 `false`。

例如: 

```json
{
    "packOptions": {
        "requireLicenseAcceptance": true
    }
}
```
   
### <a name="repository"></a>储存库
类型：Object

包含存储项目的存储库的相关信息。

#### <a name="type"></a>类型
类型：String

存储库的类型。 默认值为“git”。

例如: 

```json
{
    "packOptions": {
        "repository": {
            "type": "git"
        }
    }
}
```

#### <a name="url"></a>URL
类型：String

存储项目的存储库的 URL。

例如: 

```json
{
    "packOptions": {
        "repository": {
            "url": "http://github.com/dotnet/corefx"
        }
    }
}
```

### <a name="files"></a>文件
类型：Object

#### <a name="include"></a>include
类型：String 或带有通配模式的 String[]。

```json
{
    "include":["wwwroot", "Views"]
}
```

#### <a name="exclude"></a>exclude
类型：String 或带有通配模式的 String[]。

指定要从生成中排除的文件。

例如: 

```json
{
    "exclude": ["bin/**", "obj/**"]
}
```

#### <a name="includefiles"></a>includeFiles

类型：String 或带有通配模式的 String[]。

```json
{
    "includeFiles":[]
}
```

#### <a name="excludefiles"></a>excludeFiles

类型：String 或带有通配模式的 String[]。

```json
{
    "excludeFiles":[]
}
```

#### <a name="builtins"></a>builtIns
类型：Object

```json
{
    "builtIns":{}
}
```

#### <a name="mappings"></a>映射
类型：Object

该对象的键表示输出布局中的目标路径。

值为表示要包括的文件所在源路径的字符串或对象。  对象代表可以拥有其自身的 `include`、`exclude`、`includeFiles` 和 `excludeFiles` 部分。 

字符串示例：

```json
{
    "mappings": {
        "dest/path": "./src/path"
    }
}
```

对象示例：

```json
{
    "mappings": {
        "dest/path":{
            "include":"./src/path"
        }
    }
}
```

## <a name="analyzeroptions"></a>analyzerOptions
类型：Object

含有代码分析器所使用属性的一个对象。

例如: 

```json
{
    "analyzerOptions": { }
}
```

### <a name="languageid"></a>languageId
类型：String

要分析的语言的 ID。 “cs”表示 C#、“vb”表示 Visual Basic，“fs”表示 F#。

例如: 

```json
"analyzerOptions": {
    "languageId": "vb"
}
```

## <a name="configurations"></a>配置
类型：Object

其属性定义此项目的不同配置（如 Debug 和 Release）的一个对象。 每个值都是一个对象，可以包含具有此配置特定选项的 `buildOptions` 对象。

例如: 

```json
"configurations": {
    "Release": {
        "buildOptions": {
            "allowUnsafe": false
        }
    }
}
```

## <a name="frameworks"></a>框架
类型：Object

指定此项目支持的框架，如 .NET Framework 或通用 Windows 平台 (UWP)。 必须是有效的目标框架名字对象 (TFM)。 每个值都是一个对象，可以包含特定于此框架的信息（如 `buildOptions`、`analyzerOptions`、`dependencies`）以及以下各部分中的属性。

例如: 

```json
"frameworks": {
    "netcoreapp1.0": {
        "buildOptions": {
            "define": ["FOO", "BIZ"]
        }
    }
}
```

### <a name="dependencies"></a>依赖项
类型：Object

特定于此框架的依赖项。 这在无法跨所有目标简单指定包级别依赖项的情况下非常有用。 此问题的原因包括一个目标缺少其他目标拥有的内置支持，或者需要不同于其他目标的依赖项版本。 若要查看此节点的其他属性列表，请参阅前面的[依赖项](#dependencies)部分。

例如: 

```json
    "frameworks": {
        "netstandard1.5": {
        "dependencies": {
            "Microsoft.Extensions.JsonParser.Sources": "1.0.0-rc2-20221"
        }
    }
}
```

### <a name="frameworkassemblies"></a>frameworkAssemblies
类型：Object

类似于依赖项，但包含对 GAC 中的程序集（非 NuGet 包）的引用。 还可以指定要使用的版本和依赖项类型。 这适用于面向 .NET Framework 和可移植类库 (PCL) 目标的情况。 仅可以使用 Windows 上所指定的对象生成项目。

例如: 

```json
"frameworks": {
    "net451": {
        "frameworkAssemblies": {
            "System.Runtime": {
                "type": "build",
                "version": "4.0.0"
            }
        }
    }
}
```

### <a name="wrappedproject"></a>wrappedProject
类型：String

指定依赖项项目的位置。 

例如: 

```json
"frameworks": {
    "net451": {
        "wrappedProject": "MyProject.csproj"
    }
}
```

### <a name="bin"></a>bin
类型：Object

用于包装 DLL 文件。 可以引用和生成包含此 DLL 的包。 

仅包含一个字符串属性 `assembly`，其值为程序集路径。   

例如: 

```json
"frameworks": {
    "netcoreapp1.0": {
        "bin": {
            "assembly": "c:/otherProject/otherdll.dll"
        }
    }
}
```

## <a name="runtimes"></a>runtimes
类型：Object

（发布[独立部署](../deploying/index.md#self-contained-deployments-scd)时使用的）项目支持的[运行时标识符 (RID)](../rid-catalog.md) 列表。

例如: 

```json
"runtimes": {
    "win7-x64": {},
    "win8-x64": {},
    "win81-x64": {},
    "win10-x64": {},
    "osx.10.11-x64": {},
    "ubuntu.16.04-x64": {}
}
```

## <a name="usersecretsid"></a>userSecretsId
类型：String

指定在开发时使用的用户机密标识符。 有关详细信息，请参阅[在开发期间安全存储应用密钥](https://docs.asp.net/en/latest/security/app-secrets.html)。

例如: 

```json
{
    "userSecretsId": "aspnet-WebApp1-c23d27a4-eb88-4b18-9b77-2a93f3b15119"
}
```



<!--HONumber=Nov16_HO1-->


