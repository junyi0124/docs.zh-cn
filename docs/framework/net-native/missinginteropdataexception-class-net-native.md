---
title: 缺少互操作数据异常类 (.NET Native)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: eab4bcf8-9f5f-4731-87d8-842748a6062a
ms.openlocfilehash: bbbb484e5cb8060568b321a2a41474d60c9f87f6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96250913"
---
# <a name="missinginteropdataexception-class-net-native"></a>缺少互操作数据异常类 (.NET Native)

**适用于 Windows 10 的 .NET for Windows apps，仅 .NET Native**  
  
 当手动封送方法被调用但一个类型的元数据无法通过动态分析找到或无法在运行时指令文件中找到时，会引发该异常。  
  
 命名空间：System.Runtime.CompilerServices  
  
> [!IMPORTANT]
> `MissingInteropDataException`类仅供 .NET Native 工具链内部使用。 它不用于在第三方代码中使用，也不应用它处理应用程序代码中的异常。 相反，你可以通过将条目添加到[运行时指令文件](runtime-directives-rd-xml-configuration-file-reference.md)来消除异常。 有关详细信息，请参阅“备注”部分。  
  
## <a name="syntax"></a>语法  

 [!code-csharp[ProjectN#21](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/missinginteropdataexception_syntax1.cs#21)]
 [!code-vb[ProjectN#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/projectn/vb/missinginteropdataexception_syntax1.vb#21)]  
  
 `MissingInteropDataException` 类包含以下成员：  
  
## <a name="constructors"></a>构造函数  
  
|构造函数|说明|  
|-----------------|-----------------|  
|`public MissingInteropDataException(String resourceId, Type pertinentType)`|通过使用系统提供的消息（该消息描述错误以及缺少数据的类型）的 ID 初始化 `MissingInteropDataException` 类的新实例。 此构造函数仅供 .NET Native 工具链内部使用。|  
  
## <a name="properties"></a>属性  
  
|属性|描述|  
|--------------|-----------------|  
|`public IDictionary Data { get; }`|获取键/值对的集合，这些键/值对提供有关该异常的其他用户定义信息。 （从 <xref:System.Exception?displayProperty=nameWithType> 继承。）|  
|`public string HelpLink { get; set; }`|获取或设置指向与此异常关联的帮助文件链接。 （从 <xref:System.Exception?displayProperty=nameWithType> 继承。）|  
|`public int HResult { get; protected set; }`|获取或设置分配给特定异常的编码数值 `HRESULT`。 （从 <xref:System.Exception?displayProperty=nameWithType> 继承。）|  
|`public Exception InnerException { get; }`|获取导致当前异常的异常。 （从 <xref:System.Exception?displayProperty=nameWithType> 继承。）|  
|`public string Message { get; }`|获取描述当前异常的消息。 （从 <xref:System.Exception?displayProperty=nameWithType> 继承。）|  
|`public Type MissingType { get; private set; }`|获取或设置丢失数据的类型。|  
|`public string Source { get; set; }`|获取或设置引起错误的应用或对象名。 （从 <xref:System.Exception?displayProperty=nameWithType> 继承。）|  
|`public string StackTrace { get; }`|获取调用堆栈上的即时框架字符串表示形式。 （从 <xref:System.Exception?displayProperty=nameWithType> 继承。）|  
|`public MethodBase TargetSite { get; }`|获取引发当前异常的方法。 （从 <xref:System.Exception?displayProperty=nameWithType> 继承。）|  
  
## <a name="methods"></a>方法  
  
|方法|描述|  
|------------|-----------------|  
|`public bool Equals(Object obj)`|确定指定对象是否等于当前对象。  （从 <xref:System.Object> 继承。）|  
|`protected void Finalize()`|在垃圾回收将某一对象回收前允许该对象尝试释放资源并执行其他清理操作。 （从 <xref:System.Object> 继承。）|  
|`public Exception GetBaseException()`|返回是一个或多个后续异常的根本原因的异常。 （从 <xref:System.Exception?displayProperty=nameWithType> 继承。）|  
|`public int GetHashCode()`|为 `MissingInteropDataException` 实例返回一个哈希代码。   （从 <xref:System.Object> 继承。）|  
|`public void GetObjectData(SerializationInfo info, StreamingContext context)`|设置一个包含有关异常信息的 <xref:System.Runtime.Serialization.SerializationInfo> 对象。  （从 <xref:System.Exception?displayProperty=nameWithType> 继承。）|  
|`public Type GetType()`|获取当前实例的运行时类型。 （从 <xref:System.Exception?displayProperty=nameWithType> 继承。）|  
|`protected Object MemberwiseClone()`|创建当前对象的卷影副本。 （从 <xref:System.Object> 继承。）|  
|`public string ToString()`|返回当前异常的字符串表示形式。 （从 <xref:System.Exception?displayProperty=nameWithType> 继承。）|  
  
## <a name="events"></a>事件  
  
|事件|描述|  
|-----------|-----------------|  
|`protected event EventHandler<SafeSerializationEventArgs> SerializeObjectState`|当异常被序列化用来创建包含有关该异常的徐列出数据的异常状态对象时会出现该问题。 （从 <xref:System.Exception?displayProperty=nameWithType> 继承。）|  
  
## <a name="usage-details"></a>使用情况详细信息  

 由于类型信息无效而导致无法成功调用 COM 或 Windows 运行时组件时会引发 `MissingInteropDataException` 异常。  
  
 在运行时可用于应用的元数据由运行时指令定义 (XML 配置) 文件， \*.rd.xml。 为防止应用发生此异常，你必须修改该文件，以定义运行时必须存在的元数据。 通常会通过将 `MarshalObject`、`MarshalDelegate` 或 `MarshalStructure` 属性添加到运行时指令文件中的相应程序元素来解决该错误。 有关本文件的格式信息，请参阅[运行时指令 (rd.xml) 配置文件参考](runtime-directives-rd-xml-configuration-file-reference.md)。  
  
> [!IMPORTANT]
> 由于此异常表示应用程序需要的元数据在运行时间不可用，因此不应在 `try`/`catch` 块中处理此异常。 相反，你应该诊断引起此异常的原因并通过将适当的条目添加到运行时指令文件消除异常。  
  
 `MissingInteropDataException` 类包含单个唯一成员，即 `MissingType` 属性，它表示一种类型，而如果想成功完成方法调用需使用到该类型的元数据。 所有剩余成员均继承自基类 <xref:System.Exception?displayProperty=nameWithType>。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Exception?displayProperty=nameWithType>
- [MissingMetadataException 类](missingmetadataexception-class-net-native.md)
- [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)
