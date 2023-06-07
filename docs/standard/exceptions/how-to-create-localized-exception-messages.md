---
title: 如何使用本地化的异常消息创建用户定义的异常
description: 了解如何使用本地化的异常消息创建用户定义的异常
author: Youssef1313
dev_langs:
- csharp
- vb
ms.date: 09/13/2019
ms.openlocfilehash: fa0bbfdbfc9408302eec2edd4e4ee4503d2612c7
ms.sourcegitcommit: 7980a91f90ae5eca859db7e6bfa03e23e76a1a50
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "81243344"
---
# <a name="how-to-create-user-defined-exceptions-with-localized-exception-messages"></a>如何使用本地化的异常消息创建用户定义的异常

在本文中，你将了解如何通过使用附属程序集的本地化异常消息创建从 <xref:System.Exception> 基类继承的用户定义异常。

## <a name="create-custom-exceptions"></a>创建自定义异常

.NET 包含许多你可以使用的不同异常。 但是，在某些情况下，如果它们都无法满足你的需要，则可以创建自己的自定义异常。

假设要创建一个 `StudentNotFoundException`，其中包含 `StudentName` 属性。
若要创建自定义异常，请执行以下步骤：

1. 创建一个从 <xref:System.Exception> 继承的可序列化类。 类名称应以“Exception”结尾：

    ```csharp
    [Serializable]
    public class StudentNotFoundException : Exception { }
    ```

    ```vb
    <Serializable>
    Public Class StudentNotFoundException
        Inherits Exception
    End Class
    ```

1. 添加默认构造函数：

    ```csharp
    [Serializable]
    public class StudentNotFoundException : Exception
    {
        public StudentNotFoundException() { }

        public StudentNotFoundException(string message)
            : base(message) { }

        public StudentNotFoundException(string message, Exception inner)
            : base(message, inner) { }
    }
    ```

    ```vb
    <Serializable>
    Public Class StudentNotFoundException
        Inherits Exception

        Public Sub New()
        End Sub

        Public Sub New(message As String)
            MyBase.New(message)
        End Sub

        Public Sub New(message As String, inner As Exception)
            MyBase.New(message, inner)
        End Sub
    End Class
    ```

1. 定义任何其他属性和构造函数：

    ```csharp
    [Serializable]
    public class StudentNotFoundException : Exception
    {
        public string StudentName { get; }

        public StudentNotFoundException() { }

        public StudentNotFoundException(string message)
            : base(message) { }

        public StudentNotFoundException(string message, Exception inner)
            : base(message, inner) { }

        public StudentNotFoundException(string message, string studentName)
            : this(message)
        {
            StudentName = studentName;
        }
    }
    ```

    ```vb
    <Serializable>
    Public Class StudentNotFoundException
        Inherits Exception

        Public ReadOnly Property StudentName As String

        Public Sub New()
        End Sub

        Public Sub New(message As String)
            MyBase.New(message)
        End Sub

        Public Sub New(message As String, inner As Exception)
            MyBase.New(message, inner)
        End Sub

        Public Sub New(message As String, studentName As String)
            Me.New(message)
            StudentName = studentName
        End Sub
    End Class
    ```

## <a name="create-localized-exception-messages"></a>创建本地化异常消息

你已创建一个自定义异常，可以使用如下所示的代码在任何位置将其抛出：

```csharp
throw new StudentNotFoundException("The student cannot be found.", "John");
```

```vb
Throw New StudentNotFoundException("The student cannot be found.", "John")
```

上一行的问题是，`"The student cannot be found."` 只是一个常量字符串。 在本地化应用程序中，你需要根据用户区域性使用不同的消息。
[附属程序集](../../framework/resources/creating-satellite-assemblies-for-desktop-apps.md)是执行此操作的好方法。 附属程序集是一个 .dll，其中包含特定语言的资源。 当你在运行时请求特定资源时，CLR 将根据用户区域性查找该资源。 如果找不到该区域性对应的附属程序集，则使用默认区域性的资源。

创建本地化异常消息：

1. 创建一个名为“Resources”的新文件夹来保存资源文件。
1. 向其中添加新的资源文件。 若要在 Visual Studio 中执行此操作，请在“解决方案资源管理器”中右键单击该文件夹，然后选择“添加” > “新项” > “资源文件”。 将该文件命名为“ExceptionMessages.resx”。 这是默认的资源文件。
1. 为异常消息添加名称/值对，如下图所示：

   ![将资源添加到默认区域性](media/add-resources-to-default-culture.jpg)

1. 为法语添加新的资源文件。 将其命名为“ExceptionMessages.fr-FR.resx”。
1. 再次为异常消息添加名称/值对，但使用法语值：

   ![将资源添加到 fr-FR 区域性](media/add-resources-to-fr-culture.jpg)

1. 生成项目后，生成的输出文件夹应包含 fr-FR 文件夹，其中具有 .dll 文件，它是附属程序集。
1. 使用如下所示代码抛出异常：

    ```csharp
    var resourceManager = new ResourceManager("FULLY_QUALIFIED_NAME_OF_RESOURCE_FILE", Assembly.GetExecutingAssembly());
    throw new StudentNotFoundException(resourceManager.GetString("StudentNotFound"), "John");
    ```

    ```vb
    Dim resourceManager As New ResourceManager("FULLY_QUALIFIED_NAME_OF_RESOURCE_FILE", Assembly.GetExecutingAssembly())
    Throw New StudentNotFoundException(resourceManager.GetString("StudentNotFound"), "John")
    ```

    > [!NOTE]
    > 如果项目名称为 `TestProject`，并且资源文件 ExceptionMessages.resx 位于项目的 Resources 文件夹中，则资源文件的完全限定名称为 `TestProject.Resources.ExceptionMessages`。

## <a name="see-also"></a>请参阅

- [如何创建用户定义的异常](how-to-create-user-defined-exceptions.md)
- [创建桌面应用程序的附属程序集](../../framework/resources/creating-satellite-assemblies-for-desktop-apps.md)
- [base（C# 参考）](../../csharp/language-reference/keywords/base.md)
- [this（C# 参考）](../../csharp/language-reference/keywords/this.md)
