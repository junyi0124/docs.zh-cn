---
title: 'Wfc.exe (工作流命令行编译器工具) '
description: 了解 wfc.exe （工作流命令行编译器工具）。
ms.date: 10/10/2020
helpviewer_keywords:
- wfc [Workflow]
- compiler tool
- wfc.exe
- Workflow, compilation
- Workflow, XOML files
- Workflow, wcf
ms.openlocfilehash: 01cbfeb72e19f727a3a470059047a2192228c394
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293853"
---
# <a name="wfcexe-workflow-command-line-compiler-tool"></a>wfc.exe (工作流命令行编译器工具) 

> [!NOTE]
> 本材料讨论的类型和命名空间已废弃不用。

wfc.exe 工作流命令行编译器工具适用于文件扩展名为 *xoml* () 过时的旧工作流标记文件。

## <a name="compilation-process"></a>编译过程

编译工作流时，会在编译过程中执行下列过程：

- 执行验证可以确保根据工作流活动为自己设置的规则验证这些活动。 如果存在验证错误，则编译器会返回错误列表。  
- 根据在编译器中输入的标记定义来生成一个分部类。  

- 执行代码可以帮助在运行时执行活动。 生成事件订阅，事件订阅可帮助活动了解它们所包含的活动何时执行完毕。  
- 在 .NET Framework C# 或 Visual Basic 编译器中输入根据标记文件生成的分部类以及根据代码文件生成的分部类。 此过程的输出为 .NET 程序集：WorkflowSample.dll。 可以部署该程序集以运行工作流。

### <a name="compiler-options"></a>编译器选项

本部分介绍 wfc.exe 工作流命令行编译器的选项。

```output
    Microsoft (R) Windows Workflow Compiler version 3.0.0.0
    Copyright (C) Microsoft Corporation 2005. All rights reserved.

                      Windows Workflow Compiler Options

    wfc.exe <Xoml file list> /target:assembly [<vb/cs file list>] [/language:...]
            [/out:...] [/reference:...] [/library:...] [/debug...] [/nocode...]
             [/checktypes...] [/resource:<resource info>]

                            - OUTPUT FILE -
    /out:<file>             Output file name
    /target:assembly        Build a Windows Workflow assembly (default).
                            Short form: /t:assembly
    /target:exe             Build a Windows Workflow application.
                            Short form: /t:exe
    /delaysign[+|-]         Delay-sign the assembly using only the public portion
                            of the strong name key.
    /keyfile:<file>         Specifies a strong name key file.
    /keycontainer:<string>  Specifies a strong name key container.

                            - INPUT FILES -
    <Xoml file list>        Xoml source file name(s).
    <vb/cs file list>       Code-beside file name(s).
    /reference:<file list>  Reference metadata from the specified assembly file(s).
                            Short form is '/r:'.
    /library:<path list>    Set of directories where to lookup for the references.
                            Short form is '/lib:'.
    /resource:<resinfo>     Embed the specified resource. Short form is '/res:'.
                            resinfo format is <file>[,<name>[,public|private]].

    Rules and freeform layout files must be embedded as assembly resources.
    The resource name is constructed by using the namespace and type name
    of the activity. For example, an activity named "MyActivity" in namespace
    "WFProject" would require resource names "WFProject.MyActivity.rules"
    and/or "WFProject.MyActivity.layout".

                            - CODE GENERATION -
    /debug[+|-]             Emit full debugging information. The default is '+'.
    /nocode[+|-]            Disallow code-beside model.
                            The default is '-'. Short form is '/nc:'.
    /checktypes[+|-]        Check for permitted types in wfc.exe.config file.
                            The default is '-'. Short form is '/ct:'.

                            - LANGUAGE -
    /language:[cs|vb]       The language to use for the generated class.
                            The default is 'CS' (C#). Short form is '/l:'.
    /rootnamespace:<string> Specifies the root Namespace for all type declarations.
                            Valid only for 'VB' (Visual Basic) language.
                            Short form is '/rns:'.

                            - MISCELLANEOUS -
    /help                   Display this usage message. Short form is '/?'.
    /nologo                 Suppress compiler copyright message. Short form is '/n'.

    /nowarn                 Ignore compiler warnings. Short form is '/w'.
```

## <a name="remarks"></a>备注

> [!NOTE]
> 本材料讨论的类型和命名空间已废弃不用。

授权类型的列表通常在 *wfc.exe.config* 文件中定义。 在工作流编译的验证阶段，如果工作流源文档或伴随规则文件直接引用授权类型列表中不存在的任何 .NET Framework 类型，则该工作流源文档将被拒绝。 授权类型列表是一个 XML 文档，其中每个条目表示一个 `Assembly` 、、 `Namespace` 一个 `TypeName` 和一个授权的 { `True`&#124;`False` } 指示器。 `AuthorizedType` 对应于列表中的条目。 允许使用通配符符号（可用于包含或排除完整的命名空间）。 例如， `Type="System.*"` 包括中的所有类型 <xref:System> ，包括子命名空间中包含的类型。
  
使用授权类型列表的方式由 <xref:System.Workflow.ComponentModel.Compiler.WorkflowCompiler> 选项控制 `'/checktypes'` 。

```xml  
<configuration>  
  <System.Workflow.ComponentModel.WorkflowCompiler>
    <authorizedTypes>
      <targetFx version="v4.0">
        ...
        <authorizedType Assembly="System, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" Namespace="System*" TypeName="*" Authorized="True"/>
        ...
      </targetFx>
    </authorizedTypes>
  </System.Workflow.ComponentModel.WorkflowCompiler>  
</configuration>  
```

> [!WARNING]
> 当 `Type="System.*"` 类型存在时，可以包括其他不需要的类型（如 `Type="System.Configuration"` ）进行编译。 你应谨慎，并查看每个。 对于应限制的任何类型，请确保将设置 `Authorized` 为 `False` 。

## <a name="see-also"></a>另请参阅

- [AuthorizedType 类](xref:System.Workflow.ComponentModel.Compiler.AuthorizedType)
