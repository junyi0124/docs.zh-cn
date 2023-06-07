---
title: 适用于 WCF 开发人员的 RPC 类型 - gRPC
description: 审查 WCF 支持的远程过程调用的类型及其在 gRPC 中的等效调用
ms.date: 09/02/2019
ms.openlocfilehash: 40c0779dc015904e9dabbb448075e3c5aa5dc49a
ms.sourcegitcommit: 267d092663aba36b6b2ea853034470aea493bfae
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80111083"
---
# <a name="types-of-rpc"></a>RPC 类型

作为 Windows 通信基础 （WCF） 开发人员，您可能习惯于处理以下类型的远程过程调用 （RPC）：

- 请求/回复
- 双工：
  - 带会话的单向双工
  - 带会话的全双工
- 单向

可以自然地将这些 RPC 类型映射到现有的 gRPC 概念。 本章将依次介绍每个领域。 [第五章](migrate-wcf-to-grpc.md)将更深入地探讨类似的例子。

| WCF | gRPC |
| --- | ---- |
| 定期请求/回复 | 一元 |
| 使用客户端回调接口使用会话的双工服务 | 服务器流式处理 |
| 带会话的全双工服务 | 双向流式处理 |
| 单向操作 | 客户端流式处理 |

## <a name="requestreply"></a>请求/回复

对于获取和返回少量数据的简单请求/回复方法，请使用最简单的 gRPC 模式，即一元 RPC。

```protobuf
service Things {
    rpc Get(GetThingRequest) returns (GetThingResponse);
}
```

```csharp
public class ThingService : Things.ThingsBase
{
    public override async Task<GetThingResponse> Get(GetThingRequest request, ServerCallContext context)
    {
        // Get thing from database
        return new GetThingResponse { Thing = thing };
    }
}
```

```csharp
public async Task ShowThing(int thingId)
{
    var thing = await _thingsClient.GetAsync(new GetThingRequest { ThingId = thingId });
    Console.WriteLine($"{thing.Name}");
}
```

如您所见，实现 gRPC 一元 RPC 服务方法类似于实现 WCF 操作。 区别在于，使用 gRPC，可以重写基类方法，而不是实现接口。 在服务器上，gRPC 基方法始终返回<xref:System.Threading.Tasks.Task%601>，尽管客户端同时提供异步和阻止方法来调用服务。

## <a name="wcf-duplex-one-way-to-client"></a>WCF 双工，客户端的一种方式

WCF 应用程序（具有某些绑定）可以在客户端和服务器之间创建持久连接。 服务器可以使用<xref:System.ServiceModel.ServiceContractAttribute.CallbackContract%2A?displayProperty=nameWithType>属性中指定的*回调接口*将数据异步发送到客户端，直到连接关闭。

gRPC 服务提供与消息流类似的功能。 流在实现方面不会*精确*映射到 WCF 双工服务，但您可以实现相同的结果。

### <a name="grpc-streaming"></a>gRPC 流式处理

gRPC 支持创建从客户端到服务器以及从服务器到客户端的持久流。 这两种类型的流可以同时处于活动状态。 此功能称为双向流式处理。

随着时间的推移，您可以将流用于任意的异步消息传递。 或者，您可以使用它们传递太大的大型数据集，无法生成和发送单个请求或响应。

下面的示例显示了服务器流 RPC。

```protobuf
service ClockStreamer {
    rpc Subscribe(ClockSubscribeRequest) returns (stream ClockMessage);
}
```

```csharp
public class ClockStreamerService : ClockStreamer.ClockStreamerBase
{
    public override async Task Subscribe(ClockSubscribeRequest request,
        IServerStreamWriter<ClockMessage> responseStream,
        ServerCallContext context)
    {
        while (!context.CancellationToken.IsCancellationRequested)
        {
            var time = DateTimeOffset.UtcNow;
            await responseStream.WriteAsync(new ClockMessage { message = $"The time is {time:t}." });
            await Task.Delay(TimeSpan.FromSeconds(10), context.CancellationToken);
        }
    }
}
```

此服务器流可以从客户端应用程序使用，如以下代码所示：

```csharp
public async Task TellTheTimeAsync(CancellationToken token)
{
    var channel = GrpcChannel.ForAddress("https://localhost:5001");
    var client = new ClockStreamer.ClockStreamerClient(channel);

    var request = new ClockSubscribeRequest();
    var response = client.Subscribe(request);

    await foreach (var update in response.ResponseStream.ReadAllAsync(token))
    {
        Console.WriteLine(update.Message);
    }
}
```

> [!NOTE]
> 服务器流式处理中心对于订阅样式的服务很有用。 当在内存中构建整个数据集效率低下或不可能时，它们对于发送大型数据集也很有用。 但是，流式处理响应不如在一条消息中`repeated`发送字段快。 通常，流式处理不应用于小型数据集。

### <a name="differences-from-wcf"></a>与 WCF 的差异

WCF 双工服务使用客户端回调接口，该接口可以有多个方法。 gRPC 服务器流服务只能通过单个流发送消息。 如果需要多种方法，请使用具有[Any 字段或字段之一](protobuf-any-oneof.md)的消息类型发送不同的消息，并在客户端中编写代码来处理它们。

在 WCF 中，具有会话[的服务合同](xref:System.ServiceModel.ServiceContractAttribute)类将保持活动状态，直到连接关闭。 可以在会话中调用多种方法。 在 gRPC`Task`中，在关闭连接之前，实现方法返回的不应完成。

## <a name="wcf-one-way-operations-and-grpc-client-streaming"></a>WCF 单向操作和 gRPC 客户端流

WCF 提供单向操作（标记为`[OperationContract(IsOneWay = true)]`），返回特定于传输的确认。 gRPC 服务方法始终返回响应，即使它为空。 客户端应始终等待该响应。 对于 gRPC 中"即用即用"消息传递样式，您可以创建客户端流服务。

### <a name="thing_logproto"></a>thing_log.proto

```protobuf
service ThingLog {
  rpc OpenConnection(stream Thing) returns (ConnectionClosedResponse);
}
```

### <a name="thinglogservicecs"></a>ThingLogService.cs

```csharp
public class ThingLogService : Protos.ThingLog.ThingLogBase
{
    private static readonly ConnectionClosedResponse EmptyResponse = new ConnectionClosedResponse();
    private readonly ILogger<ThingLogService> _logger;
    public ThingLogService(ILogger<ThingLogService> logger)
    {
        _logger = logger;
    }

    public override async Task<CompletedResponse> OpenConnection(IAsyncStreamReader<Thing> requestStream, ServerCallContext context)
    {
        while (await requestStream.MoveNext(context.CancellationToken))
        {
            _logger.LogInformation(requestStream.Current.Description);
        }
        return EmptyResponse;
    }
}
```

### <a name="thinglog-client-example"></a>ThingLog 客户端示例

```csharp
public class ThingLogger : IAsyncDisposable
{
    private readonly ThingLog.ThingLogClient _client;
    private readonly AsyncClientStreamingCall<ThingLogRequest, CompletedResponse> _stream;

    public ThingLogger(ThingLog.ThingLogClient client)
    {
        _client = client;
        _stream = client.OpenConnection();
    }

    public async Task WriteAsync(string description)
    {
        await _stream.RequestStream.WriteAsync(new Thing
        {
            Description = description,
            Time = Timestamp.FromDateTimeOffset(DateTimeOffset.UtcNow)
        });
    }

    public async ValueTask DisposeAsync()
    {
        await _stream.RequestStream.CompleteAsync();
        _stream.Dispose();
    }
}
```

您可以使用客户端流 RPC 进行触发和忘记消息传递，如上例所示。 您还可以使用它们向服务器发送非常大的数据集。 相同的性能警告适用：对于较小的数据集，在`repeated`常规消息中使用字段。

## <a name="wcf-full-duplex-services"></a>WCF 全双工服务

WCF 双工绑定支持在服务接口和客户端回调接口上执行多个单向操作。 这种支持允许客户端和服务器之间持续对话。 gRPC 支持与双向流式处理 RPC 类似的内容，其中两个参数`stream`都用修饰符标记。

### <a name="chatproto"></a>聊天.proto

```protobuf
service Chatter {
    rpc Connect(stream IncomingMessage) returns (stream OutgoingMessage);
}
```

### <a name="chatterservicecs"></a>ChatterService.cs

```csharp
public class ChatterService : Chatter.ChatterBase
{
    private readonly IChatHub _hub;

    public ChatterService(IChatHub hub)
    {
        _hub = hub;
    }

    public override async Task Connect(IAsyncStreamReader<MessageRequest> requestStream, IServerStreamWriter<MessageResponse> responseStream, ServerCallContext context)
    {
        _hub.MessageReceived += async (sender, args) =>
            await responseStream.WriteAsync(new MessageResponse {Text = args.Message});

        while (await requestStream.MoveNext(context.CancellationToken))
        {
            await _hub.SendAsync(requestStream.Current.Text);
        }
    }
}
```

在前面的示例中，您可以看到实现方法同时接收请求流 （`IAsyncStreamReader<MessageRequest>`） 和响应流 （`IServerStreamWriter<MessageResponse>`。 该方法可以读取和写入消息，直到连接关闭。

### <a name="chatter-client"></a>聊天客户端

```csharp
public class Chat : IAsyncDisposable
{
    private readonly Chatter.ChatterClient _client;
    private readonly AsyncDuplexStreamingCall<MessageRequest, MessageResponse> _stream;
    private readonly CancellationTokenSource _cancellationTokenSource;
    private readonly Task _readTask;

    public Chat(Chatter.ChatterClient client)
    {
        _client = client;
        _stream = _client.Connect();
        _cancellationTokenSource = new CancellationTokenSource();
        _readTask = ReadAsync(_cancellationTokenSource.Token);
    }

    public async Task SendAsync(string message)
    {
        await _stream.RequestStream.WriteAsync(new MessageRequest {Text = message});
    }

    private async Task ReadAsync(CancellationToken token)
    {
        while (await _stream.ResponseStream.MoveNext(token))
        {
            Console.WriteLine(_stream.ResponseStream.Current.Text);
        }
    }

    public async ValueTask DisposeAsync()
    {
        await _stream.RequestStream.CompleteAsync();
        await _readTask;
        _stream.Dispose();
    }
}
```

>[!div class="step-by-step"]
>[上一个](wcf-bindings.md)
>[下一个](metadata.md)
