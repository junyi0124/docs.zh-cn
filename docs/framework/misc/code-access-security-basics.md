---
title: 代码访问安全性基础知识
description: 了解面向 CLR 的应用程序的代码访问安全性基础知识：类型安全代码、命令性语法、声明性语法、安全类库和透明代码。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [.NET Framework], code access security
ms.assetid: 4eaa6535-d9fe-41a1-91d8-b437cfc16921
ms.openlocfilehash: 9f6049913a2e8cf3e3f220b0148598a236b60bef
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90557066"
---
# <a name="code-access-security-basics"></a>代码访问安全性基础知识

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]

每个面向公共语言运行时的应用程序（即每个托管应用程序）必须与运行时的安全系统进行交互。 加载托管应用程序时，其主机会自动授予它一组权限。 这些权限由主机的本地安全设置或应用程序所在的沙盒决定。 根据这些权限，应用程序会正确运行，或者生成安全异常。

桌面应用程序的默认主机允许代码在完全信任环境中运行。 因此，如果应用程序面向桌面，它就具有不受限制的权限集。 其他主机或沙盒会为应用程序提供受限制的权限集。 由于主机的权限集各不相同，所以必须将应用程序设计为仅使用目标主机允许的权限。

你必须熟悉下列代码访问安全性概念才能编写面向公共语言运行时的有效应用程序：

- **类型安全代码**：类型安全代码是仅以定义完善的允许方法访问类型的代码。 例如，给定有效的对象引用，类型安全代码能以与实际字段成员相应的固定偏移量访问内存。 如果代码以超出内存（内存属于该对象的公共公开字段）范围的任意偏移量访问内存，则它不是类型安全的。 若要使代码能够受益于代码访问安全性，必须使用生成可验证类型安全代码的编译器。 有关详细信息，请参阅本主题后面的 [编写验证的类型安全代码](#typesafe_code) 部分。

- **命令性和声明性语法**：以公共语言运行时为目标的代码可以通过以下方法与安全系统进行交互：请求权限、要求调用方具有指定的权限，并覆盖 (给定足够权限) 的某些安全设置。 可使用两种形式的语法以编程方式与 .NET Framework 安全系统进行交互：声明性语法和命令性语法。 声明性调用通过使用属性来执行；命令性调用通过使用代码中类的新实例来执行。 一些调用只能以命令方式执行，其他调用仅能以声明方式执行，还有一些调用能以任一种方式执行。

- **安全类库**：安全类库使用安全要求确保库的调用方有权访问库公开的资源。 例如，安全类库可能具有一种方法，可用于创建一种文件，这种文件要求其调用方具有创建文件的权限。 .NET Framework 由安全类库组成。 你应了解访问你的代码所使用的任何库所需的权限。 有关详细信息，请参阅本主题后面的 [使用安全类库](#secure_library) 部分。

- **透明代码**：除了标识特定权限以外，还必须确定代码是否应作为安全透明运行，从 .NET Framework 4 开始。 安全透明代码不能调用标识为安全关键的类型或成员。 此规则适用于完全信任的应用程序以及部分受信任的应用程序。 有关详细信息，请参阅 [安全透明代码](security-transparent-code.md)。

<a name="typesafe_code"></a>

## <a name="writing-verifiably-type-safe-code"></a>编写可验证类型安全代码

实时 (JIT) 编译会执行一个验证过程以检查代码并尝试确定代码是否为类型安全的。 在验证过程中验证为类型安全的代码称为可 *验证的类型安全代码*。 代码可以是类型安全的，但由于验证过程或编译器的限制，代码可能不是可验证类型安全的。 并非所有语言都是类型安全的，而且一些语言编译器（如 Microsoft Visual c + +）无法生成可验证类型安全托管代码。 若要确定你使用的语言编译器是否生成可验证类型安全代码，请参阅编译器的文档。 如果你使用的语言编译器仅在你避免使用某些语言构造时才生成可验证的类型安全代码，则你可能需要使用 [peverify.exe 工具](../tools/peverify-exe-peverify-tool.md) 来确定你的代码是否可验证类型安全。

如果安全策略允许代码跳过验证，不是可验证类型安全的代码也可以尝试执行。 但是，由于类型安全是运行时机制隔离程序集必不可少的组成部分，如果代码违反类型安全的规则，就不能可靠地强制实施安全性。 默认情况下，仅当非类型安全的代码来自本地计算机时才允许它运行。 因此，移动代码应是类型安全的。

<a name="secure_library"></a>

## <a name="using-secure-class-libraries"></a>使用安全类库

如果你的代码进行请求并被授予类库所需的权限，将允许它访问该库，同时保护该库公开的资源不受未经授权的访问。 如果你的代码不具有适当的权限，将不允许它访问类库，恶意代码将无法使用你的代码间接访问受保护的资源。 其他调用你代码的代码也必须具有访问该库的权限。 如果它不具有权限，则你的代码也将被限制运行。

代码访问安全性不会消除编写代码时产生人为错误的可能性。 但是，如果你的应用程序使用安全类库访问受保护的资源，应用程序代码的安全风险会降低，因为类库会受到仔细审查以排除潜在的安全问题。

## <a name="declarative-security"></a>声明性安全

声明性安全语法使用 [特性](../../standard/attributes/index.md) 将安全信息放置到代码的 [元数据](../../standard/metadata-and-self-describing-components.md) 中。 属性可以程序集、类或成员级别放置，以指示你要使用的请求、需求或重写的类型。 请求用于面向公共语言运行时的应用程序，以通知运行时安全系统有关应用程序需要或不想要的权限。 在库中使用要求和重写以帮助保护调用方的资源或重写默认安全行为。

> [!NOTE]
> 在 .NET Framework 4 中，对 .NET Framework 安全模型和术语进行了重大更改。 有关这些更改的详细信息，请参阅 [安全更改](/previous-versions/dotnet/framework/security/security-changes)。

为了使用声明性安全调用，必须初始化权限对象的状态数据，使其表示所需权限的特定形式。 每个内置权限都具有一个属性，会向该属性传递 <xref:System.Security.Permissions.SecurityAction> 枚举来描述你要执行的安全操作的类型。 但是，权限还接受自己独占的自己的参数。

下列代码段显示一个声明性语法，用于要求代码调用方具有称为 `MyPermission` 的自定义权限。 此权限是假设的自定义权限，在 .NET Framework 中并不存在。 在此示例中，声明性调用直接放置在类定义之前，指定此权限应用于类级别。 向属性传递 **SecurityAction** 结构，以指定调用方必须拥有此权限才能运行。

```vb
<MyPermission(SecurityAction.Demand, Unrestricted = True)> Public Class MyClass1

   Public Sub New()
      'The constructor is protected by the security call.
   End Sub

   Public Sub MyMethod()
      'This method is protected by the security call.
   End Sub

   Public Sub YourMethod()
      'This method is protected by the security call.
   End Sub
End Class
```

```csharp
[MyPermission(SecurityAction.Demand, Unrestricted = true)]
public class MyClass
{
   public MyClass()
   {
      //The constructor is protected by the security call.
   }

   public void MyMethod()
   {
      //This method is protected by the security call.
   }

   public void YourMethod()
   {
      //This method is protected by the security call.
   }
}
```

## <a name="imperative-security"></a>强制安全性

强制性安全语法通过创建你要调用的权限对象的新实例发出安全调用。 强制性语法可用于执行需求和重写，但不可用于执行请求。

进行安全调用之前，必须初始化权限对象的状态数据，使其表示所需权限的特定形式。 例如，在创建对象时 <xref:System.Security.Permissions.FileIOPermission> ，可以使用构造函数来初始化 **FileIOPermission** 对象，使其表示对所有文件的不受限制的访问权限，或者不能访问文件。 或者，你可以使用不同的 **FileIOPermission** 对象，传递指定你希望对象表示的访问类型的参数 (即读取、追加或写入) 以及希望对象保护的文件。

除了使用命令性安全语法来调用单个安全对象，你还可以使用它来初始化权限集中的一组权限。 例如，这种方法是在一个方法中对多个权限可靠地执行 [assert](using-the-assert-method.md) 调用的唯一方法。 使用 <xref:System.Security.PermissionSet> 和 <xref:System.Security.NamedPermissionSet> 类来创建一组权限，然后调用合适的方法来调用所需的安全调用。

强制性语法可用于执行需求和重写，但不可用于执行请求。 当初始化权限状态所需的信息仅在运行时变成已知时，你可能会为需求和重写使用命令性语法而不是声明性语法。 例如，如果想确保调用方具有读取某个文件的权限，但直到运行时都不知道该文件的名称，在这种情况下，则使用命令性需求。 当你需要在运行时确定是否存在一个条件以及根据测试结果是否发出安全要求时，也可以选择使用命令性检查而不是声明性检查。

下列代码段显示了声明性语法，用于要求代码调用方具有名为 `MyPermission` 的自定义权限。 此权限是假设的自定义权限，在 .NET Framework 中并不存在。 在 `MyMethod` 中创建了 `MyPermission` 的新实例，用于通过安全调用仅对此方法提供保护。

```vb
Public Class MyClass1

   Public Sub New()

   End Sub

   Public Sub MyMethod()
      'MyPermission is demanded using imperative syntax.
      Dim Perm As New MyPermission()
      Perm.Demand()
      'This method is protected by the security call.
   End Sub

   Public Sub YourMethod()
      'YourMethod 'This method is not protected by the security call.
   End Sub
End Class
```

```csharp
public class MyClass {
   public MyClass(){

   }

   public void MyMethod() {
       //MyPermission is demanded using imperative syntax.
       MyPermission Perm = new MyPermission();
       Perm.Demand();
       //This method is protected by the security call.
   }

   public void YourMethod() {
       //This method is not protected by the security call.
   }
}
```

## <a name="using-managed-wrapper-classes"></a>使用托管包装类

大多数应用程序和组件（安全库除外） 不应直接调用非托管代码。 其原因有若干： 如果代码直接调用非托管代码，在许多情况下将不允许该代码运行，因为必须授予代码调用本机代码所需的高级别信任。 如果为允许此类应用程序运行而修改策略，则会大大削弱系统安全性，这样的话，应用程序能自由执行几乎所有操作。

此外，有权访问非托管代码的代码很可能会通过调用非托管 API 来执行几乎任何操作。 例如，有权调用非托管代码的代码不需要 <xref:System.Security.Permissions.FileIOPermission> 访问文件; 它可以直接调用非托管 (Win32) 文件 api，绕过需要 **FileIOPermission**的托管文件 api。 如果托管代码有权调入非托管代码并直接调入非托管代码，安全系统将无法可靠地强制执行安全限制，因为运行时无法对非托管代码强制执行这类限制。

如果希望应用程序执行需要访问非托管代码的操作，则应该通过包装所需功能的受信任的托管类（如果存在这样的类）来执行此操作。 如果安全类库中已存在包装类，请不要自己创建此类。 必须向此包装类授予高级别的信任度，该包装类才能调入非托管代码，该包装类负责要求调入方具有合适权限。 如果使用包装类，你的代码只需请求并被授予包装类要求的权限。

## <a name="see-also"></a>请参阅

- <xref:System.Security.PermissionSet>
- <xref:System.Security.Permissions.FileIOPermission>
- <xref:System.Security.NamedPermissionSet>
- <xref:System.Security.Permissions.SecurityAction>
- [ActualRebinds](using-the-assert-method.md)
- [代码访问安全性](code-access-security.md)
- [代码访问安全性基础知识](code-access-security-basics.md)
- [特性](../../standard/attributes/index.md)
- [元数据和自描述组件](../../standard/metadata-and-self-describing-components.md)
