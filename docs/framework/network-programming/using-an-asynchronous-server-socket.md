---
title: 使用异步服务器套接字
description: 此示例演示异步服务器套接字。 Socket 类使用 .NET Framework 异步编程处理网络服务请求。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- application protocols, sockets
- sending data, sockets
- Socket class, asynchronous server sockets
- data requests, sockets
- sockets, asynchronous server sockets
- requesting data from Internet, sockets
- server sockets
- receiving data, sockets
- asynchronous server sockets
- protocols, sockets
- Internet, sockets
ms.assetid: 813489a9-3efd-41b6-a33f-371d55397676
ms.openlocfilehash: 6800a1973d9eb65bbb7520f9db7a8f431656c685
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96265201"
---
# <a name="using-an-asynchronous-server-socket"></a>使用异步服务器套接字

异步服务器套接字使用 .NET Framework 异步编程模型处理网络服务请求。 <xref:System.Net.Sockets.Socket> 类遵循标准 .NET Framework 异步命名模式；例如，同步 <xref:System.Net.Sockets.Socket.Accept%2A> 方法对应于异步 <xref:System.Net.Sockets.Socket.BeginAccept%2A> 和 <xref:System.Net.Sockets.Socket.EndAccept%2A> 方法。  
  
 异步服务器套接字需要一个开始接受网络连接请求的方法、一个处理连接请求并开始接收网络数据的回调方法，以及一个结束接收数据的回调方法。 本部分将进一步讨论所有这些方法。  
  
 在下面的示例中，要开始接受来自网络的连接请求，`StartListening` 方法会初始化 Socket ，然后使用 BeginAccept 方法开始接受新的连接。  当套接字上接收到新的连接请求时，将调用接受回调方法。 它负责获取将要处理连接的 Socket 实例，并将该 Socket 提交给将处理请求的线程。  接受回调方法实现 <xref:System.AsyncCallback> 委托；它返回 void，并取一个 <xref:System.IAsyncResult> 类型的参数。 下面的示例是接受回调方法的 shell。  
  
```vb  
Sub AcceptCallback(ar As IAsyncResult)  
    ' Add the callback code here.  
End Sub 'AcceptCallback  
```  
  
```csharp  
void AcceptCallback(IAsyncResult ar)
{  
    // Add the callback code here.  
}  
```  
  
 BeginAccept 方法取两个参数：一个指向接受回调方法的 AsyncCallback 委托和一个用于将状态信息传递给回调方法的对象。  在下面的示例中，侦听 Socket 通过 state 参数传递给回调方法。 此示例会创建一个 AsyncCallback 委托并开始接受来自网络的连接。  
  
```vb  
listener.BeginAccept( _  
    New AsyncCallback(SocketListener.AcceptCallback),_  
    listener)  
```  
  
```csharp  
listener.BeginAccept(new AsyncCallback(SocketListener.AcceptCallback), listener);  
```  
  
 异步套接字使用系统线程池中的线程处理传入的连接。 一个线程负责接受连接，另一个线程则用于处理每个传入的连接，还有一个线程负责接收来自连接的数据。 这些线程可以是同一个线程，具体取决于线程池分配了哪一个线程。 在下面的示例中，<xref:System.Threading.ManualResetEvent?displayProperty=nameWithType> 类将挂起主线程的执行并在执行可以继续时发出信号。  
  
 下面的示例演示在本地计算机上创建异步 TCP/IP 套接字并开始接受连接的异步方法。 它假定存在一个名为 `allDone` 的全局 ManualResetEvent，该方法是名为 `SocketListener` 的类的成员，并且假定定义了一个名为 `AcceptCallback` 的回调方法。  
  
```vb  
Public Sub StartListening()  
    Dim ipHostInfo As IPHostEntry = Dns.Resolve(Dns.GetHostName())  
    Dim localEP = New IPEndPoint(ipHostInfo.AddressList(0), 11000)  
  
    Console.WriteLine($"Local address and port : {localEP.ToString()}")  
  
    Dim listener As New Socket(localEP.Address.AddressFamily, _  
       SocketType.Stream, ProtocolType.Tcp)  
  
    Try  
        listener.Bind(localEP)  
        listener.Listen(10)  
  
        While True  
            allDone.Reset()  
  
            Console.WriteLine("Waiting for a connection...")  
            listener.BeginAccept(New _  
                AsyncCallback(SocketListener.AcceptCallback), _  
                listener)  
  
            allDone.WaitOne()  
        End While  
    Catch e As Exception  
        Console.WriteLine(e.ToString())  
    End Try  
    Console.WriteLine("Closing the listener...")  
End Sub 'StartListening  
```  
  
```csharp  
public void StartListening()
{  
    IPHostEntry ipHostInfo = Dns.Resolve(Dns.GetHostName());  
    IPEndPoint localEP = new IPEndPoint(ipHostInfo.AddressList[0], 11000);  
  
    Console.WriteLine($"Local address and port : {localEP.ToString()}");  
  
    Socket listener = new Socket(localEP.Address.AddressFamily, SocketType.Stream, ProtocolType.Tcp);  
  
    try
    {  
        listener.Bind(localEP);  
        listener.Listen(10);  
  
        while (true)
        {  
            allDone.Reset();  
  
            Console.WriteLine("Waiting for a connection...");  
            listener.BeginAccept(new AsyncCallback(SocketListener.AcceptCallback), listener);  
  
            allDone.WaitOne();  
        }  
    }
    catch (Exception e)
    {  
        Console.WriteLine(e.ToString());  
    }  
  
    Console.WriteLine("Closing the listener...");  
}  
```  
  
 接受回调方法（即前例中的 `AcceptCallback`）负责向主应用程序线程发出信号，使其继续处理、建立与客户端的连接并开始异步读取客户端数据。 下面的示例是 `AcceptCallback` 方法的实现的第一部分。 这部分方法向主应用程序线程发出信号，使其继续处理并建立与客户端的连接。 它假定一个名为 `allDone` 的全局 ManualResetEvent。  
  
```vb  
Public Sub AcceptCallback(ar As IAsyncResult)  
    allDone.Set()  
  
    Dim listener As Socket = CType(ar.AsyncState, Socket)  
    Dim handler As Socket = listener.EndAccept(ar)  
  
    ' Additional code to read data goes here.  
End Sub 'AcceptCallback  
```  
  
```csharp  
public void AcceptCallback(IAsyncResult ar)
{  
    allDone.Set();  
  
    Socket listener = (Socket) ar.AsyncState;  
    Socket handler = listener.EndAccept(ar);  
  
    // Additional code to read data goes here.
}  
```  
  
 从客户端套接字读取数据需要一个在异步调用之间传递值的状态对象。 以下示例实现一个用于从远程客户端接收字符串的状态对象。 它包含以下各项的字段：客户端套接字、用于接收数据的数据缓冲区，和用于创建客户端发送的数据字符串的 <xref:System.Text.StringBuilder>。 将这些字段放入该状态对象中，使这些字段的值在多个调用之间得以保留，以便从客户端套接字读取数据。  
  
```vb  
Public Class StateObject  
    Public workSocket As Socket = Nothing  
    Public BufferSize As Integer = 1024  
    Public buffer(BufferSize) As Byte  
    Public sb As New StringBuilder()  
End Class 'StateObject  
```  
  
```csharp  
public class StateObject
{  
    public Socket workSocket = null;  
    public const int BufferSize = 1024;  
    public byte[] buffer = new byte[BufferSize];  
    public StringBuilder sb = new StringBuilder();  
}  
```  
  
 `AcceptCallback` 方法的这部分（即开始从客户端套接字接收数据的部分）首先初始化 `StateObject` 类的实例，然后调用 <xref:System.Net.Sockets.Socket.BeginReceive%2A> 方法开始从客户端套接字异步读取数据。  
  
 以下示例显示完整的 `AcceptCallback` 方法。 它假定存在一个名为 `allDone,` 且定义了 `StateObject` 类的全局 ManualResetEvent，并且假定在名为 `SocketListener` 的类中定义了 `ReadCallback` 方法。  
  
```vb  
Public Shared Sub AcceptCallback(ar As IAsyncResult)  
    ' Get the socket that handles the client request.  
    Dim listener As Socket = CType(ar.AsyncState, Socket)  
    Dim handler As Socket = listener.EndAccept(ar)  
  
    ' Signal the main thread to continue.  
    allDone.Set()  
  
    ' Create the state object.  
    Dim state As New StateObject()  
    state.workSocket = handler  
    handler.BeginReceive(state.buffer, 0, state.BufferSize, 0, _  
        AddressOf AsynchronousSocketListener.ReadCallback, state)  
End Sub 'AcceptCallback  
```  
  
```csharp  
public static void AcceptCallback(IAsyncResult ar)
{  
    // Get the socket that handles the client request.  
    Socket listener = (Socket) ar.AsyncState;  
    Socket handler = listener.EndAccept(ar);  
  
    // Signal the main thread to continue.  
    allDone.Set();  
  
    // Create the state object.  
    StateObject state = new StateObject();  
    state.workSocket = handler;  
    handler.BeginReceive(state.buffer, 0, StateObject.BufferSize, 0,  
        new AsyncCallback(AsynchronousSocketListener.ReadCallback), state);  
}  
```  
  
 需要为异步套接字服务器实现的最终方法是返回客户端发送的数据的读取回调方法。 与接受回调方法一样，读取回调方法也是 AsyncCallback 委托。 此方法将来自客户端套接字的一个或多个字节读入数据缓冲区，然后再次调用 BeginReceive 方法，直到客户端完成数据发送为止。 从客户端读取了整个消息后，将在控制台上显示字符串，且会关闭处理客户端连接的服务器套接字。  
  
 下面的示例实现 `ReadCallback` 方法。 它假定定义了 `StateObject` 类。  
  
```vb  
Public Shared Sub ReadCallback(ar As IAsyncResult)  
    Dim state As StateObject = CType(ar.AsyncState, StateObject)  
    Dim handler As Socket = state.workSocket  
  
    ' Read data from the client socket.
    Dim read As Integer = handler.EndReceive(ar)  
  
    ' Data was read from the client socket.  
    If read > 0 Then  
        state.sb.Append(Encoding.ASCII.GetString(state.buffer, 0, read))  
        handler.BeginReceive(state.buffer, 0, state.BufferSize, 0, _  
            AddressOf ReadCallback, state)  
    Else  
        If state.sb.Length > 1 Then  
            ' All the data has been read from the client;  
            ' display it on the console.  
            Dim content As String = state.sb.ToString()  
            Console.WriteLine($"Read {content.Length} bytes from socket. {ControlChars.Cr} Data : {content}")  
        End If  
    End If  
End Sub 'ReadCallback  
```  
  
```csharp  
public static void ReadCallback(IAsyncResult ar)
{  
    StateObject state = (StateObject) ar.AsyncState;  
    Socket handler = state.WorkSocket;  
  
    // Read data from the client socket.  
    int read = handler.EndReceive(ar);  
  
    // Data was read from the client socket.  
    if (read > 0)
    {  
        state.sb.Append(Encoding.ASCII.GetString(state.buffer,0,read));  
        handler.BeginReceive(state.buffer, 0, StateObject.BufferSize, 0,  
            new AsyncCallback(ReadCallback), state);  
    }
    else
    {  
        if (state.sb.Length > 1)
        {  
            // All the data has been read from the client;  
            // display it on the console.  
            string content = state.sb.ToString();  
            Console.WriteLine($"Read {content.Length} bytes from socket.\n Data : {content}");
        }  
        handler.Close();  
    }  
}  
```  
  
## <a name="see-also"></a>请参阅

- [使用同步服务器套接字](using-a-synchronous-server-socket.md)
- [异步服务器套接字示例](asynchronous-server-socket-example.md)
- [线程处理](../../standard/threading/index.md)
- [使用套接字侦听](listening-with-sockets.md)
