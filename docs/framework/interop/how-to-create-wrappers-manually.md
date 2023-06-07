---
title: 如何：手动创建包装器
description: 手动创建 COM 类型的包装器。 使用现有 IDL 文件或类型库，或者创建托管声明，并将程序集导出到类型库。
ms.date: 03/30/2017
helpviewer_keywords:
- wrappers, creating manually
ms.assetid: cc2a70d8-6a58-4071-a8cf-ce28c018c09b
ms.openlocfilehash: 0321e98e6237e54397db7e583546fd69803ab7e1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275952"
---
# <a name="how-to-create-wrappers-manually"></a>如何：手动创建包装器

如果决定在托管源代码中手动声明 COM 类型，则最佳的着手点是现有的接口定义语言 (IDL) 文件或类型库。 不具备 IDL 文件或无法生成类型库文件时，可以通过创建托管的声明并将生成的程序集导出到类型库来模拟 COM 类型。  
  
### <a name="to-simulate-com-types-from-managed-source"></a>从托管源模拟 COM 类型  
  
1. 在符合公共语言规范 (CLS) 的语言中声明类型并编译文件。  
  
2. 使用[类型库导出程序 (Tlbexp.exe)](../tools/tlbexp-exe-type-library-exporter.md) 导出包含类型的程序集。  
  
3. 将导出的 COM 类型库用作声明面向 COM 托管类型的基础。  
  
### <a name="to-create-a-runtime-callable-wrapper-rcw"></a>创建运行时可调用包装器 (RCW)  
  
1. 假设你有一个 IDL 文件或类型库文件，请决定要在自定义 RCW 中包含哪些类和接口。 可以排除不打算在应用程序中直接或间接使用的任何类型。  
  
2. 在符合 CLS 的语言中创建一个源文件，并声明类型。 有关导入转换过程的完整说明，请参阅[有关从类型库转换到程序集的摘要](/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100))。 实际上，创建自定义 RCW 时，需要手动执行由[类型库导入程序 (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md) 所提供的类型转换活动。 下一章节的示例显示 IDL 或类型库文件中的类型以及它们在 C# 代码中的对应类型。  
  
3. 完成声明后，请像编译任何其他托管源代码一样编译此文件。  
  
4. 与使用 Tlbimp.exe 导入的类型相同，某些类型需要其他信息，你可以将其直接添加到自己的代码中。 有关详细信息，请参阅[如何：编辑互操作程序集](/previous-versions/dotnet/netframework-4.0/8zbc969t(v=vs.100))。  
  
## <a name="example"></a>示例  

 以下代码演示了 IDL 中的 `ISATest` 接口和 `SATest` 类以及 C# 源代码中相应类型的示例。  
  
 **IDL 或类型库文件**  
  
```cpp
 [  
object,  
uuid(40A8C65D-2448-447A-B786-64682CBEF133),  
dual,  
helpstring("ISATest Interface"),  
pointer_default(unique)  
 ]  
interface ISATest : IDispatch  
 {  
[id(1), helpstring("method InSArray")]
HRESULT InSArray([in] SAFEARRAY(int) *ppsa, [out,retval] int *pSum);  
 };  
 [  
uuid(116CCA1E-7E39-4515-9849-90790DA6431E),  
helpstring("SATest Class")  
 ]  
coclass SATest  
 {  
  [default] interface ISATest;  
 };  
```  
  
 **托管源代码的包装器**  
  
```csharp  
using System;  
using System.Runtime.InteropServices;  
using System.Runtime.CompilerServices;  
  
[assembly:Guid("E4A992B8-6F5C-442C-96E7-C4778924C753")]  
[assembly:ImportedFromTypeLib("SAServerLib")]  
namespace SAServer  
{  
 [ComImport]  
 [Guid("40A8C65D-2448-447A-B786-64682CBEF133")]  
 [TypeLibType(TypeLibTypeFlags.FLicensed)]  
 public interface ISATest  
 {  
  [DispId(1)]  
  //[MethodImpl(MethodImplOptions.InternalCall,  
  // MethodCodeType=MethodCodeType.Runtime)]  
  int InSArray( [MarshalAs(UnmanagedType.SafeArray,  
      SafeArraySubType=VarEnum.VT_I4)] ref int[] param );  
 }
 [ComImport]  
 [Guid("116CCA1E-7E39-4515-9849-90790DA6431E")]  
 [ClassInterface(ClassInterfaceType.None)]  
 [TypeLibType(TypeLibTypeFlags.FCanCreate)]  
 public class SATest : ISATest  
 {  
  [DispId(1)]  
  [MethodImpl(MethodImplOptions.InternalCall,
  MethodCodeType=MethodCodeType.Runtime)]  
  extern int ISATest.InSArray( [MarshalAs(UnmanagedType.SafeArray,
  SafeArraySubType=VarEnum.VT_I4)] ref int[] param );  
 }  
}  
```  
  
## <a name="see-also"></a>请参阅

- [自定义运行时可调用包装器](/previous-versions/dotnet/netframework-4.0/e753eftz(v=vs.100))
- [COM 数据类型](/previous-versions/dotnet/netframework-4.0/sak564ww(v=vs.100))
- [如何：编辑互操作程序集](/previous-versions/dotnet/netframework-4.0/8zbc969t(v=vs.100))
- [有关从类型库转换到程序集的摘要](/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100))
- [Tlbimp.exe（类型库导入程序）](../tools/tlbimp-exe-type-library-importer.md)
- [Tlbexp.exe（类型库导出程序）](../tools/tlbexp-exe-type-library-exporter.md)
