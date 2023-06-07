---
title: 数组的默认封送处理
description: 了解数组的默认封送处理。 查看托管数组、非托管数组、将数组参数传递给 .NET 代码，以及将数组传递给 COM。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- interop marshaling, arrays
- arrays, interop marshaling
ms.assetid: 8a3cca8b-dd94-4e3d-ad9a-9ee7590654bc
ms.openlocfilehash: b6a675bbbfb974d78539d02b31b500b03a2ac8f4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96256652"
---
# <a name="default-marshaling-for-arrays"></a>数组的默认封送处理

在完全由托管代码组成的应用程序中，公共语言运行时将数组类型作为 In/Out 参数传递。 而互操作封送拆收器默认将数组作为 In 参数传递。  
  
 使用[固定优化](copying-and-pinning.md)，blittable 数组在与同一单元中的对象交互时，可能看上去像是作为 In/Out 参数运行。 但是，如果随后将代码导出到用于生成跨计算机代理的类型库，且该库用于跨单元封送调用，则调用可还原为真正的 In 参数行为。  
  
 数组本就很复杂，而托管数组和非托管数组之间的差异使它们比其他 blittable 类型具有更多信息。  
  
## <a name="managed-arrays"></a>托管数组  

 可以有各种托管数组类型；但是，<xref:System.Array?displayProperty=nameWithType> 类是所有数组类型的基类。 System.Array 类的属性可确定数组的秩、长度、下限和上限，其方法可用于访问、搜索、复制、创建数组以及对数组排序。  
  
 这些数组类型是动态类型，在基类库中未定义相应的静态类型。 将元素类型和秩的每一种组合视作不同类型的数组非常方便。 因此，一维整数数组与一维 double 类型数组是不同的类型。 同样，二维整数数组与一维整数数组也不同。 比较类型时，不考虑数组界限。  
  
 如下表所示，托管数组的所有实例都必须有特定的元素类型、秩和下限。  
  
|托管数组类型|元素类型|级别|下限|签名表示法|  
|------------------------|------------------|----------|-----------------|------------------------|  
|ELEMENT_TYPE_ARRAY|由类型指定。|由秩指定。|由界限视情况指定。|*type* **[** *n*,*m* **]**|  
|ELEMENT_TYPE_CLASS|未知|未知|未知|**System.Array**|  
|ELEMENT_TYPE_SZARRAY|由类型指定。|1|0|*type* **[** *n* **]**|  
  
## <a name="unmanaged-arrays"></a>非托管数组  

 非托管数组是 COM 样式安全数组或 C 样式数组，其长度固定或可变。 安全数组是自我描述的数组，带有关联数组数据的类型、秩和界限。 C 样式数组是具有固定下限 0 的一维类型化数组。 封送处理服务对两种数组类型的支持均有限。  
  
## <a name="passing-array-parameters-to-net-code"></a>将数组参数传递给 .NET 代码  

 C 样式数组和安全数组都可以作为安全数组或 C 样式数组从非托管代码传递给 .NET 代码。 下表显示非托管类型值和导入的类型。  
  
|非托管类型|导入的类型|  
|--------------------|-------------------|  
|**SafeArray(** *Type* **)**|**ELEMENT_TYPE_SZARRAY** **\<** *ConvertedType* **>**<br /><br /> 秩 = 1，下限 = 0。 只有在托管签名中提供大小时，才知道大小。 不满足秩 = 1 或下限 = 0 的安全数组不能作为 SZARRAY 封送。|  
|*Type*  **[]**|**ELEMENT_TYPE_SZARRAY** **\<** *ConvertedType* **>**<br /><br /> 秩 = 1，下限 = 0。 只有在托管签名中提供大小时，才知道大小。|  
  
### <a name="safe-arrays"></a>安全数组  

 从类型库将安全数组导入 .NET 程序集时，该数组转换为已知类型（例如 int）的一维数组。 适用于参数的类型转换规则同样适用于数组元素。 例如，BSTR 类型的安全数组可变为托管字符串数组，而变体的安全数组可变为托管对象数组。 从类型库中捕获 SAFEARRAY 元素类型并将它保存在 <xref:System.Runtime.InteropServices.UnmanagedType> 枚举的 SAFEARRAY 值中 。  
  
 由于无法根据类型库确定安全数组的秩和界限，因此假定秩等于 1，下限等于 0。 必须在由[类型库导入程序 (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md) 生成的托管签名中定义秩和界限。 如果运行时传递给方法的秩不同，则会引发 <xref:System.Runtime.InteropServices.SafeArrayRankMismatchException>。 如果运行时传递的数组的类型不同，则会引发 <xref:System.Runtime.InteropServices.SafeArrayTypeMismatchException>。 下面的示例演示托管代码和非托管代码中的安全数组。  
  
 非托管的签名  
  
```cpp
HRESULT New1([in] SAFEARRAY( int ) ar);  
HRESULT New2([in] SAFEARRAY( DATE ) ar);  
HRESULT New3([in, out] SAFEARRAY( BSTR ) *ar);  
```  
  
 托管的签名  
  
```vb  
Sub New1(<MarshalAs(UnmanagedType.SafeArray, SafeArraySubType:=VT_I4)> _  
   ar() As Integer)  
Sub New2(<MarshalAs(UnmanagedType.SafeArray, SafeArraySubType:=VT_DATE)> _
   ar() As DateTime)  
Sub New3(ByRef <MarshalAs(UnmanagedType.SafeArray, SafeArraySubType:=VT_BSTR)> _
   ar() As String)  
```  
  
```csharp  
void New1([MarshalAs(UnmanagedType.SafeArray, SafeArraySubType=VT_I4)] int[] ar) ;  
void New2([MarshalAs(UnmanagedType.SafeArray, SafeArraySubType=VT_DATE)]
   DateTime[] ar);  
void New3([MarshalAs(UnmanagedType.SafeArray, SafeArraySubType=VT_BSTR)]
   ref String[] ar);  
```  
  
 如果修改由 Tlbimp.exe 产生的方法签名以指示元素类型 ELEMENT_TYPE_ARRAY 而非 ELEMENT_TYPE_SZARRAY，则可将多维或非零界限安全数组封送到托管代码中 。 或者，可将 /sysarray 开关与 Tlbimp.exe 一起使用，将所有数组作为 <xref:System.Array?displayProperty=nameWithType> 对象导入。 如果已知正在传递的数组是多维数组，则可编辑由 Tlbimp.exe 生成的 Microsoft 中间语言 (MSIL) 代码，然后重新编译它。 有关如何修改 MSIL 代码的详细信息，请参阅[自定义运行时可调用包装器](/previous-versions/dotnet/netframework-4.0/e753eftz(v=vs.100))。  
  
### <a name="c-style-arrays"></a>C 样式数组  

 将 C 样式数组从类型库导入 .NET 程序集中时，数组被转换为 ELEMENT_TYPE_SZARRAY。  
  
 数组元素类型根据类型库确定并在导入期间保留。 适用于参数的转换规则同样适用于数组元素。 例如，LPStr 类型的数组变为字符串类型的数组 。 Tlbimp.exe 捕获数组元素类型并将 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性应用于该参数。  
  
 假定数组秩等于 1。 如果秩大于 1，则将按列主序将数组作为一维数组封送。 下限始终等于 0。  
  
 类型库可包含定长数组或变长数组。 Tlbimp.exe 只能从类型库中导入定长数组，这是因为类型库缺少封送变长数组所需的信息。 对于定长数组，从类型库导入大小，并在应用于参数的 MarshalAsAttribute 中捕获大小。  
  
 必须手动定义含变长数组的类型库，如以下示例所示。  
  
 非托管的签名  
  
```cpp
HRESULT New1(int ar[10]);  
HRESULT New2(double ar[10][20]);  
HRESULT New3(LPWStr ar[10]);  
```  
  
 托管的签名  
  
```vb  
Sub New1(<MarshalAs(UnmanagedType.LPArray, SizeConst=10)> _  
   ar() As Integer)  
Sub New2(<MarshalAs(UnmanagedType.LPArray, SizeConst=200)> _  
   ar() As Double)  
Sub New2(<MarshalAs(UnmanagedType.LPArray, _  
   ArraySubType=UnmanagedType.LPWStr, SizeConst=10)> _  
   ar() As String)  
```  
  
```csharp  
void New1([MarshalAs(UnmanagedType.LPArray, SizeConst=10)] int[] ar);  
void New2([MarshalAs(UnmanagedType.LPArray, SizeConst=200)] double[] ar);  
void New2([MarshalAs(UnmanagedType.LPArray,
   ArraySubType=UnmanagedType.LPWStr, SizeConst=10)] String[] ar);  
```  
  
 虽然可将 size_is 或 length_is 属性应用于接口定义语言 (IDL) 源中的数组，以便将大小传达给客户端，但是 Microsoft 接口定义语言 (MIDL) 编译器不会将该信息传送到类型库 。 如果不知道大小，互操作封送处理服务就无法封送数组元素。 因此，将变长数组作为引用参数导入。 例如：  
  
 非托管的签名  
  
```cpp
HRESULT New1(int ar[]);  
HRESULT New2(int ArSize, [size_is(ArSize)] double ar[]);  
HRESULT New3(int ElemCnt, [length_is(ElemCnt)] LPStr ar[]);  
```  
  
 托管的签名  
  
```vb  
Sub New1(ByRef ar As Integer)  
Sub New2(ByRef ar As Double)  
Sub New3(ByRef ar As String)  
```  
  
```csharp  
void New1(ref int ar);
void New2(ref double ar);
void New3(ref String ar);
```  
  
 编辑由 Tlbimp.exe 生成的 Microsoft 中间语言 (MSIL) 代码，再重新编译该代码，即可向封送拆收器提供数组大小。 有关如何修改 MSIL 代码的详细信息，请参阅[自定义运行时可调用包装器](/previous-versions/dotnet/netframework-4.0/e753eftz(v=vs.100))。 要指示数组中的元素数，请采用以下任一方式将 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 类型应用于托管方法定义的数组参数：  
  
- 标识含数组中的元素数的另一个参数。 按位置标识参数，从第一个参数开始，将第一个参数标识为数字 0。
  
    ```vb  
    Sub [New](ElemCnt As Integer, _  
       \<MarshalAs(UnmanagedType.LPArray, SizeParamIndex:=1)> _  
       ar() As Integer)  
    ```  
  
    ```csharp  
    void New(  
       int ElemCnt,
       [MarshalAs(UnmanagedType.LPArray, SizeParamIndex=0)] int[] ar );  
    ```  
  
- 将数组大小定义为常数。 例如：  
  
    ```vb  
    Sub [New](\<MarshalAs(UnmanagedType.LPArray, SizeConst:=128)> _  
       ar() As Integer)  
    ```  
  
    ```csharp  
    void New(  
       [MarshalAs(UnmanagedType.LPArray, SizeConst=128)] int[] ar );  
    ```  
  
 将数组从非托管代码封送到托管代码时，封送拆收器检查与参数关联的 MarshalAsAttribute 以确定数组大小。 如果未指定数组大小，则只封送一个元素。  
  
> [!NOTE]
> 将托管数组封送到非托管代码不会受 MarshalAsAttribute 的影响。 在该方向上，数组大小通过检查确定。 无法封送托管数组的子集。  
  
 互操作封送拆收器使用 CoTaskMemAlloc 和 CoTaskMemFree 方法分配和检索内存 。 非托管代码所执行的内存分配也必须使用这些方法。  
  
## <a name="passing-arrays-to-com"></a>将数组传递给 COM  

 所有托管数组类型都可以从托管代码传递给非托管代码。 根据托管类型和应用于它的属性，可将数组作为安全数组或 C 样式数组进行访问，如下表所示。  
  
|托管数组类型|导出结果|  
|------------------------|-----------------|  
|**ELEMENT_TYPE_SZARRAY** **\<** *type* **>**|<xref:System.Runtime.InteropServices.UnmanagedType> **.SafeArray(** *type* **)**<br /><br /> UnmanagedType.LPArray<br /><br /> 签名中提供了类型。 秩始终为 1，下限始终为 0。 在运行时大小始终为已知。|  
|**ELEMENT_TYPE_ARRAY** **\<** *type* **>** **\<** *rank* **>**[**\<** *bounds* **>**]|**UnmanagedType.SafeArray(** *type* **)**<br /><br /> UnmanagedType.LPArray<br /><br /> 签名中提供了类型、秩和界限。 在运行时大小始终为已知。|  
|**ELEMENT_TYPE_CLASS** **\<**<xref:System.Array?displayProperty=nameWithType>**>**|UT_Interface<br /><br /> **UnmanagedType.SafeArray(** *type* **)**<br /><br /> 在运行时类型、秩、界限和大小始终为已知。|  
  
 在与含有 LPSTR 或 LPWSTR 的结构数组相关的 OLE 自动化中，存在一项限制。  因此，必须将 String 字段作为 UnmanagedType.BSTR 封送 。 否则，将引发异常。  
  
### <a name="element_type_szarray"></a>ELEMENT_TYPE_SZARRAY  

 将包含 ELEMENT_TYPE_SZARRAY 参数（一维数组）的方法从 .NET 程序集导出到类型库时，会将该数组参数转换为给定类型的 SAFEARRAY 。 同样的转换规则也适用于数组元素类型。 自动将托管数组的内容从托管内存复制到 SAFEARRAY 中。 例如：  
  
#### <a name="managed-signature"></a>托管的签名  
  
```vb  
Sub [New](ar() As Long)  
Sub [New](ar() As String)  
```  
  
```csharp  
void New(long[] ar );  
void New(String[] ar );  
```  
  
#### <a name="unmanaged-signature"></a>非托管的签名  
  
```cpp
HRESULT New([in] SAFEARRAY( long ) ar);
HRESULT New([in] SAFEARRAY( BSTR ) ar);  
```  
  
 安全数组的秩始终为 1，下限始终为 0。 大小在运行时由所传递的托管数组的大小确定。  
  
 还可以通过使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性将数组作为 C 样式数组封送。 例如：  
  
#### <a name="managed-signature"></a>托管的签名  
  
```vb  
Sub [New](<MarshalAs(UnmanagedType.LPArray, SizeParamIndex:=1)> _  
   ar() As Long, size as Integer)  
Sub [New](<MarshalAs(UnmanagedType.LPArray, SizeParamIndex:=1)> _  
   ar() As String, size as Integer)  
Sub [New](<MarshalAs(UnmanagedType.LPArray, _  
   ArraySubType= UnmanagedType.LPStr, SizeParamIndex:=1)> _  
   ar() As String, size as Integer)  
```  
  
```csharp  
void New([MarshalAs(UnmanagedType.LPArray, SizeParamIndex=1)]
   long [] ar, int size );  
void New([MarshalAs(UnmanagedType.LPArray, SizeParamIndex=1)]
   String [] ar, int size );  
void New([MarshalAs(UnmanagedType.LPArray, ArraySubType=
   UnmanagedType.LPStr, SizeParamIndex=1)]
   String [] ar, int size );  
```  
  
#### <a name="unmanaged-signature"></a>非托管的签名  
  
```cpp
HRESULT New(long ar[]);
HRESULT New(BSTR ar[]);
HRESULT New(LPStr ar[]);  
```  
  
 虽然封送拆收器具有封送数组所需的长度信息，但通常会将数组长度作为单独的参数传递，以便将长度传达给被调用方。  
  
### <a name="element_type_array"></a>ELEMENT_TYPE_ARRAY  

 将包含 ELEMENT_TYPE_ARRAY 参数的方法从 .NET 程序集导出到类型库时，会将该数组参数转换为给定类型的 SAFEARRAY 。 自动将托管数组的内容从托管内存复制到 SAFEARRAY 中。 例如：  
  
#### <a name="managed-signature"></a>托管的签名  
  
```vb  
Sub [New](ar(,) As Long)  
Sub [New](ar(,) As String)  
```  
  
```csharp  
void New( long [,] ar );  
void New( String [,] ar );  
```  
  
#### <a name="unmanaged-signature"></a>非托管的签名  
  
```cpp
HRESULT New([in] SAFEARRAY( long ) ar);
HRESULT New([in] SAFEARRAY( BSTR ) ar);  
```  
  
 安全数组的秩、大小和界限在运行时由托管数组的特征确定。  
  
 还可以通过应用 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性将数组作为 C 样式数组封送。 例如：  
  
#### <a name="managed-signature"></a>托管的签名  
  
```vb  
Sub [New](<MarshalAs(UnmanagedType.LPARRAY, SizeParamIndex:=1)> _  
   ar(,) As Long, size As Integer)  
Sub [New](<MarshalAs(UnmanagedType.LPARRAY, _  
   ArraySubType:=UnmanagedType.LPStr, SizeParamIndex:=1)> _  
   ar(,) As String, size As Integer)  
```  
  
```csharp  
void New([MarshalAs(UnmanagedType.LPARRAY, SizeParamIndex=1)]
   long [,] ar, int size );  
void New([MarshalAs(UnmanagedType.LPARRAY,
   ArraySubType= UnmanagedType.LPStr, SizeParamIndex=1)]
   String [,] ar, int size );  
```  
  
#### <a name="unmanaged-signature"></a>非托管的签名  
  
```cpp
HRESULT New(long ar[]);
HRESULT New(LPStr ar[]);  
```  
  
 无法封送嵌套数组。 例如，使用[类型库导出程序 (Tlbexp.exe)](../tools/tlbexp-exe-type-library-exporter.md) 进行导出时，以下签名将生成错误。  
  
#### <a name="managed-signature"></a>托管的签名  
  
```vb  
Sub [New](ar()()() As Long)  
```  
  
```csharp  
void New(long [][][] ar );  
```  
  
### <a name="element_type_class-systemarray"></a>ELEMENT_TYPE_CLASS \<System.Array>  

 将包含 <xref:System.Array?displayProperty=nameWithType> 参数的方法从 .NET 程序集导出到类型库时，会将该数组参数转换为 _Array 接口。 只能通过 _Array 接口的方法和属性访问托管数组的内容。 还可通过使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性将 System.Array 作为 SAFEARRAY 封送 。 作为安全数组封送时，将数组元素视作变体封送。 例如：  
  
#### <a name="managed-signature"></a>托管的签名  
  
```vb  
Sub New1( ar As System.Array )  
Sub New2( <MarshalAs(UnmanagedType.Safe array)> ar As System.Array )  
```  
  
```csharp  
void New1( System.Array ar );  
void New2( [MarshalAs(UnmanagedType.Safe array)] System.Array ar );  
```  
  
#### <a name="unmanaged-signature"></a>非托管的签名  
  
```cpp
HRESULT New([in] _Array *ar);
HRESULT New([in] SAFEARRAY(VARIANT) ar);  
```  
  
### <a name="arrays-within-structures"></a>结构中的数组  

 非托管结构可包含嵌入数组。 默认情况下，将这些嵌入数组字段作为 SAFEARRAY 封送。 在以下示例中，`s1` 是直接在结构本身中分配的嵌入数组。  
  
#### <a name="unmanaged-representation"></a>非托管表示形式  
  
```cpp
struct MyStruct {  
    short s1[128];  
}  
```  
  
 数组可作为 <xref:System.Runtime.InteropServices.UnmanagedType> 封送，这需要设置 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 字段。 只能将大小设置为常数。 以下代码演示 `MyStruct` 的相应托管定义。  
  
```vb  
Public Structure <StructLayout(LayoutKind.Sequential)> MyStruct  
   Public <MarshalAs(UnmanagedType.ByValArray, SizeConst := 128)> _  
     s1() As Short  
End Structure  
```  
  
```csharp  
[StructLayout(LayoutKind.Sequential)]  
public struct MyStruct {  
   [MarshalAs(UnmanagedType.ByValArray, SizeConst=128)] public short[] s1;  
}  
```  
  
## <a name="see-also"></a>请参阅

- [默认封送处理行为](default-marshaling-behavior.md)
- [可直接复制到本机结构中的类型和非直接复制到本机结构中的类型](blittable-and-non-blittable-types.md)
- [方向特性](/previous-versions/dotnet/netframework-4.0/77e6taeh(v=vs.100))
- [复制和锁定](copying-and-pinning.md)
