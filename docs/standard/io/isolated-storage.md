---
title: 独立存储
description: 了解独立存储（一种数据存储机制），它在代码与保存的数据之间定义了标准化的关联方式，从而提供隔离性和安全性。
ms.date: 03/30/2017
helpviewer_keywords:
- data storage using isolated storage
- stores
- storing data using isolated storage
- isolated storage
- location of isolated storage in file system
- standardizing storage systems
- storing data using isolated storage, when not to use
- code, isolated storage
- isolated storage, options
- data storage using isolated storage, when not to use
- storing data using isolated storage, options
- isolated storage, when not to use
- data storage using isolated storage, options
- isolation
ms.assetid: aff939d7-9e49-46f2-a8cd-938d3020e94e
ms.openlocfilehash: 1b123b231a7b6856ca527d00b5927c1a8d0e08b6
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94819131"
---
# <a name="isolated-storage"></a>独立存储

对于桌面应用，独立存储是一种数据存储机制，它定义了将代码与保存的数据关联的标准化方式，从而提供隔离性和安全性。 同时，标准化也提供了其他好处。 管理员可以使用旨在操作独立存储的工具来配置文件存储空间、设置安全策略及删除未使用的数据。 通过独立存储，代码不再需要使用唯一的路径来指定文件系统中的安全位置，同时可以保护数据免遭只具有独立存储访问权限的其他应用程序的损坏。 不再需要指示应用程序的存储区域位置的硬编码信息。

> [!IMPORTANT]
> 独立存储不适用于 Windows 8.x 应用商店应用。 请改用 Windows 运行时 API 包含的 `Windows.Storage` 命名空间中的应用程序数据类来存储本地数据和文件。 有关详细信息，请参阅 Windows 开发人员中心的 [应用程序数据](/previous-versions/windows/apps/hh464917(v=win.10)) 。

<a name="data_compartments_and_stores"></a>

## <a name="data-compartments-and-stores"></a>数据隔离舱和存储区

当应用程序在文件中存储数据时，必须仔细选择文件名和存储位置，最大程度地减小其他应用程序知道该存储位置的可能性，从而使数据不易受到损坏。 如果没有标准系统来管理这些问题，想临时开发最大程度地减少存储冲突的技术可能并非易事，而且开发出来的技术也不见得可靠。

通过使用独立存储，数据将始终按用户和程序集进行隔离。 程序集的源或强名称等凭据确定程序集的身份。 通过使用类似的凭据，数据还可以按应用程序域进行隔离。

当使用独立存储时，应用程序将数据保存到与代码标识的某些方面（例如，其发行者或签名）关联的独特数据隔离舱。 数据隔离舱是一个抽象的存储位置，而不是具体的存储位置，它由一个或多个独立的存储文件（叫做存储区）组成，这些独立的存储文件包含存储数据的实际目录位置。 例如，应用程序可能有一个与其关联的数据隔离舱，文件系统中的某个目录将实现实际保留应用程序数据的存储区。 保存在存储区中的数据可以是任意类型的数据，无论是用户首选项信息还是应用程序状态都可以。 对于开发人员来说，数据隔离舱的位置是透明的。 应用商店通常位于客户端，但是，服务器应用程序可以使用独立存储通过模拟该服务的用户存储信息。 独立存储还可以将信息和用户漫游配置文件一起存储在服务器上，这样，漫游用户就可以随时使用该信息。

<a name="quotas"></a>

## <a name="quotas-for-isolated-storage"></a>独立存储的配额

配额是对可使用的独立存储数量的限制。 配额包括文件空间的字节及与存储区中目录和其他信息关联的系统开销。 独立存储使用权限配额，这些配额是使用 <xref:System.Security.Permissions.IsolatedStoragePermission> 对象设置的存储限制。 如果尝试写入的数据超出配额，则会引发 <xref:System.IO.IsolatedStorage.IsolatedStorageException> 异常。  安全策略确定向代码授予的权限，它可以使用 .NET Framework 配置工具 (Mscorcfg.msc) 来修改。 已授予 <xref:System.Security.Permissions.IsolatedStoragePermission> 的代码所使用的存储范围不能超过 <xref:System.Security.Permissions.IsolatedStoragePermission.UserQuota%2A> 属性的限制。 但是，由于代码可以通过表示不同的用户标识绕过权限配额，所以权限配额用作指导代码如何工作的指南，而不是对代码行为的硬性限制。

不对漫游存储区强制执行配额。 因此，对使用它们的代码要求稍高级别的权限。 枚举值 <xref:System.Security.Permissions.IsolatedStorageContainment.AssemblyIsolationByRoamingUser> 和 <xref:System.Security.Permissions.IsolatedStorageContainment.DomainIsolationByRoamingUser> 为漫游用户指定使用独立存储的权限。

<a name="secure_access"></a>

## <a name="secure-access"></a>安全访问

通过使用独立存储，可以使部分受信任的应用程序以由计算机安全策略控制的方式存储数据。 对于用户需慎重运行的下载的组件来说，这尤为有用。 在使用标准 I/O 机制访问文件系统时，安全策略很少向这种代码授予权限。 但是默认情况下，会对在本地计算机、本地网络或 Internet 中运行的代码授予使用独立存储的权限。

管理员可以根据适当的信任级别限制应用程序或用户可以使用多少独立存储。 另外，管理员可以完全移除用户的持久性数据。 若要创建或访问独立存储，则必须授予代码相应的 <xref:System.Security.Permissions.IsolatedStorageFilePermission> 权限。

要访问独立存储，代码必须具有所有必要的本机平台操作系统权限。 必须满足用来控制哪些用户有权使用文件系统的访问控制列表 (ACL)。 除非执行（特定于平台的）模拟，否则 .NET 应用程序已经具有访问独立存储的操作系统权限。 在这种情况下，应用程序负责确保被模拟的用户标识具有访问独立存储的适当操作系统权限。 对于在 Web 上运行或从 Web 下载的代码而言，这种访问为之提供了一种对与特定用户相关的存储区域进行读写操作的简便方法。

为了控制对独立存储的访问，公共语言运行时使用 <xref:System.Security.Permissions.IsolatedStorageFilePermission> 对象。 每个对象都具有指定以下值的属性：

- 允许的用法，这指出了所允许的访问类型。 这些值是 <xref:System.Security.Permissions.IsolatedStorageContainment> 枚举的成员。 有关这些值的更多信息，请参见下一节中的表。

- 存储配额（如上一节所述）。

当代码第一次尝试打开存储时，运行时要求 <xref:System.Security.Permissions.IsolatedStorageFilePermission> 权限。 它根据代码的受信任程度决定是否授予此权限。 如果授予此权限，则允许的用法和存储配额值由安全策略和代码对 <xref:System.Security.Permissions.IsolatedStorageFilePermission>的请求决定。 安全策略使用 .NET Framework 配置工具 (Mscorcfg.msc) 来进行设置。 检查调用堆栈中的所有调用方以确保每个调用方至少具有适当的允许的用法。 运行时还检查强加于代码的配额，该代码打开或创建将在其中保存文件的存储区。 如果满足这些条件，就授予权限。 每次文件写入存储区时，都将再次检查配额。

因为公共语言运行时将根据安全策略授予任何适当的 <xref:System.Security.Permissions.IsolatedStorageFilePermission> ，所以请求权限不需要应用程序代码。 然而，有很好的理由来请求应用程序需要的特定权限，包括 <xref:System.Security.Permissions.IsolatedStorageFilePermission>。

<a name="allowed_usage"></a>

## <a name="allowed-usage-and-security-risks"></a>允许的用法和安全风险

<xref:System.Security.Permissions.IsolatedStorageFilePermission> 指定的允许的用法确定允许代码创建和使用独立存储的程度。 下表显示了权限中指定的允许的用法如何与隔离的类型对应，并总结了与每种允许的用法关联的安全风险。

|允许的用法|隔离类型|安全影响|
|-------------------|---------------------|---------------------|
|<xref:System.Security.Permissions.IsolatedStorageContainment.None>|不允许使用任何独立存储。|没有安全影响。|
|<xref:System.Security.Permissions.IsolatedStorageContainment.DomainIsolationByUser>|按用户、域和程序集隔离。 每个程序集在域中都有单独的子存储区。 使用此权限的存储也由计算机隐式隔离。|此权限级别无法阻止他人未经授权滥用资源，尽管强制的配额对此做法增添了一些难度。 这叫做拒绝服务攻击。|
|<xref:System.Security.Permissions.IsolatedStorageContainment.DomainIsolationByRoamingUser>|与 `DomainIsolationByUser`相同，但如果启用漫游用户配置文件且不强制配额，则存储将保存到将漫游的位置。|因为必须禁用配额，所以存储资源更易受到拒绝服务攻击。|
|<xref:System.Security.Permissions.IsolatedStorageContainment.AssemblyIsolationByUser>|按用户和程序集隔离。 使用此权限的存储也由计算机隐式隔离。|在此级别强制实施配额以帮助防止拒绝服务攻击。 由于另一个域中相同的程序集可以访问该存储区，这就使信息可能在应用程序间泄露。|
|<xref:System.Security.Permissions.IsolatedStorageContainment.AssemblyIsolationByRoamingUser>|与 `AssemblyIsolationByUser`相同，但如果启用漫游用户配置文件且不强制配额，则存储将保存到将漫游的位置。|与 `AssemblyIsolationByUser`中相同，但没有配额，增加了拒绝服务攻击的风险。|
|<xref:System.Security.Permissions.IsolatedStorageContainment.AdministerIsolatedStorageByUser>|按用户隔离。 通常，只有管理或调试工具才使用此级别的权限。|使用该权限访问允许代码查看或删除任何的用户独立存储文件或目录（而不论程序集是否隔离）。 存在的风险包括（但不限于）泄露信息和数据丢失。|
|<xref:System.Security.Permissions.IsolatedStorageContainment.UnrestrictedIsolatedStorage>|按所有用户、域和程序集隔离。 通常，只有管理或调试工具才使用此级别的权限。|此权限有可能会整个危害所有用户的所有独立存储区。|

## <a name="safety-of-isolated-storage-components-with-regard-to-untrusted-data"></a>与不受信任的数据相关的独立存储组件的安全性

__本节适用于以下框架：__

- .NET Framework（所有版本）
- .NET Core 2.1+
- .NET 5.0+

.NET Framework 和 .NET Core 提供独立存储作为一种为用户、应用程序或组件保留数据的机制。 这是一个旧组件，主要用于现已弃用的代码访问安全性方案。

各种独立存储 API 和工具可用于跨信任边界读取数据。 例如，从计算机范围的作用域中读取数据会从计算机上其他可能不太受信任的用户帐户聚合数据。 从计算机范围的独立存储作用域读取的组件或应用程序应了解读取此数据的后果。

### <a name="security-sensitive-apis-that-can-read-from-the-machine-wide-scope"></a>可从计算机范围作用域读取的安全敏感 API

调用以下任意 API 的组件或应用程序从计算机范围的作用域中读取：

* [IsolatedStorageFile.GetEnumerator](/dotnet/api/system.io.isolatedstorage.isolatedstoragefile.getenumerator)，传递包含 IsolatedStorageScope.Machine 标志的作用域
* [IsolatedStorageFile.GetMachineStoreForApplication](/dotnet/api/system.io.isolatedstorage.isolatedstoragefile.getmachinestoreforapplication)
* [IsolatedStorageFile.GetMachineStoreForAssembly](/dotnet/api/system.io.isolatedstorage.isolatedstoragefile.getmachinestoreforassembly)
* [IsolatedStorageFile.GetMachineStoreForDomain](/dotnet/api/system.io.isolatedstorage.isolatedstoragefile.getmachinestorefordomain)
* [IsolatedStorageFile.GetStore](/dotnet/api/system.io.isolatedstorage.isolatedstoragefile.getstore)，传递包含 IsolatedStorageScope.Machine 标志的作用域
* [IsolatedStorageFile.Remove](/dotnet/api/system.io.isolatedstorage.isolatedstoragefile.remove)，传递包含 `IsolatedStorageScope.Machine` 标志的作用域

如果通过 `/machine` 开关调用，则会影响[独立存储工具](../../framework/tools/storeadm-exe-isolated-storage-tool.md) `storeadm.exe`，如以下代码所示：

```txt
storeadm.exe /machine [any-other-switches]
```

Visual Studio 和 .NET Framework SDK 包含独立存储工具。

如果应用程序不涉及调用上述 API，或者工作流不涉及以这种方式调用 `storeadm.exe`，则不会应用此文档。

### <a name="impact-in-multi-user-environments"></a>多用户环境中的影响

如前所述，从一个信任环境写入的数据产生的这些 API 安全影响可从不同的信任环境中读取。 独立存储通常使用三个位置中的一个来读取和写入数据：

1. `%LOCALAPPDATA%\IsolatedStorage\`：例如，`User` 范围的 `C:\Users\<username>\AppData\Local\IsolatedStorage\`。
2. `%APPDATA%\IsolatedStorage\`：例如，`User|Roaming` 范围的 `C:\Users\<username>\AppData\Roaming\IsolatedStorage\`。
3. `%PROGRAMDATA%\IsolatedStorage\`：例如，`Machine` 范围的 `C:\ProgramData\IsolatedStorage\`。

每个用户的前两个位置都是独立的。 Windows 可确保同一计算机上的不同用户帐户无法访问彼此的用户配置文件文件夹。 使用 `User` 或 `User|Roaming` 存储区的两个不同用户帐户不会看到彼此的数据，并且不会干扰彼此的数据。

第三个位置在计算机上的所有用户帐户之间共享。 不同的帐户可以从此位置读取数据以及将数据写入此位置，并且可以查看彼此的数据。

上述路径可能因使用的 Windows 版本而异。

现在，假设有一个多用户系统，其中有两个注册用户 Mallory 和 Bob 。 Mallory 能够访问用户配置文件目录 `C:\Users\Mallory\`，并且可以访问共享计算机范围的存储位置 `C:\ProgramData\IsolatedStorage\`。 她无法访问 Bob 的用户配置文件目录 `C:\Users\Bob\`。

如果 Mallory 要攻击 Bob，她可以将数据写入到计算机范围的存储位置，然后尝试影响 Bob 从计算机范围的存储中读取数据。 当 Bob 运行从该存储区读取的应用时，该应用将对 Mallory 放置在此处的数据进行操作，但从 Bob 的用户帐户的上下文中运行。 本文档的其余部分介绍了各种攻击途径和应用可执行的步骤，以最大程度地降低这些攻击的风险。

> [!NOTE]
> 为了进行这种攻击，Mallory 需要：
>
> * 计算机上的用户帐户。
> * 能够将文件放在文件系统上的已知位置。
> * 了解 Bob 将在某个时间点运行尝试读取此数据的应用。
>
> 这些不是适用于标准单用户桌面环境（例如家庭电脑或单员工企业工作站）的威胁途径。

#### <a name="elevation-of-privilege"></a>特权提升

当 Bob 的应用读取 Mallory 的文件并根据该负载的内容自动尝试执行某些操作时，会出现特权提升攻击。 假设某个应用从计算机范围的存储读取启动脚本的内容，并将这些内容传递到 `Process.Start`。 如果 Mallory 可以在计算机范围的存储中放置恶意脚本，则在 Bob 启动其应用时：

* Bob 的应用会在其用户配置文件的上下文中分析和启动 Mallory 的恶意脚本。
* Mallory 会在本地计算机上获取对 Bob 帐户的访问权限。

#### <a name="denial-of-service"></a>拒绝服务

当 Bob 的应用读取 Mallory 的文件后崩溃或正常停止运行时，就会出现拒绝服务攻击。 再次假设前面提到的应用，它尝试从计算机范围的存储分析启动脚本。 如果 Mallory 可以将格式不正确的文件放在计算机范围的存储中，那么她可以：

* 使 Bob 的应用在启动路径初期引发异常。
* 出于异常原因，阻止应用成功启动。

然后，她拒绝 Bob 在他自己的用户帐户下启动该应用。

#### <a name="information-disclosure"></a>信息泄露

当 Mallory 可以诱使 Bob 泄露 Mallory 通常不能访问的文件内容时，会出现信息泄露。 假设 Bob 有一个机密文件 C:\Users\Bob\secret.txt，Mallory 想要读取该文件。 她知道该文件的路径，但无法读取，因为 Windows 禁止她获取对 Bob 用户配置文件目录的访问权限。

相反，Mallory 会将硬链接放置在计算机范围的存储中。 这是一种特殊类型的文件，它本身不包含任何内容，而是指向磁盘上的另一个文件。 尝试读取硬链接文件将改为读取链接所指向文件的内容。 创建硬链接后，Mallory 仍无法读取文件内容，因为她无权访问链接的目标 (`C:\Users\Bob\secret.txt`)。 不过，Bob 有权访问此文件。

当 Bob 的应用现在从计算机范围的存储中读取内容时，会无意中读取他的 `secret.txt` 文件的内容，就像文件本身已存在于计算机范围的存储中一样。 当 Bob 的应用退出时，如果该应用尝试将文件重新保存到计算机范围的存储中，则最终会在 *C:\ProgramData\IsolatedStorage\* 目录中放置该文件的实际副本。 由于此目录可由计算机上的任何用户读取，Mallory 现在可以读取该文件的内容。

### <a name="best-practices-to-defend-against-these-attacks"></a>防范这些攻击的最佳做法

__重要提示：__ 如果你的环境具有多个相互不受信任的用户，请不要调用 API `IsolatedStorageFile.GetEnumerator(IsolatedStorageScope.Machine)` 或调用工具 `storeadm.exe /machine /list`。 这两种情况都假设它们正在对受信任的数据进行操作。 如果攻击者可以在计算机范围的存储中放置恶意的有效负载，则该有效负载会在运行这些命令的用户的上下文中导致特权提升攻击。

如果在多用户环境中操作，请重新考虑使用针对计算机范围的独立存储功能。 如果应用必须从计算机范围的位置中读取数据，则最好从仅由管理员帐户写入的位置读取数据。 `%PROGRAMFILES%` 目录和 `HKLM` 注册表配置单元是仅由管理员写入并可由所有人读取的位置的示例。 因此从这些位置读取的数据为可信数据。

如果应用必须在多用户环境中使用计算机范围，请验证从计算机范围的存储中读取的任何文件内容。 如果应用反序列化这些文件中的对象图，请考虑使用更安全的序列化程序，如 `XmlSerializer`，而不是 `BinaryFormatter` 或 `NetDataContractSerializer` 等危险序列化程序。 使用深度嵌套的对象图或根据文件内容执行资源分配的对象图。

<a name="isolated_storage_locations"></a>

## <a name="isolated-storage-locations"></a>独立存储位置

有时候，使用操作系统的文件系统来验证对独立存储进行的更改会非常有帮助。 你可能还需要了解独立存储文件的位置。 该位置随操作系统的不同而不同。 下表显示了在几个常见操作系统上创建独立存储的根位置。 在此根位置下查找 Microsoft\IsolatedStorage 目录。 您必须更改文件夹设置以显示隐藏文件和文件夹，才能查看到文件系统中的独立存储。

|操作系统|在文件系统中的位置|
|----------------------|-----------------------------|
|Windows 2000、Windows XP、Windows Server 2003（从 Windows NT 4.0 升级）|支持漫游的存储区 =<br /><br /> \<SYSTEMROOT>\Profiles\\<user\>\Application Data<br /><br /> 非漫游存储区 =<br /><br /> \<SYSTEMROOT>\Profiles\\<user\>\Local Settings\Application Data|
|Windows 2000 - 全新安装（和从 Windows 98 及 Windows NT 3.51 升级）|支持漫游的存储区 =<br /><br /> \<SYSTEMDRIVE>\Documents and Settings\\<user\>\Application Data<br /><br /> 非漫游存储区 =<br /><br /> \<SYSTEMDRIVE>\Documents and Settings\\<user\>\Local Settings\Application Data|
|Windows XP、Windows Server 2003 - 全新安装（和从 Windows 2000 及 Windows 98 升级）|支持漫游的存储区 =<br /><br /> \<SYSTEMDRIVE>\Documents and Settings\\<user\>\Application Data<br /><br /> 非漫游存储区 =<br /><br /> \<SYSTEMDRIVE>\Documents and Settings\\<user\>\Local Settings\Application Data|
|Windows 8、Windows 7、Windows Server 2008、Windows Vista|支持漫游的存储区 =<br /><br /> \<SYSTEMDRIVE>\Users\\<user\>\AppData\Roaming<br /><br /> 非漫游存储区 =<br /><br /> \<SYSTEMDRIVE>\Users\\<user\>\AppData\Local|

<a name="isolated_storage_tasks"></a>

## <a name="creating-enumerating-and-deleting-isolated-storage"></a>创建、枚举和删除独立存储

.NET 在 <xref:System.IO.IsolatedStorage> 命名空间中提供了三个类来帮助你执行涉及独立存储的任务：

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>派生自 <xref:System.IO.IsolatedStorage.IsolatedStorage?displayProperty=nameWithType> ，它提供对存储的程序集和应用程序文件的基本管理。 <xref:System.IO.IsolatedStorage.IsolatedStorageFile> 类的实例表示位于文件系统中的单个存储区。

- <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream> 派生自 <xref:System.IO.FileStream?displayProperty=nameWithType> ，它提供对存储中的文件的访问。

- <xref:System.IO.IsolatedStorage.IsolatedStorageScope> 是一个枚举，使您可以创建并选择具有适当隔离类型的存储区。

独立存储类使您可以创建、枚举并删除独立存储。 通过 <xref:System.IO.IsolatedStorage.IsolatedStorageFile> 对象可以使用执行这些任务的方法。 某些操作要求你具有 <xref:System.Security.Permissions.IsolatedStorageFilePermission> 权限（表示管理独立存储的权限）；你可能还需要具有访问文件或目录的操作系统权限。

有关演示常见的独立存储任务的一系列示例，请参见 [相关主题](#related_topics)中列出的帮助主题。

<a name="scenarios_for_isolated_storage"></a>

## <a name="scenarios-for-isolated-storage"></a>独立存储的情况

在许多情况下，独立存储非常有用，包括这四种场景：

- 下载的控件。 不允许从 Internet 下载的托管代码控件通过正常的 I/O 类写入硬盘，但它们可以使用独立存储来持久保存用户设置和应用程序状态。

- 共享组件存储。 应用程序间共享的组件可以使用独立存储来提供对数据存储区的有控制的访问。

- 服务器存储。 服务器应用程序可以使用独立存储为请求应用程序的大量用户提供单独的存储区。 因为独立存储始终按用户进行隔离，所以服务器必须模拟发出请求的用户。 在这种情况下，根据主体的标识隔离数据，该标识与应用程序用来区分其用户的标识是同一个标识。

- 漫游。 应用程序还可以将独立存储和漫游用户配置文件一起使用。 这允许用户的独立存储区和配置文件一起漫游。

不要在以下情况下使用独立存储：

- 用来存储重要机密，例如不加密的密钥或密码，因为独立存储对高度受信任的代码、非托管代码或计算机的受信任用户不设防。

- 用来存储代码。

- 用来存储管理员控制的配置和部署设置。 （因为管理员不控制用户首选项，所以用户首选项不被认为是配置设置。）

许多应用程序都使用数据库来存储和隔离数据，在这种情况下，数据库中的一个或多个行可能代表某个特定用户的存储。 当用户数较少时、当使用数据库的系统开销非常大时或当不存在数据库功能时，您可以选择使用独立存储而不使用数据库。 另外，当应用程序要求比数据库的行所提供的存储更加灵活和复杂的存储时，独立存储也可以提供一个可行的替代方案。

<a name="related_topics"></a>

## <a name="related-articles"></a>相关文章

|Title|描述|
|-----------|-----------------|
|[隔离的类型](types-of-isolation.md)|描述不同类型的隔离。|
|[如何：获取独立存储的存储区](how-to-obtain-stores-for-isolated-storage.md)|提供使用 <xref:System.IO.IsolatedStorage.IsolatedStorageFile> 类获取按用户和程序集隔离的存储区的示例。|
|[如何：枚举独立存储的存储区](how-to-enumerate-stores-for-isolated-storage.md)|演示如何使用 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetEnumerator%2A?displayProperty=nameWithType> 方法计算用户的所有独立存储的大小。|
|[如何：删除独立存储中的存储区](how-to-delete-stores-in-isolated-storage.md)|演示如何使用 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.Remove%2A?displayProperty=nameWithType> 方法以两种不同方式删除独立存储区。|
|[如何：预见独立存储中的空间不足条件](how-to-anticipate-out-of-space-conditions-with-isolated-storage.md)|说明如何测量独立存储区中剩余的空间。|
|[如何：在独立存储中创建文件和目录](how-to-create-files-and-directories-in-isolated-storage.md)|提供一些在独立存储区中创建文件和目录的示例。|
|[如何：在独立存储中查找现有文件和目录](how-to-find-existing-files-and-directories-in-isolated-storage.md)|演示如何读取独立存储区中的目录结构和文件。|
|[如何：在独立存储中读取和写入文件](how-to-read-and-write-to-files-in-isolated-storage.md)|提供一个向独立存储文件写入字符串并将其读取回的示例。|
|[如何：在独立存储中删除文件和目录](how-to-delete-files-and-directories-in-isolated-storage.md)|演示如何删除独立存储文件和目录。|
|[文件和流 I/O](index.md)|解释如何执行同步和异步文件和数据流访问。|

<a name="reference"></a>

## <a name="reference"></a>参考

- <xref:System.IO.IsolatedStorage.IsolatedStorage?displayProperty=nameWithType>

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile?displayProperty=nameWithType>

- <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream?displayProperty=nameWithType>

- <xref:System.IO.IsolatedStorage.IsolatedStorageScope?displayProperty=nameWithType>
