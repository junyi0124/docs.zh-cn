---
title: 比较 project.json 和 csproj
description: 查看 project.json 和 csproj 元素之间的映射。
author: natemcmaster
ms.date: 03/13/2017
ms.openlocfilehash: c8638bc30ba09d8e8d464159aded60dcde4b8dc0
ms.sourcegitcommit: 32f0d6f4c01ddc6ca78767c3a30e3305f8cd032c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2020
ms.locfileid: "87427016"
---
# <a name="a-mapping-between-projectjson-and-csproj-properties"></a>project.json 和 csproj 属性之间的映射

作者 [Nate McMaster](https://github.com/natemcmaster)

.NET Core 工具的开发过程中实施了一项重要的设计更改，即不再支持 *project.json* 文件，而是将 .NET Core 项目转移到 MSBuild/csproj 格式。

本文介绍 *project.json* 中的设置如何以 MSBuild/csproj 格式表示，以便用户可学习如何使用新格式，并了解将项目升级到最新版本的工具时由迁移工具做出的更改。

## <a name="the-csproj-format"></a>csproj 格式

新格式 \*.csproj 是一种基于 XML 的格式。 以下示例演示使用 `Microsoft.NET.Sdk` 的 .NET Core 项目的根节点。 对于 Web 项目，所使用的 SDK 是 `Microsoft.NET.Sdk.Web`。

```xml
<Project Sdk="Microsoft.NET.Sdk">
...
</Project>
```

## <a name="common-top-level-properties"></a>常见顶级属性

### <a name="name"></a>NAME

```json
{
  "name": "MyProjectName"
}
```

不再支持。 在 csproj 中，这取决于项目文件名（通常与目录名称匹配）。 例如 `MyProjectName.csproj`。

默认情况下，项目文件名还指定 `<AssemblyName>` 和 `<PackageId>` 属性的值。

```xml
<PropertyGroup>
  <AssemblyName>MyProjectName</AssemblyName>
  <PackageId>MyProjectName</PackageId>
</PropertyGroup>
```

如果 `buildOptions\outputName` 属性是在 project.json 中定义的，`<AssemblyName>` 将具有不同于 `<PackageId>` 的其他值。
有关详细信息，请参阅[其他常用生成选项](#other-common-build-options)。

### <a name="version"></a>version

```json
{
  "version": "1.0.0-alpha-*"
}
```

使用 `VersionPrefix` 和 `VersionSuffix` 属性：

```xml
<PropertyGroup>
  <VersionPrefix>1.0.0</VersionPrefix>
  <VersionSuffix>alpha</VersionSuffix>
</PropertyGroup>
```

还可以使用 `Version` 属性，但这可能会在打包过程中替代版本设置：

```xml
<PropertyGroup>
  <Version>1.0.0-alpha</Version>
</PropertyGroup>
```

### <a name="other-common-root-level-options"></a>其他常用根级别选项

```json
{
  "authors": [ "Anne", "Bob" ],
  "company": "Contoso",
  "language": "en-US",
  "title": "My library",
  "description": "This is my library.\r\nAnd it's really great!",
  "copyright": "Nugetizer 3000",
  "userSecretsId": "xyz123"
}
```

```xml
<PropertyGroup>
  <Authors>Anne;Bob</Authors>
  <Company>Contoso</Company>
  <NeutralLanguage>en-US</NeutralLanguage>
  <AssemblyTitle>My library</AssemblyTitle>
  <Description>This is my library.
And it's really great!</Description>
  <Copyright>Nugetizer 3000</Copyright>
  <UserSecretsId>xyz123</UserSecretsId>
</PropertyGroup>
```

## <a name="frameworks"></a>框架

### <a name="one-target-framework"></a>一个目标框架

```json
{
  "frameworks": {
    "netcoreapp1.0": {}
  }
}
```

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp1.0</TargetFramework>
</PropertyGroup>
```

### <a name="multiple-target-frameworks"></a>多个目标框架

```json
{
  "frameworks": {
    "netcoreapp1.0": {},
    "net451": {}
  }
}
```

使用 `TargetFrameworks` 属性定义目标框架的列表。 使用分号来分隔多个框架值。

```xml
<PropertyGroup>
  <TargetFrameworks>netcoreapp1.0;net451</TargetFrameworks>
</PropertyGroup>
```

## <a name="dependencies"></a>依赖项

> [!IMPORTANT]
> 如果依赖项是一个**项目**而不是包，则格式不同。
> 有关详细信息，请参阅[依赖项类型](#dependency-type)部分。

### <a name="netstandardlibrary-metapackage"></a>NETStandard.Library 元包

```json
{
  "dependencies": {
    "NETStandard.Library": "1.6.0"
  }
}
```

```xml
<PropertyGroup>
  <NetStandardImplicitPackageVersion>1.6.0</NetStandardImplicitPackageVersion>
</PropertyGroup>
```

### <a name="microsoftnetcoreapp-metapackage"></a>Microsoft.NETCore.App 元包

```json
{
  "dependencies": {
    "Microsoft.NETCore.App": "1.0.0"
  }
}
```

```xml
<PropertyGroup>
  <RuntimeFrameworkVersion>1.0.3</RuntimeFrameworkVersion>
</PropertyGroup>
```

迁移项目中的 `<RuntimeFrameworkVersion>` 值由已安装的 SDK 版本确定。

### <a name="top-level-dependencies"></a>顶级依赖项

```json
{
  "dependencies": {
    "Microsoft.AspNetCore": "1.1.0"
  }
}
```

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore" Version="1.1.0" />
</ItemGroup>
```

### <a name="per-framework-dependencies"></a>依赖项（按框架）

```json
{
  "framework": {
    "net451": {
      "dependencies": {
        "System.Collections.Immutable": "1.3.1"
      }
    },
    "netstandard1.5": {
      "dependencies": {
        "Newtonsoft.Json": "9.0.1"
      }
    }
  }
}
```

```xml
<ItemGroup Condition="'$(TargetFramework)'=='net451'">
  <PackageReference Include="System.Collections.Immutable" Version="1.3.1" />
</ItemGroup>

<ItemGroup Condition="'$(TargetFramework)'=='netstandard1.5'">
  <PackageReference Include="Newtonsoft.Json" Version="9.0.1" />
</ItemGroup>
```

### <a name="imports"></a>导入

```json
{
  "dependencies": {
    "YamlDotNet": "4.0.1-pre309"
  },
  "frameworks": {
    "netcoreapp1.0": {
      "imports": [
        "dnxcore50",
        "dotnet"
      ]
    }
  }
}
```

```xml
<PropertyGroup>
  <PackageTargetFallback>dnxcore50;dotnet</PackageTargetFallback>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="YamlDotNet" Version="4.0.1-pre309" />
</ItemGroup>
```

### <a name="dependency-type"></a>依赖项类型

#### <a name="type-project"></a>类型：项目

```json
{
  "dependencies": {
    "MyOtherProject": "1.0.0-*",
    "AnotherProject": {
      "type": "project"
    }
  }
}
```

```xml
<ItemGroup>
  <ProjectReference Include="..\MyOtherProject\MyOtherProject.csproj" />
  <ProjectReference Include="..\AnotherProject\AnotherProject.csproj" />
</ItemGroup>
```

> [!NOTE]
> 这将打破 `dotnet pack --version-suffix $suffix` 确定项目引用的依赖项版本的方式。

#### <a name="type-build"></a>类型：生成

```json
{
  "dependencies": {
    "Microsoft.EntityFrameworkCore.Design": {
      "version": "1.1.0",
      "type": "build"
    }
  }
}
```

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="1.1.0" PrivateAssets="All" />
</ItemGroup>
```

#### <a name="type-platform"></a>类型：平台

```json
{
  "dependencies": {
    "Microsoft.NETCore.App": {
      "version": "1.1.0",
      "type": "platform"
    }
  }
}
```

csproj 中没有等效项。

## <a name="runtimes"></a>runtimes

```json
{
  "runtimes": {
    "win7-x64": {},
    "osx.10.11-x64": {},
    "ubuntu.16.04-x64": {}
  }
}
```

```xml
<PropertyGroup>
  <RuntimeIdentifiers>win7-x64;osx.10.11-x64;ubuntu.16.04-x64</RuntimeIdentifiers>
</PropertyGroup>
```

### <a name="standalone-apps-self-contained-deployment"></a>独立应用（独立部署）

在 project.json 中，定义 `runtimes` 部分意味着应用在生成和发布期间独立。
在 MSBuild 中，生成期间所有项目均*可移植*，但可发布为独立。

`dotnet publish --framework netcoreapp1.0 --runtime osx.10.11-x64`

有关详细信息，请参阅[独立部署 (SCD)](../deploying/index.md#publish-self-contained)。

## <a name="tools"></a>工具

```json
{
  "tools": {
    "Microsoft.EntityFrameworkCore.Tools.DotNet": "1.0.0-*"
  }
}
```

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="1.0.0" />
</ItemGroup>
```

> [!NOTE]
> csproj 中不支持工具上的 `imports`。 需要导入的工具无法用于新的 `Microsoft.NET.Sdk`。

## <a name="buildoptions"></a>buildOptions

另请参阅[文件](#files)。

### <a name="emitentrypoint"></a>emitEntryPoint

```json
{
  "buildOptions": {
    "emitEntryPoint": true
  }
}
```

```xml
<PropertyGroup>
  <OutputType>Exe</OutputType>
</PropertyGroup>
```

如果 `emitEntryPoint` 为 `false`，`OutputType` 的值会转换为 `Library`（这是默认值）：

```json
{
  "buildOptions": {
    "emitEntryPoint": false
  }
}
```

```xml
<PropertyGroup>
  <OutputType>Library</OutputType>
  <!-- or, omit altogether. It defaults to 'Library' -->
</PropertyGroup>
```

### <a name="keyfile"></a>keyFile

```json
{
  "buildOptions": {
    "keyFile": "MyKey.snk"
  }
}
```

`keyFile` 元素在 MSBuild 中扩展为三个属性：

```xml
<PropertyGroup>
  <AssemblyOriginatorKeyFile>MyKey.snk</AssemblyOriginatorKeyFile>
  <SignAssembly>true</SignAssembly>
  <PublicSign Condition="'$(OS)' != 'Windows_NT'">true</PublicSign>
</PropertyGroup>
```

### <a name="other-common-build-options"></a>其他常用生成选项

```json
{
  "buildOptions": {
    "warningsAsErrors": true,
    "nowarn": ["CS0168", "CS0219"],
    "xmlDoc": true,
    "preserveCompilationContext": true,
    "outputName": "Different.AssemblyName",
    "debugType": "portable",
    "allowUnsafe": true,
    "define": ["TEST", "OTHERCONDITION"]
  }
}
```

```xml
<PropertyGroup>
  <TreatWarningsAsErrors>true</TreatWarningsAsErrors>
  <NoWarn>$(NoWarn);CS0168;CS0219</NoWarn>
  <GenerateDocumentationFile>true</GenerateDocumentationFile>
  <PreserveCompilationContext>true</PreserveCompilationContext>
  <AssemblyName>Different.AssemblyName</AssemblyName>
  <DebugType>portable</DebugType>
  <AllowUnsafeBlocks>true</AllowUnsafeBlocks>
  <DefineConstants>$(DefineConstants);TEST;OTHERCONDITION</DefineConstants>
</PropertyGroup>
```

## <a name="packoptions"></a>packOptions

另请参阅[文件](#files)。

### <a name="common-pack-options"></a>常用包选项

```json
{
  "packOptions": {
    "summary": "numl is a machine learning library intended to ease the use of using standard modeling techniques for both prediction and clustering.",
    "tags": ["machine learning", "framework"],
    "releaseNotes": "Version 0.9.12-beta",
    "iconUrl": "http://numl.net/images/ico.png",
    "projectUrl": "http://numl.net",
    "licenseUrl": "https://raw.githubusercontent.com/sethjuarez/numl/master/LICENSE.md",
    "requireLicenseAcceptance": false,
    "repository": {
      "type": "git",
      "url": "https://raw.githubusercontent.com/sethjuarez/numl"
    },
    "owners": ["Seth Juarez"]
  }
}
```

```xml
<PropertyGroup>
  <!-- summary is not migrated from project.json, but you can use the <Description> property for that if needed. -->
  <PackageTags>machine learning;framework</PackageTags>
  <PackageReleaseNotes>Version 0.9.12-beta</PackageReleaseNotes>
  <PackageIcon>ico.png</PackageIcon>
  <PackageProjectUrl>http://numl.net</PackageProjectUrl>
  <PackageLicenseUrl>https://raw.githubusercontent.com/sethjuarez/numl/master/LICENSE.md</PackageLicenseUrl>
  <PackageRequireLicenseAcceptance>false</PackageRequireLicenseAcceptance>
  <RepositoryType>git</RepositoryType>
  <RepositoryUrl>https://raw.githubusercontent.com/sethjuarez/numl</RepositoryUrl>
  <!-- owners is not supported in MSBuild -->
</PropertyGroup>
```

MSBuild 中没有 `owners` 元素的等效项。 对于 `summary`，可以使用 MSBuild `<Description>` 属性。 `summary` 的值未自动迁移到该属性，因为该属性已映射到 [`description`](#other-common-root-level-options) 元素。  [PackageIconUrl 已被弃用](/nuget/reference/msbuild-targets#packageiconurl)，被 PackageIcon 取代。

## <a name="scripts"></a>脚本

```json
{
  "scripts": {
    "precompile": "generateCode.cmd",
    "postpublish": [ "obfuscate.cmd", "removeTempFiles.cmd" ]
  }
}
```

它们在 MSBuild 中的等效项是[目标](/visualstudio/msbuild/msbuild-targets)：

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="generateCode.cmd" />
</Target>

<Target Name="MyPostCompileTarget" AfterTargets="Publish">
  <Exec Command="obfuscate.cmd" />
  <Exec Command="removeTempFiles.cmd" />
</Target>
```

## <a name="runtimeoptions"></a>runtimeOptions

```json
{
  "runtimeOptions": {
    "configProperties": {
      "System.GC.Server": true,
      "System.GC.Concurrent": true,
      "System.GC.RetainVM": true,
      "System.Threading.ThreadPool.MinThreads": 4,
      "System.Threading.ThreadPool.MaxThreads": 25
    }
  }
}
```

此组中除“`System.GC.Server`”属性以外的所有设置与迁移过程中被提升为根对象的选项一并被置于项目文件夹下名为 runtimeconfig.template.json 的文件中：

```json
{
  "configProperties": {
    "System.GC.Concurrent": true,
    "System.GC.RetainVM": true,
    "System.Threading.ThreadPool.MinThreads": 4,
    "System.Threading.ThreadPool.MaxThreads": 25
  }
}
```

`System.GC.Server` 属性已迁移到 csproj 文件：

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
</PropertyGroup>
```

但可以在 csproj 以及 MSBuild 属性中设置所有这些值：

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
  <ConcurrentGarbageCollection>true</ConcurrentGarbageCollection>
  <RetainVMGarbageCollection>true</RetainVMGarbageCollection>
  <ThreadPoolMinThreads>4</ThreadPoolMinThreads>
  <ThreadPoolMaxThreads>25</ThreadPoolMaxThreads>
</PropertyGroup>
```

## <a name="shared"></a>共享

```json
{
  "shared": "shared/**/*.cs"
}
```

在 csproj 中不支持。 改为在 .nuspec 文件中创建包含内容文件。
有关详细信息，请参阅[包含内容文件](/nuget/schema/nuspec#including-content-files)。

## <a name="files"></a>文件

在 *project.json* 中，可将生成和打包操作扩展为从不同的文件夹进行编译和嵌入。
在 MSBuild 中，使用[项](/visualstudio/msbuild/common-msbuild-project-items)实现此操作。 以下示例是一个常见转换：

```json
{
  "buildOptions": {
    "compile": {
      "copyToOutput": "notes.txt",
      "include": "../Shared/*.cs",
      "exclude": "../Shared/Not/*.cs"
    },
    "embed": {
      "include": "../Shared/*.resx"
    }
  },
  "packOptions": {
    "include": "Views/",
    "mappings": {
      "some/path/in/project.txt": "in/package.txt"
    }
  },
  "publishOptions": {
    "include": [
      "files/",
      "publishnotes.txt"
    ]
  }
}
```

```xml
<ItemGroup>
  <Compile Include="..\Shared\*.cs" Exclude="..\Shared\Not\*.cs" />
  <EmbeddedResource Include="..\Shared\*.resx" />
  <Content Include="Views\**\*" PackagePath="%(Identity)" />
  <None Include="some/path/in/project.txt" Pack="true" PackagePath="in/package.txt" />
  
  <None Include="notes.txt" CopyToOutputDirectory="Always" />
  <!-- CopyToOutputDirectory = { Always, PreserveNewest, Never } -->

  <Content Include="files\**\*" CopyToPublishDirectory="PreserveNewest" />
  <None Include="publishnotes.txt" CopyToPublishDirectory="Always" />
  <!-- CopyToPublishDirectory = { Always, PreserveNewest, Never } -->
</ItemGroup>
```

> [!NOTE]
> 许多默认 [glob 模式](https://en.wikipedia.org/wiki/Glob_(programming))由 .NET Core SDK 自动添加。 有关详细信息，请参阅[默认编译包括](../project-sdk/overview.md#default-compilation-includes)。

所有 MSBuild `ItemGroup` 元素都支持`Include`、`Exclude` 和 `Remove`。

可使用 `PackagePath="path"` 修改 .nupkg 内的包布局。

除 `Content` 外，大多数项组需要显式添加要包括在包中的 `Pack="true"`。 `Content` 将被置于包中的 *content* 文件夹，因为 `<IncludeContentInPack>` 属性默认设置为 `true`。
有关详细信息，请参阅[在包中包含内容](/nuget/schema/msbuild-targets#including-content-in-a-package)。

`PackagePath="%(Identity)"` 是一种将包路径设置为项目相对文件路径的快捷方法。

## <a name="testrunner"></a>testRunner

### <a name="xunit"></a>xUnit

```json
{
  "testRunner": "xunit",
  "dependencies": {
    "dotnet-test-xunit": "<any>"
  }
}
```

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.0.0-*" />
  <PackageReference Include="xunit" Version="2.2.0-*" />
  <PackageReference Include="xunit.runner.visualstudio" Version="2.2.0-*" />
</ItemGroup>
```

### <a name="mstest"></a>MSTest

```json
{
  "testRunner": "mstest",
  "dependencies": {
    "dotnet-test-mstest": "<any>"
  }
}
```

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.0.0-*" />
  <PackageReference Include="MSTest.TestAdapter" Version="1.1.12-*" />
  <PackageReference Include="MSTest.TestFramework" Version="1.1.11-*" />
</ItemGroup>
```

## <a name="see-also"></a>请参阅

- [CLI 中更改的简要概述](cli-msbuild-architecture.md)
