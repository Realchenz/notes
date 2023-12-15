1. BIO
 `server`

   (1) 通过ServerSocket注册端口
```java
ServerSocket serverSocket = null;
		try {
			// 服务器监听
			serverSocket = new ServerSocket(port);
			System.out.println(
					"BlockingEchoServer已启动，端口：" + port);
			
		} catch (IOException e) {
			System.out.println(
					"BlockingEchoServer启动异常，端口：" + port);
			System.out.println(e.getMessage());
		}
```
   (2) 通过ServerSocket.accept()阻塞等待客户端连接
```java
try (
				// 接受客户端建立链接，生成Socket实例
				Socket clientSocket = serverSocket.accept();
				PrintWriter out = 
						new PrintWriter(clientSocket.getOutputStream(), true);
				
				// 接收客户端的信息
				BufferedReader in = 
						new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));) {
			String inputLine;
			while ((inputLine = in.readLine()) != null) {
				
				// 发送信息给客户端
				out.println(inputLine);
				System.out.println(
						"BlockingEchoServer -> " + clientSocket.getRemoteSocketAddress() + ":" + inputLine);
			}
		} catch (IOException e) {
			System.out.println(
					"BlockingEchoServer异常!" + e.getMessage());
		}
```
   (3) 通过Socket.getInputStream()获取客户端发送的数据
   (4) 通过Socket.getOutputStream()发送数据给客户端
   (5) 通过Socket.close()关闭连接

`client`
    (1) 通过Socket连接服务器
```java
try (
            Socket echoSocket = new Socket(hostName, portNumber);
            PrintWriter out = // 输出流
                new PrintWriter(echoSocket.getOutputStream(), true);
            BufferedReader in = // 输入流
                new BufferedReader(
                    new InputStreamReader(echoSocket.getInputStream()));
            BufferedReader stdIn = // 标准输入流
                new BufferedReader(
                    new InputStreamReader(System.in))
        ) {
            String userInput;
            while ((userInput = stdIn.readLine()) != null) {
                out.println(userInput);
                System.out.println("echo: " + in.readLine());
            }
        } catch (UnknownHostException e) {
            System.err.println("不明主机，主机名为： " + hostName);
            System.exit(1);
        } catch (IOException e) {
            System.err.println("不能从主机中获取I/O，主机名为：" +
                hostName);
            System.exit(1);
        } 
```
2. 伪异步IO
3. NIO
   (1) 通过ServerSocketChannel.open()打开ServerSocketChannel
```java
ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
```
   (2) 通过ServerSocketChannel.bind()绑定端口
```java
serverSocketChannel.bind(new InetSocketAddress(port));
```
   (3) 通过ServerSocketChannel.configureBlocking(false)设置非阻塞模式
```java
serverSocketChannel.configureBlocking(false);
```
   (4) 通过Selector.open()打开Selector
```java
Selector selector = Selector.open();
```
   (5) 通过ServerSocketChannel.register()注册到Selector
```java
serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);
```
   (6) 通过Selector.select()等待客户端连接
```java
selector.select();
```
   (7) 通过Selector.selectedKeys()获取就绪事件集合
```java
Set<SelectionKey> selectedKeys = selector.selectedKeys();
```
   (8) 通过Iterator迭代器遍历就绪事件集合
```java
Iterator<SelectionKey> keyIterator = selectedKeys.iterator();
```
   (9) 通过SelectionKey.isAcceptable()判断是否是接入事件
```java
if (key.isAcceptable()) {
    // a connection was accepted by a ServerSocketChannel.
} else if (key.isConnectable()) {
    // a connection was established with a remote server.
} else if (key.isReadable()) {
    // a channel is ready for reading
} else if (key.isWritable()) {
    // a channel is ready for writing
}
```
   (10) 通过ServerSocketChannel.accept()获取SocketChannel
```java
SocketChannel socketChannel = serverSocketChannel.accept();
```
   (11) 通过SocketChannel.read()读取请求码流
```java
ByteBuffer readBuffer = ByteBuffer.allocate(1024);
socketChannel.read(readBuffer);
```
   (12) 通过SocketChannel.write()写入响应码流
```java
ByteBuffer writeBuffer = ByteBuffer.allocate(1024);
writeBuffer.put("received".getBytes());
writeBuffer.flip();
socketChannel.write(writeBuffer);
```
   (13) 通过SocketChannel.close()关闭连接
```java
socketChannel.close();
```
4. AIO
   (1) 通过AsynchronousServerSocketChannel.open()打开AsynchronousServerSocketChannel
```java