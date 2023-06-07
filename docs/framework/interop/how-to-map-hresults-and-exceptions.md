---
title: 如何：映射 HRESULT 和异常
description: 查看如何将从 COM 方法返回的 HRESULT 值映射到 .NET 方法引发的异常。 运行时处理 COM 和 .NET 之间的转换。
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- interoperation with unmanaged code, HRESULTs
- exceptions, HRESULTs
- interoperation with unmanaged code, exceptions
- HRESULTs
- COM interop, HRESULTs
- COM interop, exceptions
ms.assetid: 610b364b-2761-429d-9c4a-afbc3e66f1b9
ms.openlocfilehash: ea5d920ccfa8a91c9fdc9d95c6165c8dfb52c6ff
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96267034"
---
# <a name="how-to-map-hresults-and-exceptions"></a>如何：映射 HRESULT 和异常

COM 方法通过返回 HRESULT 来报告错误；NET 方法通过引发异常来报告错误。 运行时处理这两者之间的转换。 NET Framework 中的每个异常类都将映射到 HRESULT。  
  
 用户定义的异常类可指定任何适用的 HRESULT。 这些异常类可动态更改通过设置异常对象上的 HResult 字段生成异常时返回的 HRESULT。 异常的其他信息通过 IErrorInfo 接口提供给客户端，这是在非管理进程中的 NET 对象上实现的。  
  
 如果要创建扩展 System.Exception 的类，需要在构造时设置 HRESULT 字段。 否则，基类将分配 HRESULT 值。 可以通过提供异常的构造函数的值，将新的异常类映射到现有的 HRESULT。  
  
 请注意，如果线程上存在 `IErrorInfo`，运行时将忽略 `HRESULT`。  此行为可以发生在 `HRESULT` 和`IErrorInfo` 表示不同错误的事例中。  
  
### <a name="to-create-a-new-exception-class-and-map-it-to-an-hresult"></a>创建新的异常类并将其映射到 HRESULT  
  
1. 使用以下代码创建名为 `NoAccessException` 的新的异常类，并将其映射到 HRESULT `E_ACCESSDENIED`。  
  
    ```cpp  
    Class NoAccessException : public ApplicationException  
    {  
        NoAccessException () {  
        HResult = E_ACCESSDENIED;
    }  
    }  
    CMyClass::MethodThatThrows  
    {  
    throw new NoAccessException();  
    }  
    ```  
  
 可能会遇到同时使用托管和非托管代码的程序（使用任何编程语言）。 例如，以下代码示例中的自定义封送处理程序使用 Marshal.ThrowExceptionForHR (int HResult) 方法，以使用特定 HRESULT 值引发异常。 此方法查找 HRESULT，并生成适当的异常类型。 例如，以下代码片段中的 HRESULT 可生成 ArgumentException。  
  
```cpp  
CMyClass::MethodThatThrows  
{  
    Marshal.ThrowExceptionForHR(COR_E_ARGUMENT);  
}  
```  
  
 下表提供从每个 HRESULT 到其在 NET Framework 中的相似异常类的完整映射。  
  
|HRESULT|.NET 异常|  
|-------------|--------------------|  
|**MSEE_E_APPDOMAINUNLOADED**|**AppDomainUnloadedException**|  
|**COR_E_APPLICATION**|**ApplicationException**|  
|**COR_E_ARGUMENT 或 E_INVALIDARG**|**ArgumentException**|  
|**COR_E_ARGUMENTOUTOFRANGE**|**ArgumentOutOfRangeException**|  
|**COR_E_ARITHMETIC 或 ERROR_ARITHMETIC_OVERFLOW**|**ArithmeticException**|  
|**COR_E_ARRAYTYPEMISMATCH**|**ArrayTypeMismatchException**|  
|**COR_E_BADIMAGEFORMAT 或 ERROR_BAD_FORMAT**|**BadImageFormatException**|  
|**COR_E_COMEMULATE_ERROR**|**COMEmulateException**|  
|**COR_E_CONTEXTMARSHAL**|**ContextMarshalException**|  
|**COR_E_CORE**|**CoreException**|  
|**NTE_FAIL**|**CryptographicException**|  
|**COR_E_DIRECTORYNOTFOUND 或 ERROR_PATH_NOT_FOUND**|**DirectoryNotFoundException**|  
|**COR_E_DIVIDEBYZERO**|**DivideByZeroException**|  
|**COR_E_DUPLICATEWAITOBJECT**|**DuplicateWaitObjectException**|  
|**COR_E_ENDOFSTREAM**|**EndOfStreamException**|  
|**COR_E_TYPELOAD**|**EntryPointNotFoundException**|  
|**COR_E_EXCEPTION**|**Exception**|  
|**COR_E_EXECUTIONENGINE**|**ExecutionEngineException**|  
|**COR_E_FIELDACCESS**|**FieldAccessException**|  
|**COR_E_FILENOTFOUND 或 ERROR_FILE_NOT_FOUND**|**FileNotFoundException**|  
|**COR_E_FORMAT**|**FormatException**|  
|**COR_E_INDEXOUTOFRANGE**|**IndexOutOfRangeException**|  
|**COR_E_INVALIDCAST 或 E_NOINTERFACE**|**InvalidCastException**|  
|**COR_E_INVALIDCOMOBJECT**|**InvalidComObjectException**|  
|**COR_E_INVALIDFILTERCRITERIA**|**InvalidFilterCriteriaException**|  
|**COR_E_INVALIDOLEVARIANTTYPE**|**InvalidOleVariantTypeException**|  
|**COR_E_INVALIDOPERATION**|**InvalidOperationException**|  
|**COR_E_IO**|**IOException**|  
|**COR_E_MEMBERACCESS**|**AccessException**|  
|**COR_E_METHODACCESS**|**MethodAccessException**|  
|**COR_E_MISSINGFIELD**|**MissingFieldException**|  
|**COR_E_MISSINGMANIFESTRESOURCE**|**MissingManifestResourceException**|  
|**COR_E_MISSINGMEMBER**|**MissingMemberException**|  
|**COR_E_MISSINGMETHOD**|**MissingMethodException**|  
|**COR_E_MULTICASTNOTSUPPORTED**|**MulticastNotSupportedException**|  
|**COR_E_NOTFINITENUMBER**|**NotFiniteNumberException**|  
|**E_NOTIMPL**|**NotImplementedException**|  
|**COR_E_NOTSUPPORTED**|**NotSupportedException**|  
|**COR_E_NULLREFERENCE orE_POINTER**|**NullReferenceException**|  
|**COR_E_OUTOFMEMORY 或**<br /><br /> **E_OUTOFMEMORY**|**OutOfMemoryException**|  
|**COR_E_OVERFLOW**|**OverflowException**|  
|**COR_E_PATHTOOLONG 或 ERROR_FILENAME_EXCED_RANGE**|**PathTooLongException**|  
|**COR_E_RANK**|**RankException**|  
|**COR_E_REFLECTIONTYPELOAD**|**ReflectionTypeLoadException**|  
|**COR_E_REMOTING**|**RemotingException**|  
|**COR_E_SAFEARRAYTYPEMISMATCH**|**SafeArrayTypeMismatchException**|  
|**COR_E_SECURITY**|**SecurityException**|  
|**COR_E_SERIALIZATION**|**SerializationException**|  
|**COR_E_STACKOVERFLOW orERROR_STACK_OVERFLOW**|**StackOverflowException**|  
|**COR_E_SYNCHRONIZATIONLOCK**|**SynchronizationLockException**|  
|**COR_E_SYSTEM**|**SystemException**|  
|**COR_E_TARGET**|**TargetException**|  
|**COR_E_TARGETINVOCATION**|**TargetInvocationException**|  
|**COR_E_TARGETPARAMCOUNT**|**TargetParameterCountException**|  
|**COR_E_THREADABORTED**|**ThreadAbortException**|  
|**COR_E_THREADINTERRUPTED**|**ThreadInterruptedException**|  
|**COR_E_THREADSTATE**|**ThreadStateException**|  
|**COR_E_THREADSTOP**|**ThreadStopException**|  
|**COR_E_TYPELOAD**|**TypeLoadException**|  
|**COR_E_TYPEINITIALIZATION**|**TypeInitializationException**|  
|**COR_E_VERIFICATION**|**VerificationException**|  
|**COR_E_WEAKREFERENCE**|**WeakReferenceException**|  
|**COR_E_VTABLECALLSNOTSUPPORTED**|**VTableCallsNotSupportedException**|  
|**所有其他 HRESULT**|**COMException**|  
  
 若要检索扩展的错误信息，托管客户端需要检查已生成的异常对象的字段。 若要异常对象提供有关错误的有用信息，COM 对象需要实现 IErrorInfo 接口。 运行时使用由 IErrorInfo 提供的信息来初始化异常对象。  
  
 如果 COM 对象不支持 IErrorInfo，运行时将使用默认值初始化异常对象。 下表列出与异常对象关联的每个字段，并标识了当 COM 对象支持 IErrorInfo 时的默认信息源。  
  
 请注意，如果线程上存在 `IErrorInfo`，运行时将忽略 `HRESULT`。  此行为可以发生在 `HRESULT` 和`IErrorInfo` 表示不同错误的事例中。  
  
|异常字段|来自 COM 的信息源|  
|---------------------|------------------------------------|  
|**ErrorCode**|从调用返回的 HRESULT。|  
|**HelpLink**|如果 IErrorInfo-> HelpContext 为非零值，字符串通过串联 IErrorInfo-> GetHelpFile 和“#”以及 IErrorInfo-> GetHelpContext 形成。 否则，字符串将从 IErrorInfo-> GetHelpFile 返回。|  
|**InnerException**|通常为 null 引用（在 Visual Basic 中为 Nothing）。|  
|**消息**|从 IErrorInfo->GetDescription 返回的字符串。|  
|**源**|从 IErrorInfo->GetSource 返回的字符串。|  
|**StackTrace**|堆栈跟踪。|  
|**TargetSite**|返回出现故障的 HRESULT 的方法的名称。|  
  
 异常字段，如 Message、Source 和 StackTrace，不可用于 StackOverflowException。  
  
## <a name="see-also"></a>请参阅

- [高级 COM 互操作性](/previous-versions/dotnet/netframework-4.0/bd9cdfyx(v=vs.100))
- [异常](../../standard/exceptions/index.md)
