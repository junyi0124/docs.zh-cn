---
title: 如何：运行沙盒中部分受信任的代码
description: 了解如何在 .NET 中的沙盒中运行部分受信任的代码。 AppDomain 类是一种对托管应用程序进行沙盒处理的有效方法。
ms.date: 03/30/2017
helpviewer_keywords:
- partially trusted code
- sandboxing
- partial trust
- restricted security environment
- code security, sandboxing
ms.assetid: d1ad722b-5b49-4040-bff3-431b94bb8095
ms.openlocfilehash: baa04a3c55728590b8aa502648a8ab42bf62f903
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96288276"
---
# <a name="how-to-run-partially-trusted-code-in-a-sandbox"></a>如何：运行沙盒中部分受信任的代码

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 沙盒处理是在受限制的安全环境中运行代码的做法，从而限制授予代码的访问权限。 例如，如果你有来自不完全信任的源的托管库，则不应将其作为完全受信任运行。 相反，应将代码放在将权限限制为预计需要的权限（例如 <xref:System.Security.Permissions.SecurityPermissionFlag.Execution> 权限）的沙盒中。  
  
 沙盒处理还可用于测试将在部分受信任环境中运行的分发的代码。  
  
 <xref:System.AppDomain> 是为托管应用程序提供沙盒的有效途径。 用于运行部分受信任的代码的应用程序域具有定义受保护资源的权限，这些资源在于 <xref:System.AppDomain> 内运行时可用。 在 <xref:System.AppDomain> 内运行的代码由与 <xref:System.AppDomain> 关联的权限约束并且仅能访问指定的资源。 <xref:System.AppDomain> 还包括用于标识程序集（作为完全受信任的程序集加载）的 <xref:System.Security.Policy.StrongName> 数组。 这使 <xref:System.AppDomain> 的创建者能启动新沙盒域，该域允许特定的帮助程序程序集为完全受信任的程序集。 将程序集加载为完全受信任的程序集的另一种方法是：将其放在全局程序集缓存中；但是，这会在该计算机上创建的所有应用程序域中将程序集加载为完全受信任的程序集。 强名称列表支持提供更多限制性决定的每个 <xref:System.AppDomain> 的决策。  
  
 可以使用 <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29?displayProperty=nameWithType> 方法重载为在沙盒中运行的应用程序指定权限集。 此重载使你能够指定所需代码访问安全性的确切级别。 使用此重载加载到 <xref:System.AppDomain> 中的程序集可以仅具有指定的授予集，也可以是完全受信任的程序集。 如果程序集位于全局程序集缓存中或在 `fullTrustAssemblies`（<xref:System.Security.Policy.StrongName>）数组参数中列出，则该程序集被授予完全信任。 只应将已知完全受信任的程序集添加到 `fullTrustAssemblies` 列表中。  
  
 重载具有以下签名：  
  
```csharp
AppDomain.CreateDomain( string friendlyName,  
                        Evidence securityInfo,  
                        AppDomainSetup info,  
                        PermissionSet grantSet,  
                        params StrongName[] fullTrustAssemblies);  
```  
  
 <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29> 方法重载的参数指定 <xref:System.AppDomain> 的名称、<xref:System.AppDomain> 的证据、标识沙盒的应用程序基的 <xref:System.AppDomainSetup> 对象、要使用的权限集和完全受信任的程序集的强名称。  
  
 出于安全原因，在 `info` 参数中指定的应用程序基不应为主机应用程序的应用程序基。  
  
 对于 `grantSet` 参数，可以指定已显式创建的权限集或由 <xref:System.Security.SecurityManager.GetStandardSandbox%2A> 方法创建的标准权限集。  
  
 与大多数 <xref:System.AppDomain> 加载不同，<xref:System.AppDomain> 的证据（由 `securityInfo` 参数提供）不用于确定部分受信任的程序集的授予集。 相反，它由 `grantSet` 参数单独指定。 但证据可以用于其它目的，例如确定独立存储范围。  
  
### <a name="to-run-an-application-in-a-sandbox"></a>在沙盒中运行应用程序  
  
1. 创建要授予给不受信任的应用程序的权限集。 可以授予的最小权限是 <xref:System.Security.Permissions.SecurityPermissionFlag.Execution> 权限。 还可以授予其它你认为可能对不受信任的代码安全的权限；例如，<xref:System.Security.Permissions.IsolatedStorageFilePermission>。 下面的代码创建仅具有 <xref:System.Security.Permissions.SecurityPermissionFlag.Execution> 权限的新权限集。  
  
    ```csharp
    PermissionSet permSet = new PermissionSet(PermissionState.None);  
    permSet.AddPermission(new SecurityPermission(SecurityPermissionFlag.Execution));  
    ```  
  
     或者，可以使用现有的已命名权限集，如 Internet。  
  
    ```csharp
    Evidence ev = new Evidence();  
    ev.AddHostEvidence(new Zone(SecurityZone.Internet));  
    PermissionSet internetPS = SecurityManager.GetStandardSandbox(ev);  
    ```  
  
     <xref:System.Security.SecurityManager.GetStandardSandbox%2A> 方法返回 `Internet` 权限集或 `LocalIntranet` 权限集，具体取决于证据中的区域。 <xref:System.Security.SecurityManager.GetStandardSandbox%2A> 还为一些作为引用传递的证据对象构造标识权限。  
  
2. 为包含调用不受信任的代码的宿主类（在此示例中，命名为 `Sandboxer`）的程序集签名。 将用于对程序集签名的 <xref:System.Security.Policy.StrongName> 添加到 <xref:System.AppDomain.CreateDomain%2A> 调用的 `fullTrustAssemblies` 参数的 <xref:System.Security.Policy.StrongName> 数组中。 主机类必须作为完全受信任的类运行，以启用部分信任代码的执行或为部分信任应用程序提供服务。 这就是读取程序集的 <xref:System.Security.Policy.StrongName> 的方式：  
  
    ```csharp
    StrongName fullTrustAssembly = typeof(Sandboxer).Assembly.Evidence.GetHostEvidence<StrongName>();  
    ```  
  
     不需要将 .NET framework 程序集（如 mscorlib 和 System.dll）添加到完全信任列表中，因为它们是作为完全受信任的程序集从全局程序集缓存中加载的。  
  
3. 初始化 <xref:System.AppDomain.CreateDomain%2A> 方法的 <xref:System.AppDomainSetup> 参数。 使用此参数，可以控制新的 <xref:System.AppDomain> 的许多设置。 <xref:System.AppDomainSetup.ApplicationBase%2A> 属性是一个重要设置，并且应不同于宿主应用程序的 <xref:System.AppDomain> 的 <xref:System.AppDomainSetup.ApplicationBase%2A> 属性。 如果 <xref:System.AppDomainSetup.ApplicationBase%2A> 设置相同，则部分信任应用程序可以获取主机应用程序以加载（作为完全受信任的应用程序）它定义的异常，从而攻击它。 这是为什么不建议使用 catch（异常）的另一个原因。 将主机的应用程序基设置为与沙盒应用程序的应用程序基不同，这可降低攻击风险。  
  
    ```csharp
    AppDomainSetup adSetup = new AppDomainSetup();  
    adSetup.ApplicationBase = Path.GetFullPath(pathToUntrusted);  
    ```  
  
4. 使用已指定的参数调用 <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29> 方法重载来创建应用程序域。  
  
     此方法的签名是：  
  
    ```csharp
    public static AppDomain CreateDomain(string friendlyName,
        Evidence securityInfo, AppDomainSetup info, PermissionSet grantSet,
        params StrongName[] fullTrustAssemblies)  
    ```  
  
     其他信息：  
  
    - 这是唯一将 <xref:System.Security.PermissionSet> 作为参数的 <xref:System.AppDomain.CreateDomain%2A> 方法的重载，因而它是唯一允许以部分信任设置加载应用程序的重载。  
  
    - `evidence` 参数不用于计算权限集；它由 .NET Framework 的其它功能用于标识。  
  
    - 此重载必须设置`info` 参数的 <xref:System.AppDomainSetup.ApplicationBase%2A> 属性。  
  
    - `fullTrustAssemblies` 参数具有 `params` 关键字，这意味着不需要创建 <xref:System.Security.Policy.StrongName> 数组。 允许将 0、1 或更多强名称作为参数传递。  
  
    - 要用于创建应用程序域的代码是：  
  
    ```csharp
    AppDomain newDomain = AppDomain.CreateDomain("Sandbox", null, adSetup, permSet, fullTrustAssembly);  
    ```  
  
5. 将代码加载到已创建的沙盒 <xref:System.AppDomain> 中。 可以通过两种方法实现此目的：  
  
    - 调用程序集的 <xref:System.AppDomain.ExecuteAssembly%2A> 方法。  
  
    - 使用 <xref:System.Activator.CreateInstanceFrom%2A> 方法来创建派生自新的 <xref:System.AppDomain> 中的 <xref:System.MarshalByRefObject> 的类的实例。  
  
     第二种方法非常可取，因为这样可以更轻松地将参数传递给新的 <xref:System.AppDomain> 实例。 <xref:System.Activator.CreateInstanceFrom%2A> 方法提供两大重要功能：  
  
    - 可以使用指向不包含你的程序集的位置的基本代码。  
  
    - 可以在完全信任 (<xref:System.Security.Permissions.PermissionState.Unrestricted?displayProperty=nameWithType>) 的 <xref:System.Security.CodeAccessPermission.Assert%2A> 下进行创建，这样就可以创建关键类的一个实例。  (如果程序集没有透明标记并加载为完全受信任，则会发生这种情况。 ) 因此，你必须小心地仅创建你信任的具有此函数的代码，我们建议你仅在新应用程序域中创建完全受信任的类的实例。  
  
    ```csharp
    ObjectHandle handle = Activator.CreateInstanceFrom(  
    newDomain, typeof(Sandboxer).Assembly.ManifestModule.FullyQualifiedName,  
           typeof(Sandboxer).FullName );  
    ```  
  
     若要在新域中创建类的实例，该类必须扩展 <xref:System.MarshalByRefObject> 类。
  
    ```csharp
    class Sandboxer:MarshalByRefObject  
    ```  
  
6. 将新的域实例解包到此域中的引用中。 此引用用于执行不受信任的代码。  
  
    ```csharp
    Sandboxer newDomainInstance = (Sandboxer) handle.Unwrap();  
    ```  
  
7. 调用你刚刚创建的 `Sandboxer` 类的实例中的 `ExecuteUntrustedCode` 方法。  
  
    ```csharp
    newDomainInstance.ExecuteUntrustedCode(untrustedAssembly, untrustedClass, entryPoint, parameters);  
    ```  
  
     此调用在具有受限权限的沙盒应用程序域中执行。  
  
    ```csharp
    public void ExecuteUntrustedCode(string assemblyName, string typeName, string entryPoint, Object[] parameters)  
    {  
        //Load the MethodInfo for a method in the new assembly. This might be a method you know, or
        //you can use Assembly.EntryPoint to get to the entry point in an executable.  
        MethodInfo target = Assembly.Load(assemblyName).GetType(typeName).GetMethod(entryPoint);  
        try  
        {  
            // Invoke the method.  
            target.Invoke(null, parameters);  
        }  
        catch (Exception ex)  
        {  
        //When information is obtained from a SecurityException extra information is provided if it is
        //accessed in full-trust.  
            new PermissionSet(PermissionState.Unrestricted).Assert();  
            Console.WriteLine("SecurityException caught:\n{0}", ex.ToString());  
            CodeAccessPermission.RevertAssert();  
            Console.ReadLine();  
        }  
    }  
    ```  
  
     <xref:System.Reflection> 用于获取部分受信任的程序集中的方法的句柄。 该句柄可以用于以安全的方式、使用最小权限来执行代码。  
  
     在前面的代码中，请在打印 <xref:System.Security.SecurityException> 之前备注完全信任权限的 <xref:System.Security.PermissionSet.Assert%2A>。  
  
    ```csharp
    new PermissionSet(PermissionState.Unrestricted).Assert()  
    ```  
  
     完全信任断言用于从 <xref:System.Security.SecurityException> 中获取扩展信息。 没有 <xref:System.Security.PermissionSet.Assert%2A>，则 <xref:System.Security.SecurityException> 的 <xref:System.Security.SecurityException.ToString%2A> 方法将发现堆栈上有部分受信任的代码，并将限制返回的信息。 如果部分信任代码可以读取该信息，可能会导致安全问题，但可以通过不授予 <xref:System.Security.Permissions.UIPermission> 来缓解此风险。 完全信任断言应慎用，且仅在确定不允许部分信任代码提升为完全信任代码时使用。 一般来说，不要调用同一函数中不信任的代码，也不要在调用完全信任断言后调用代码。 始终在完成使用断言后还原断言，这是个好做法。  
  
## <a name="example"></a>示例  

 下面的示例实现前一部分中的过程。 在此示例中，Visual Studio 解决方案中一个名为 `Sandboxer` 的项目还包含一个名为 `UntrustedCode` 的项目（该项目实现类 `UntrustedClass`）。 此方案假定你已下载库程序集，该程序集包含应该返回 `true` 或 `false` 的方法，以指示你提供的数字是否为斐波纳契数。 相反，该方法会尝试从你的计算机中读取文件。 下面的示例演示不受信任的代码。  
  
```csharp
using System;  
using System.IO;  
namespace UntrustedCode  
{  
    public class UntrustedClass  
    {  
        // Pretend to be a method checking if a number is a Fibonacci  
        // but which actually attempts to read a file.  
        public static bool IsFibonacci(int number)  
        {  
           File.ReadAllText("C:\\Temp\\file.txt");  
           return false;  
        }  
    }  
}  
```  
  
 下面的示例演示执行不受信任的代码的 `Sandboxer` 应用程序代码。  
  
```csharp
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using System.IO;  
using System.Security;  
using System.Security.Policy;  
using System.Security.Permissions;  
using System.Reflection;  
using System.Runtime.Remoting;  
  
//The Sandboxer class needs to derive from MarshalByRefObject so that we can create it in another
// AppDomain and refer to it from the default AppDomain.  
class Sandboxer : MarshalByRefObject  
{  
    const string pathToUntrusted = @"..\..\..\UntrustedCode\bin\Debug";  
    const string untrustedAssembly = "UntrustedCode";  
    const string untrustedClass = "UntrustedCode.UntrustedClass";  
    const string entryPoint = "IsFibonacci";  
    private static Object[] parameters = { 45 };  
    static void Main()  
    {  
        //Setting the AppDomainSetup. It is very important to set the ApplicationBase to a folder
        //other than the one in which the sandboxer resides.  
        AppDomainSetup adSetup = new AppDomainSetup();  
        adSetup.ApplicationBase = Path.GetFullPath(pathToUntrusted);  
  
        //Setting the permissions for the AppDomain. We give the permission to execute and to
        //read/discover the location where the untrusted code is loaded.  
        PermissionSet permSet = new PermissionSet(PermissionState.None);  
        permSet.AddPermission(new SecurityPermission(SecurityPermissionFlag.Execution));  
  
        //We want the sandboxer assembly's strong name, so that we can add it to the full trust list.  
        StrongName fullTrustAssembly = typeof(Sandboxer).Assembly.Evidence.GetHostEvidence<StrongName>();  
  
        //Now we have everything we need to create the AppDomain, so let's create it.  
        AppDomain newDomain = AppDomain.CreateDomain("Sandbox", null, adSetup, permSet, fullTrustAssembly);  
  
        //Use CreateInstanceFrom to load an instance of the Sandboxer class into the  
        //new AppDomain.
        ObjectHandle handle = Activator.CreateInstanceFrom(  
            newDomain, typeof(Sandboxer).Assembly.ManifestModule.FullyQualifiedName,  
            typeof(Sandboxer).FullName  
            );  
        //Unwrap the new domain instance into a reference in this domain and use it to execute the
        //untrusted code.  
        Sandboxer newDomainInstance = (Sandboxer) handle.Unwrap();  
        newDomainInstance.ExecuteUntrustedCode(untrustedAssembly, untrustedClass, entryPoint, parameters);  
    }  
    public void ExecuteUntrustedCode(string assemblyName, string typeName, string entryPoint, Object[] parameters)  
    {  
        //Load the MethodInfo for a method in the new Assembly. This might be a method you know, or
        //you can use Assembly.EntryPoint to get to the main function in an executable.  
        MethodInfo target = Assembly.Load(assemblyName).GetType(typeName).GetMethod(entryPoint);  
        try  
        {  
            //Now invoke the method.  
            bool retVal = (bool)target.Invoke(null, parameters);  
        }  
        catch (Exception ex)  
        {  
            // When we print informations from a SecurityException extra information can be printed if we are
            //calling it with a full-trust stack.  
            new PermissionSet(PermissionState.Unrestricted).Assert();  
            Console.WriteLine("SecurityException caught:\n{0}", ex.ToString());  
            CodeAccessPermission.RevertAssert();  
            Console.ReadLine();  
        }  
    }  
}  
```  
  
## <a name="see-also"></a>另请参阅

- [安全编码准则](../../standard/security/secure-coding-guidelines.md)
