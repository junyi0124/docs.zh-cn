---
title: UriTemplate 和 UriTemplateTable
ms.date: 03/30/2017
ms.assetid: 5cbbe03f-4a9e-4d44-9e02-c5773239cf52
ms.openlocfilehash: 212cc0a7f90ac2e74837118a905519148ddc089d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289667"
---
# <a name="uritemplate-and-uritemplatetable"></a>UriTemplate 和 UriTemplateTable

Web 开发人员需要能够描述其服务所响应的 URI 的形状和布局。 Windows Communication Foundation (WCF) 添加了两个新类，使开发人员能够控制其 Uri。 <xref:System.UriTemplate> 和 <xref:System.UriTemplateTable> 构成 WCF 中基于 URI 的调度引擎的基础。 还可以自行使用这些类，使开发人员可以利用模板和 URI 映射机制，而无需实现 WCF 服务。  
  
## <a name="templates"></a>模板  

 模板是一种描述一组相对 URI 的方法。 下表中的一组 URI 模板演示如何定义一个检索各类天气信息的系统。  
  
|数据|模板|  
|----------|--------------|  
|全国预报|weather/national|  
|州预报|weather/{state}|  
|城市预报|weather/{state}/{city}|  
|活动预报|weather/{state}/{city}/{activity}|  
  
 上表描述了一组结构相似的 URI。 每一项都是一个 URI 模板。 大括号中的各段描述变量； 大括号之外的各段描述文本字符串。 利用 WCF 模板类，开发人员可以获取传入 URI，例如 "/weather/wa/seattle/cycling"，并将其与描述它的模板 "/weather/{state}/{city}/{activity}" 匹配。  
  
## <a name="uritemplate"></a>UriTemplate  

 <xref:System.UriTemplate> 是包装 URI 模板的类。 其构造函数接受一个定义模板的字符串参数。 此字符串包含具有下节所述格式的模板。 <xref:System.UriTemplate> 类提供一些方法，用于将传入的 URI 与模板进行匹配，根据模板生成 URI，检索在模板中使用的变量名集合，确定两个模板是否等效，返回模板的字符串。  
  
 <xref:System.UriTemplate.Match%28System.Uri%2CSystem.Uri%29> 接受一个基址和一个候选 URI，并尝试将该 URI 与模板进行匹配。 如果匹配成功，则返回一个 <xref:System.UriTemplateMatch> 实例。 <xref:System.UriTemplateMatch> 对象包含一个基准 URI、候选 URI、查询参数的名称/值集合、相对路径段数组、匹配变量的名称/值集合、用于执行匹配操作的 <xref:System.UriTemplate> 实例、包含候选 URI 中任意不匹配部分的字符串（在模板有通配符时使用），以及一个与模板关联的对象。  
  
> [!NOTE]
> 将候选 URI 与模板进行匹配时，<xref:System.UriTemplate> 类将忽略方案和端口号。  
  
 从模板生成 URI 的方法有两个，即 <xref:System.UriTemplate.BindByName%28System.Uri%2CSystem.Collections.Specialized.NameValueCollection%29> 和 <xref:System.UriTemplate.BindByPosition%28System.Uri%2CSystem.String%5B%5D%29>。 <xref:System.UriTemplate.BindByName%28System.Uri%2CSystem.Collections.Specialized.NameValueCollection%29> 接受一个基址和一个参数的名称/值集合。 模板绑定后，这些参数将替换变量。 <xref:System.UriTemplate.BindByPosition%28System.Uri%2CSystem.String%5B%5D%29> 接受名称/值对，并从左向右替换他们。  
  
 <xref:System.UriTemplate.ToString> 返回模板字符串。  
  
 <xref:System.UriTemplate.PathSegmentVariableNames%2A> 属性包含变量名称的集合，这些变量就是模板字符串的路径段中所使用的变量。  
  
 <xref:System.UriTemplate.IsEquivalentTo%28System.UriTemplate%29> 接受一个 <xref:System.UriTemplate> 作为参数，并返回一个布尔值，该值指定两个模板是否等效。 有关详细信息，请参阅本主题后面的模板等效性部分。  
  
 <xref:System.UriTemplate> 旨在处理符合 HTTP URI 语法的任意 URI 方案。 下面这些示例都是支持的 URI 方案。  
  
- `http://`  
  
- `https://`  
  
- `net.tcp://`  
  
- `net.pipe://`  
  
- `sb://`  
  
 诸如 file:// 和 urn:// 这样的方案不符合 HTTP URI 语法，它们与 URI 模板一起使用时，将导致不可预知的结果。  
  
### <a name="template-string-syntax"></a>模板字符串语法  

 模板分为三个部分：路径、可选查询和可选片段。 有关示例，请参见下面的模板：  
  
`"/weather/{state}/{city}?forecast={length)#frag1`  
  
 路径由“/weather/{state}/{city}”构成，查询由“?forecast={length}”构成，片段由“#frag1”构成。  
  
 在路径表达式中，首尾斜杠是可选的。 查询表达式和片段表达式可完全省略。 路径由一系列用 "/" 分隔的段组成，每个段可以有一个文本值、一个 (用 {大括号} ) 编写的变量名，或者是以 "" ) 编写的通配符 (\* 。 在上一模板中，“\weather\”段为文本值，而“{state}”和“{city}”为变量。 变量从其大括号的内容中提取其名称，以后可将其替换为具体的值以创建 *已关闭的 URI*。 通配符是可选的，但只能出现在 URI 的末尾，它在逻辑上与 "路径的其余部分" 匹配。  
  
 查询表达式（如果存在）指定了一系列由 "&" 分隔的无序名称/值对。 查询表达式的元素可以是文本对 (x=2) 或变量对 (x={var})。 只有查询的右侧可以有变量表达式。 不允许 {someName} = {someValue}。 不允许使用不成对的值 (?x)。 空查询表达式与只包含一个 "？" 的查询表达式之间没有任何差别 (两个都表示 "any query" ) 。  
  
 片段表达式可以由文本值构成，不允许使用任何变量。  
  
 模板字符串内的所有模板变量名都必须是唯一的。 模板变量名不区分大小写。  
  
 有效模板字符串的示例：  
  
- ""  
  
- "/shoe"  
  
- "/shoe/ \* "  
  
- "{shoe}/boat"  
  
- "{鞋}/{boat}/bed/{quilt}"  
  
- "鞋/{船}"  
  
- "鞋/{船}/ \* "  
  
- "鞋/船？ x = 2"  
  
- "鞋/{船}？ x = {床}"  
  
- "鞋/{船}？ x = {床} &y = 波段"  
  
- "？ x = {鞋}"  
  
- "鞋单 = 3&y = {var}  
  
 无效模板字符串的示例：  
  
- "{鞋}/{SHOE}/x = 2" –重复变量名。  
  
- "{鞋}/boat/？床 = {鞋}" –重复的变量名称。  
  
- "？ x = 2&x = 3" –查询字符串内的名称/值对必须是唯一的（即使它们是文本）。  
  
- "？ x = 2&" –查询字符串格式不正确。  
  
- "？ 2&x = {鞋}" –查询字符串必须是名称/值对。  
  
- "？ y = 2&&X = 3" –查询字符串必须是名称值对，名称不能以 "&" 开头。  
  
### <a name="compound-path-segments"></a>复合路径段  

 复合路径段允许单个 URI 路径段包含多个变量，以及组合有文本的变量。 下面这些示例都是有效的复合路径段：  
  
- /filename.{ext}/  
  
- /{filename}.jpg/  
  
- /{filename}.{ext}/  
  
- /{a}.{b}someLiteral{c}({d})/  
  
 下面这些示例都是无效的复合路径段：  
  
- / {} -变量必须命名为。  
  
- /{shoe}{boat} - 必须用文本分隔变量。  
  
### <a name="matching-and-compound-path-segments"></a>匹配和复合路径段  

 复合路径段允许定义在单个路径段内具有多个变量的 UriTemplate。 例如，在下面的模板字符串中： "Addresses/{state}"。{city} " (州和市县) 的两个变量是在同一段中定义的。 此模板将匹配 URL （如）， `http://example.com/Washington.Redmond` 但它还会匹配 url，如 `http://example.com/Washington.Redmond.Microsoft` 。 对于后一种情况，状态变量将包含 "华盛顿" 并且 city 变量将包含 "Redmond. Microsoft"。 这时，任何文本（“/”除外）都将与 {city} 变量相匹配。 如果需要一个与 "额外" 文本不匹配的模板，请将该变量置于单独的模板段中，例如： "Addresses/{state}/{city}"。  
  
### <a name="named-wildcard-segments"></a>命名通配符段  

 命名的通配符段是其变量名称以通配符 "" 开头的任何路径变量段 \* 。 下面的模板字符串包含一个名为“shoe”的命名通配符段。  
  
`"literal/{*shoe}"`  
  
 通配符段必须遵循以下规则：  
  
- 每个模板字符串最多只能有一个命名通配符段。  
  
- 命名通配符段必须位于路径中的最右段。  
  
- 命名通配符段不能与匿名通配符段共存于同一模板字符串中。  
  
- 命名通配符段的名称必须是唯一的。  
  
- 命名通配符段不能具有默认值。  
  
- 命名通配符段不能以 "/" 结尾。  
  
### <a name="default-variable-values"></a>默认变量值  

 通过默认变量值，可以在模板中指定变量的默认值。 默认变量可以用声明变量的大括号来指定，也可以指定为传递到 UriTemplate 构造函数的集合。 下面的模板演示用具有默认值的变量来指定 <xref:System.UriTemplate> 的两种方法。  
  
```csharp
UriTemplate t = new UriTemplate("/test/{a=1}/{b=5}");  
```  
  
 此模板声明一个名为 `a`、默认值为 `1` 的变量和一个名为 `b`、默认值为 `5` 的变量。  
  
> [!NOTE]
> 仅允许路径段变量具有默认值。 不允许查询字符串变量、复合段变量和命名通配符变量具有默认值。  
  
 下面的代码演示在匹配候选 URI 时，如何处理默认变量值。  
  
```csharp
Uri baseAddress = new Uri("http://localhost:8000/");

UriTemplate t = new UriTemplate("/{state=WA}/{city=Redmond}/", true);
Uri candidate = new Uri("http://localhost:8000/OR");

UriTemplateMatch m1 = t.Match(baseAddress, candidate);

Console.WriteLine($"Template: {t}");
Console.WriteLine($"Candidate URI: {candidate}");

// Display contents of BoundVariables
Console.WriteLine("BoundVariables:");
foreach (string key in m1.BoundVariables.AllKeys)
{
    Console.WriteLine($"\t{key}={m1.BoundVariables[key]}");
}
// The output of the above code is  
// Template: /{state=WA}/{city=Redmond}/
// Candidate URI: http://localhost:8000/OR
// BoundVariables:
//         STATE=OR
//         CITY=Redmond
```  
  
> [!NOTE]
> 诸如这样的 uri 与 `http://localhost:8000///` 前面代码中列出的模板不匹配 `http://localhost:8000/` 。  
  
 下面的代码演示在使用模板创建 URI 时，如何处理默认变量值。  
  
```csharp
Uri baseAddress = new Uri("http://localhost:8000/");  
Dictionary<string,string> defVals = new Dictionary<string,string> {{"a","1"}, {"b", "5"}};  
UriTemplate t = new UriTemplate("/test/{a}/{b}", defVals);  
NameValueCollection vals = new NameValueCollection();  
vals.Add("a", "10");  
  
Uri boundUri = t.BindByName(baseAddress, vals);  
Console.WriteLine("BaseAddress: {0}", baseAddress);  
Console.WriteLine("Template: {0}", t.ToString());  
  
Console.WriteLine("Values: ");  
foreach (string key in vals.AllKeys)  
{  
    Console.WriteLine("\tKey = {0}, Value = {1}", key, vals[key]);  
}  
Console.WriteLine("Bound URI: {0}", boundUri);  
  
// The output of the preceding code is  
// BaseAddress: http://localhost:8000/  
// Template: /test/{a}/{b}  
// Values:  
//     Key = a, Value = 10  
// Bound URI: http://localhost:8000/test/10/5  
```  
  
为变量提供默认值 `null` 时，有一些其他约束。 如果某个变量包含在模板字符串的最右段中，或者该段右侧的所有段都具有默认值 `null`，则该变量可以具有默认值 `null`。 下面是带有 `null` 默认值的有效模板字符串：  
  
- `UriTemplate t = new UriTemplate("shoe/{boat=null}");`

- `UriTemplate t = new UriTemplate("{shoe=null}/{boat=null}");`
  
- `UriTemplate t = new UriTemplate("{shoe=1}/{boat=null}");`

 以下是无效的模板字符串，其默认值为 `null` ：  
  
- `UriTemplate t = new UriTemplate("{shoe=null}/boat"); // null default must be in the right most path segment`
  
- `UriTemplate t = new UriTemplate("{shoe=null}/{boat=x}/{bed=null}"); // shoe cannot have a null default because boat does not have a default null value`

### <a name="default-values-and-matching"></a>默认值和匹配  

 将候选 URI 与带有默认值的模板进行匹配时，如果候选 URI 中未指定值，则将在 <xref:System.UriTemplateMatch.BoundVariables%2A> 集合中放置默认值。  
  
### <a name="template-equivalence"></a>模板等效性  

 当所有模板的文本都匹配并且它们在同一段中具有变量时，两个模板被认为是 *结构等效* 的。 例如，以下模板是结构等效的：  
  
- /a/{var1}/b b/{var2}？ x = 1&y = 2  
  
- a/{x}/b% 20b/{var1}？ y = 2&x = 1  
  
- a/{y}/B% 20B/{z}/？ y = 2&x = 1  
  
 在这里需注意一些事项：  
  
- 如果模板包含前导斜杠，则只忽略第一个。  
  
- 比较模板字符串以确定结构等效性时，变量名和路径段不区分大小写，查询字符串区分大小写。  
  
- 查询字符串不分次序。  
  
## <a name="uritemplatetable"></a>UriTemplateTable  

 <xref:System.UriTemplateTable> 类表示一个绑定到开发人员所选对象的 <xref:System.UriTemplate> 对象的关联表。 调用 <xref:System.UriTemplateTable> 之前，<xref:System.UriTemplate> 必须至少包含一个 <xref:System.UriTemplateTable.MakeReadOnly%28System.Boolean%29>。 在调用 <xref:System.UriTemplateTable> 之前，可以更改 <xref:System.UriTemplateTable.MakeReadOnly%28System.Boolean%29> 的内容。 调用 <xref:System.UriTemplateTable.MakeReadOnly%28System.Boolean%29> 时，会执行验证。 执行的验证的类型取决于传给 `allowMultiple` 的 <xref:System.UriTemplateTable.MakeReadOnly%28System.Boolean%29> 参数的值。  
  
 如果在调用 <xref:System.UriTemplateTable.MakeReadOnly%28System.Boolean%29> 时传入 `false`，则 <xref:System.UriTemplateTable> 将进行检查以确保表中无模板。 如果找到任何结构等效的模板，则引发异常。 如果要确保只有一个模板与传入的 URI 匹配，可将此方法与 <xref:System.UriTemplateTable.MatchSingle%28System.Uri%29> 结合使用。  
  
 如果在调用 <xref:System.UriTemplateTable.MakeReadOnly%28System.Boolean%29> 时传入 `true`，则 <xref:System.UriTemplateTable> 允许 <xref:System.UriTemplateTable> 中包含多个结构等效的模板。  
  
 如果添加到 <xref:System.UriTemplate> 中的一组 <xref:System.UriTemplateTable> 对象包含查询字符串，则这些字符串不得有歧义。 可以使用相同的查询字符串。  
  
> [!NOTE]
> 尽管 <xref:System.UriTemplateTable> 允许使用除 HTTP 之外的方案的基址，但是将候选 URI 与模板匹配时，将忽略方案和端口号。  
  
### <a name="query-string-ambiguity"></a>查询字符串多义性  

 如果某个 URI 匹配多个模板，则共享等效路径的模板将包含歧义查询字符串。  
  
 以下几组查询字符串在其自身范围内无歧义：  
  
- ?x=1  
  
- ?x=2  
  
- ?x=3  
  
- ？ x = 1&y = {var}  
  
- ？ x = 2&z = {var}  
  
- ?x=3  
  
- ?x=1  
  
- ?  
  
- ? x={var}  
  
- ?  
  
- ？ m = get&c = rss  
  
- ？ m = put&c = rss  
  
- ？ m = get&c = atom  
  
- ？ m = put&c = atom  
  
 以下几组查询字符串模板在其自身范围内有歧义：  
  
- ?x=1  
  
- ?x={var}  
  
 “x=1”- 与这两个模板都匹配。  
  
- ?x=1  
  
- ?y=2  
  
 "x = 1&y = 2" 匹配两个模板。 这是因为查询字符串包含的查询字符串变量可能比与之匹配的模板多。  
  
- ?x=1  
  
- ？ x = 1&y = {var}  
  
 "x = 1&y = 3" 匹配两个模板。  
  
- ？ x = 3&y = 4  
  
- ？ x = 3&z = 5  
  
> [!NOTE]
> 作为 URI 路径或 <xref:System.UriTemplate> 路径段文本的组成部分时，字符 á 和 Á 视为不同的字符（但字符 a 和 A 视为相同的字符）。 如果字符作为 {variableName} 或查询字符串的一部分出现，则将其视为相同的字符 <xref:System.UriTemplate> ，并且 (和也被视为相同的字符) 。  
  
## <a name="see-also"></a>另请参阅

- [WCF Web HTTP 编程模型概述](wcf-web-http-programming-model-overview.md)
- [WCF Web HTTP 编程对象模型](wcf-web-http-programming-object-model.md)
- [UriTemplate](../samples/uritemplate-sample.md)
- [UriTemplate 表](../samples/uritemplate-table-sample.md)
- [UriTemplate 表调度程序](../samples/uritemplate-table-dispatcher-sample.md)
