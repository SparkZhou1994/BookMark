# Netty——异步和事件驱动
## Netty的核心组件
### Channel
传入或传出数据的载体，打开、关闭、连接、断开
### 回调
方法引用
当回调触发时，被interfaceChannelHandler的实现处理，如实现ChannelInboundHandlerAdapter的channelActive方法
### Future
ChannelFuture提供了额外方法，可以注册一个或者多个ChannelFutureListener实例。
```
private static final Channel CHANNEL_FROM_SOMEWHERE = new NioSocketChannel();

/**
 * Listing 1.3 Asynchronous connect
 *
 * Listing 1.4 Callback in action
 * */
public static void connect() {
    Channel channel = CHANNEL_FROM_SOMEWHERE; //reference form somewhere
    // Does not block
    ChannelFuture future = channel.connect(
            new InetSocketAddress("192.168.0.1", 25));
    future.addListener(new ChannelFutureListener() {
        @Override
        public void operationComplete(ChannelFuture future) {
            if (future.isSuccess()) {
                ByteBuf buffer = Unpooled.copiedBuffer(
                        "Hello", Charset.defaultCharset());
                ChannelFuture wf = future.channel()
                        .writeAndFlush(buffer);
                // ...
            } else {
                Throwable cause = future.cause();
                cause.printStackTrace();
            }
        }
    });
}
```
### 事件和ChannelHandler
#### 选择器、事件和EventLoop
通过触发事件将Selector从应用程序中抽象出来，消除了手动编写派发代码。在内部，每个Channel分配一个EventLoop。EventLoop由一个线程驱动，处理一个Chennel的所有IO事件。
# 你的第一款Netty应用程序
## 编写服务器
### ChannelHandler和业务逻辑
这个简单单的应用只需要用到少量的方法，继承Channel-InboundHandlerAdapter，提供了ChannelInboundHandler的默认实现。
```
public class EchoServerHandler extends ChannelInboundHandlerAdapter {
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) {
        ByteBuf in = (ByteBuf) msg;
        System.out.println(
                "Server received: " + in.toString(CharsetUtil.UTF_8));
        ctx.write(in);
    }

    @Override
    public void channelReadComplete(ChannelHandlerContext ctx)
            throws Exception {
        ctx.writeAndFlush(Unpooled.EMPTY_BUFFER)
                .addListener(ChannelFutureListener.CLOSE);
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx,
        Throwable cause) {
        cause.printStackTrace();
        ctx.close();
    }
}
```
### 引导服务器
```
public class EchoServer {
    private final int port;

    public EchoServer(int port) {
        this.port = port;
    }

    public static void main(String[] args)
        throws Exception {
        if (args.length != 1) {
            System.err.println("Usage: " + EchoServer.class.getSimpleName() +
                " <port>"
            );
            return;
        }
        int port = Integer.parseInt(args[0]);
        new EchoServer(port).start();
    }

    public void start() throws Exception {
        final EchoServerHandler serverHandler = new EchoServerHandler();
        EventLoopGroup group = new NioEventLoopGroup(); // 进行事件处理，如接受新连接以及读写数据
        try {
            ServerBootstrap b = new ServerBootstrap(); // 引导和绑定服务器
            b.group(group)
                .channel(NioServerSocketChannel.class)
                .localAddress(new InetSocketAddress(port)) //指定绑定本地地址
                .childHandler(new ChannelInitializer<SocketChannel>() { // 关键，当一个新的连接被接受时，新的子Channel会被创建。将新的Handler添加到ChannelPipeline中。
                    @Override
                    public void initChannel(SocketChannel ch) throws Exception {
                        ch.pipeline().addLast(serverHandler);
                    }
                });

            ChannelFuture f = b.bind().sync(); // 绑定服务器
            System.out.println(EchoServer.class.getName() +
                " started and listening for connections on " + f.channel().localAddress());
            f.channel().closeFuture().sync();
        } finally {
            group.shutdownGracefully().sync();
        }
    }
}
```
## 编写Echo客户端
### 通过ChannelHandler实现客户端逻辑
```
public class EchoClientHandler
    extends SimpleChannelInboundHandler<ByteBuf> {
    @Override
    public void channelActive(ChannelHandlerContext ctx) {
        ctx.writeAndFlush(Unpooled.copiedBuffer("Netty rocks!",
                CharsetUtil.UTF_8));
    }

    @Override
    public void channelRead0(ChannelHandlerContext ctx, ByteBuf in) {
        System.out.println(
                "Client received: " + in.toString(CharsetUtil.UTF_8));
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx,
        Throwable cause) {
        cause.printStackTrace();
        ctx.close();
    }
}
```
### 引导客户端
```
public class EchoClient {
    private final String host;
    private final int port;

    public EchoClient(String host, int port) {
        this.host = host;
        this.port = port;
    }

    public void start()
        throws Exception {
        EventLoopGroup group = new NioEventLoopGroup();
        try {
            Bootstrap b = new Bootstrap();
            b.group(group)
                .channel(NioSocketChannel.class)
                .remoteAddress(new InetSocketAddress(host, port))
                .handler(new ChannelInitializer<SocketChannel>() {
                    @Override
                    public void initChannel(SocketChannel ch)
                        throws Exception {
                        ch.pipeline().addLast(
                             new EchoClientHandler());
                    }
                });
            ChannelFuture f = b.connect().sync();
            f.channel().closeFuture().sync();
        } finally {
            group.shutdownGracefully().sync();
        }
    }

    public static void main(String[] args)
            throws Exception {
        if (args.length != 2) {
            System.err.println("Usage: " + EchoClient.class.getSimpleName() +
                    " <host> <port>"
            );
            return;
        }

        final String host = args[0];
        final int port = Integer.parseInt(args[1]);
        new EchoClient(host, port).start();
    }
}
```
# Netty的组件和设计
## Channel、EventLoop和ChannelFuture
- Channel -- Socket
- EventLoop -- 控制流、多线程处理、并发
- ChannelFuture -- 异步通知

### Channel 接口
基于Java的网络编程中，其基本的构造时Socket
### EventLoop接口
EventLoop定义了Netty核心抽象，用于处理连接的生命周期中发生的事件。
一个EventLoop在它的生命周期内只和一个Thread绑定，所有由EventLoop处理的IO事件都将在它专有的Thread上被处理
一个Channel只能注册于一个EventLoop，但一个EventLoop会被分配给一个或多个Channel
因此一个Channel的IO操作都是相同的Thread执行的，消除了同步的需要
### ChannelFuture接口
可以将ChannelFuture看作将来要执行的操作的结果的占位符。
## ChannelHandler和ChannelPipeline
### ChannelHandler
ChannelHandler专门用于几乎任何类型的动作，例如将数据从一个格式转换为另外一种格式，或者处理转换过程中所抛出的异常。
### ChannelPipeline 接口
ChannelPipeline是ChannelHandler的编排顺序
入站和出站ChannelHandler可以被安装到同一个ChannelPipeline

通过使用作为参数传递到每个方法的ChannelhandlerContext, 可以通过调用ChannelHandlerContext上的对应方法，将事件传递给下一个ChannelHandler的方法的实现。
当ChannelHandler被添加到ChannelPipeline时，会被分配一个ChannelHandlerContext，代表了ChannelHandler和ChannelPipeline之间的绑定。
有两种发送消息的方式
- 写入Channel中，消息从Channel——pipeline的尾端开始流动
- 写入ChannelHandler相关联的ChannelHandlerContext对象中，消息从下一个ChannelHandler开始流动

### SimpleChannelInboundHandler
当需要利用一个ChannelHandler来接受解码消息，并对该数据应用业务逻辑，只需要扩展基类SimpleChannelInboundHandler，重写方法并获取一个ChannelHandlerContext的引用。
其中最重要的方法时channelRead0，不要阻塞当前的IO线程。
## 引导
用于客户端Bootstrap，EventLoopGroup的数目为1
用于服务端ServerBootstrap，EventLoopGroup的数目为2，第一组只包含一个ServerChannel，用于端口绑定，其EventLoop负责连接请求创建Channel。第二组包含处理客户端连接的Channel，当连接接受时，给Channel分配一个EventLoop。
# 传输
## 案例研究： 传输迁移
### 不通过Netty使用OIO和NIO
```
public class PlainOioServer {
    public void serve(int port) throws IOException {
        final ServerSocket socket = new ServerSocket(port);
        try {
            for(;;) {
                final Socket clientSocket = socket.accept();
                System.out.println(
                        "Accepted connection from " + clientSocket);
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        OutputStream out;
                        try {
                            out = clientSocket.getOutputStream();
                            out.write("Hi!\r\n".getBytes(
                                    Charset.forName("UTF-8")));
                            out.flush();
                            clientSocket.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                        } finally {
                            try {
                                clientSocket.close();
                            } catch (IOException ex) {
                                // ignore on close
                            }
                        }
                    }
                }).start();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

```
public class PlainNioServer {
    public void serve(int port) throws IOException {
        ServerSocketChannel serverChannel = ServerSocketChannel.open();
        serverChannel.configureBlocking(false);
        ServerSocket ss = serverChannel.socket();
        InetSocketAddress address = new InetSocketAddress(port);
        ss.bind(address);
        Selector selector = Selector.open();
        serverChannel.register(selector, SelectionKey.OP_ACCEPT);
        final ByteBuffer msg = ByteBuffer.wrap("Hi!\r\n".getBytes());
        for (;;){
            try {
                selector.select();
            } catch (IOException ex) {
                ex.printStackTrace();
                //handle exception
                break;
            }
            Set<SelectionKey> readyKeys = selector.selectedKeys();
            Iterator<SelectionKey> iterator = readyKeys.iterator();
            while (iterator.hasNext()) {
                SelectionKey key = iterator.next();
                iterator.remove();
                try {
                    if (key.isAcceptable()) {
                        ServerSocketChannel server =
                                (ServerSocketChannel) key.channel();
                        SocketChannel client = server.accept();
                        client.configureBlocking(false);
                        client.register(selector, SelectionKey.OP_WRITE |
                                SelectionKey.OP_READ, msg.duplicate());
                        System.out.println(
                                "Accepted connection from " + client);
                    }
                    if (key.isWritable()) {
                        SocketChannel client =
                                (SocketChannel) key.channel();
                        ByteBuffer buffer =
                                (ByteBuffer) key.attachment();
                        while (buffer.hasRemaining()) {
                            if (client.write(buffer) == 0) {
                                break;
                            }
                        }
                        client.close();
                    }
                } catch (IOException ex) {
                    key.cancel();
                    try {
                        key.channel().close();
                    } catch (IOException cex) {
                        // ignore on close
                    }
                }
            }
        }
    }
}
```
### 通过Netty使用OIO和NIO
```
public class NettyOioServer {
    public void server(int port)
            throws Exception {
        final ByteBuf buf =
                Unpooled.unreleasableBuffer(Unpooled.copiedBuffer("Hi!\r\n", Charset.forName("UTF-8")));
        EventLoopGroup group = new OioEventLoopGroup();
        try {
            ServerBootstrap b = new ServerBootstrap();
            b.group(group)
                    .channel(OioServerSocketChannel.class)
                    .localAddress(new InetSocketAddress(port))
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        public void initChannel(SocketChannel ch)
                                throws Exception {
                                ch.pipeline().addLast(
                                    new ChannelInboundHandlerAdapter() {
                                        @Override
                                        public void channelActive(
                                                ChannelHandlerContext ctx)
                                                throws Exception {
                                            ctx.writeAndFlush(buf.duplicate())
                                                    .addListener(
                                                            ChannelFutureListener.CLOSE);
                                        }
                                    });
                        }
                    });
            ChannelFuture f = b.bind().sync();
            f.channel().closeFuture().sync();
        } finally {
            group.shutdownGracefully().sync();
        }
    }
}
```

```
public class NettyNioServer {
    public void server(int port) throws Exception {
        final ByteBuf buf =
                Unpooled.unreleasableBuffer(Unpooled.copiedBuffer("Hi!\r\n",
                        Charset.forName("UTF-8")));
        NioEventLoopGroup group = new NioEventLoopGroup(); // 不同
        try {
            ServerBootstrap b = new ServerBootstrap();
            b.group(group).channel(NioServerSocketChannel.class) // 不同
                    .localAddress(new InetSocketAddress(port))
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                                      @Override
                                      public void initChannel(SocketChannel ch)
                                              throws Exception {
                                              ch.pipeline().addLast(
                                                  new ChannelInboundHandlerAdapter() {
                                                      @Override
                                                      public void channelActive(
                                                              ChannelHandlerContext ctx) throws Exception {
                                                                ctx.writeAndFlush(buf.duplicate())
                                                                  .addListener(
                                                                          ChannelFutureListener.CLOSE);
                                                      }
                                                  });
                                      }
                                  }
                    );
            ChannelFuture f = b.bind().sync();
            f.channel().closeFuture().sync();
        } finally {
            group.shutdownGracefully().sync();
        }
    }
}
```
## 传输API
由于Channel是独一无二的，两个不同的Channel实例返回相同的散列码，将抛出Error。
ChannelHandler用途
- 格式转换
- 异常通知
- 提供Channel变为活动或者非活动的通知
- 提供Channel注册到EventLoop，或者从EventLoop注销的通知
- 自定义通知

可以添加或者移除ChannelHandler来修改ChannelPipeline。添加（SslHandler）来支持STARTLS协议。
## 内置的传输
### NIO -- 非阻塞IO
选择器运行在一个检查状态变化并对其做出相应响应的线程上，在应用程序对状态的改变做出响应后，选择器将会被重置，并将重复这个过程。
### Epoll -- 用于Linux的本地非阻塞传输
在Linux（2.5.44以上）上，考虑这个版本的传输。
只需将NioEventLoopGroup替换成EpollEventLoopGroup，并将NioServerSocketChannel替换成EpollServerSocketChannel
### OIO -- 旧阻塞IO
Netty利用SO_TIMEOUT这个Socket标志，将用于异步传输相同的API来支持OIO。
如果操作在指定的时间间隔内没有完成，则将会抛出一个SocketTimeout Exception 。Netty将捕获这异常并继续处理循环。在EventLoop 下一次运行时，它将再次尝试。
### 用于JVM内部通信的Local传输
用于在同一个JVM中运行的客户端和服务器程序之间的异步通信。这个传输也支持对于所有Netty传输实现都共同的API。
服务器Channel相关联的SocketAddress并没有绑定物理网络地址。
### Embedded传输
扩展一个ChannelHandler功能，而不修改其内部代码。
如果想为自己的ChannelHandler实现编写单元测试，考虑这个传输。这样就不需要创建大量的Mock。
# ByteBuf
## ByteBuf的API
- abstract class ByteBuf
- interface ByteBufHolder
## ByteBuf类 -- Netty的数据容器
### 它是如何工作的
ByteBuffer有readerIndex和writerIndex。
ByteBuf名称以read或者write开头的方法，将推进对应的索引，而以set或者get开头的操作则不会。
### 使用模式
#### 堆缓冲区
存储在JVM的堆空间中。其被称为支撑数组，它能在没有使用池化的情况下提供分配和释放。
```
    public static void heapBuffer() {
        ByteBuf heapBuf = Unpooled.buffer(1024);
        if (heapBuf.hasArray()) {
            byte[] array = heapBuf.array();
            int offset = heapBuf.arrayOffset() + heapBuf.readerIndex();
            int length = heapBuf.readableBytes();
            handleArray(array, offset, length);
        }
    }
```
#### 直接缓冲区
避免每次IO操作前后将缓冲区内容复制到一个中间缓冲区。
缺点：分配释放昂贵，如果数据不是堆上的，还得进行一次复制。如果数据作为数组来访问，建议使用堆内存。
```
    public static void directBuffer() {
        ByteBuf directBuf = Unpooled.buffer(1024);
        if (!directBuf.hasArray()) {
            int length = directBuf.readableBytes();
            byte[] array = new byte[length];
            directBuf.getBytes(directBuf.readerIndex(), array);
            handleArray(array, 0, length);
        }
    }
```
#### 复合缓冲区
为多个ByteBuf提供一个聚合视图，根据需要添加或者删除ByteBuf实例。通过CompositeByteBuf来实现这个模式。
当重复数据多时，可以消除没有必要的复制。
```
    public static void byteBufComposite() {
        CompositeByteBuf messageBuf = Unpooled.compositeBuffer();
        ByteBuf headerBuf = Unpooled.buffer(1024); // can be backing or direct
        ByteBuf bodyBuf = Unpooled.buffer(1024);   // can be backing or direct
        messageBuf.addComponents(headerBuf, bodyBuf);
        //...
        messageBuf.removeComponent(0); // remove the header
        for (ByteBuf buf : messageBuf) {
            System.out.println(buf.toString());
        }

        // 可能不支持访问其支撑数组，访问数据类似于直接缓冲区的模式。
        int length = messageBuf.readableBytes();
        byte[] array = new byte[length];
        messageBuf.getBytes(messageBuf.readerIndex(), array); // 将字节读入数组
        handleArray(array, 0, array.length); // 使用偏移量和长度的方式使用数组
    }
```
## 字节操作
### 随机访问索引
这样访问不会改变readerIndex和writerIndex
```
    public static void byteBufRelativeAccess() {
        ByteBuf buffer = Unpooled.buffer(1024); //get reference form somewhere
        for (int i = 0; i < buffer.capacity(); i++) {
            byte b = buffer.getByte(i);
            System.out.println((char) b);
        }
    }
```
### 顺序访问索引
ByteBuf有两个索引，而JDK中只有一个，所以需要flip()
### 可丢弃字节
使用discardReadBytes()，回收空间，变为可写，但其内容不能保证清空。
该方法确保了可写段的最大化，但可能会内存复制，因为可可写字节被移动到缓冲区的开始位置。
### 可读字节
任何以read或者skip开头的操作都将检索或者跳过位于当前readerIndex的数据，并增加已读字节数。
如果被调用的方法需要一个ByteBuf参数作为写入的目标，并且没有指定目标索引参数，那么目标缓冲区的writerIndex也会被增加。
### 可写字节
```
    public static void write() {
        // Fills the writable bytes of a buffer with random integers.
        ByteBuf buffer = Unpooled.buffer(1024);//get reference form somewhere
        while (buffer.writableBytes() >= 4) {
            buffer.writeInt(random.nextInt());
        }
    }
```
### 索引管理
JDKInputStream中的mark(int readlimit)和reset()分别返回当前位置和重置位置。
同样，调用markReaderIndex()、markWriterIndex()、resetWriterIndex()、resetReaderIndex(）得到同样效果，只是没有readlimit来标记什么时候失效。
可调用clear()将其都置为0。这比discardReadBytes()轻量的多。
### 查找操作
简单的可以使用indexOf()，复杂的需要一个ByteBufProcessor作为参数的方法达成。
该类也定义常见的值如NULL或者 '/r'
```
    public static void byteProcessor() {
        ByteBuf buffer = Unpooled.buffer(1024);//get reference form somewhere
        int index = buffer.forEachByte(ByteProcessor.FIND_CR); // Netty 4.0.x
        // int index = buffer.forEachByte(ByteBufProcessor.FIND_CR);  // Netty 4.1.x
    }
```
### 派生缓冲区
duplicate()、slice() 都将返回一个新的ByteBuf实例，内部存储和JDK的ByteBuffer一样是共享的，但有自己的读写标记索引。
```
    public static void byteBufSlice() {
        Charset utf8 = Charset.forName("UTF-8");
        ByteBuf buf = Unpooled.copiedBuffer("Netty in Action rocks!", utf8);
        ByteBuf sliced = buf.slice(0, 15);
        System.out.println(sliced.toString(utf8));
        buf.setByte(0, (byte)'J');
        assert buf.getByte(0) == sliced.getByte(0);
    }
```
如果需要一个真实的副本，可以用copy()
```
    public static void byteBufCopy() {
        Charset utf8 = Charset.forName("UTF-8");
        ByteBuf buf = Unpooled.copiedBuffer("Netty in Action rocks!", utf8);
        ByteBuf copy = buf.copy(0, 15);
        System.out.println(copy.toString(utf8));
        buf.setByte(0, (byte)'J');
        assert buf.getByte(0) != copy.getByte(0);
    }
```
## ByteBufHolder接口
提供了高级特性的支持，如缓冲区池化。
## ByteBuf分配
### 按需分配 ByteBufAllocator接口
为了降低开销，进行了池化。
```
    public static void obtainingByteBufAllocatorReference(){
        Channel channel = new NioSocketChannel(); //get reference form somewhere
        ByteBufAllocator allocator = channel.alloc();
        //...
        ChannelHandlerContext ctx = DummyChannelHandlerContext.DUMMY_INSTANCE;//get reference form somewhere
        ByteBufAllocator allocator2 = ctx.alloc();
        //...
    }
```
提供了两种ByteBufAllocator实现：PooledByteBufAllocator和UnpooledByteBufAllocator，前者使用了池化，该实现使用了jemalloc。后者不池化，每次调用都返回一个新的实例。
默认使用PooledByteBufAllocator。
### Unpooled缓冲区
Unpooled工具类，提供了静态方法来创建未池化的ByteBuf实例。
### ByteBufUtil类
最有价值的是hexdump()方法，以十六进制打印ByteBuf的内容。另一个是equals(ByteBuf, ByteBuf)
## 引用计数
```
    public static void referenceCounting(){
        Channel channel = new NioSocketChannel(); //get reference form somewhere
        ByteBufAllocator allocator = channel.alloc();
        //...
        ByteBuf buffer = allocator.directBuffer();
        assert buffer.refCnt() == 1;
        //...
    }

    public static void releaseReferenceCountedObject(){
        ByteBuf buffer = Unpooled.buffer(1024);//get reference form somewhere
        boolean released = buffer.release();
        //...
    }
```
一般最后访问对象的一方负责释放。
自定义Referencecounted的实现类，其release()方法总是0，从而一次性地使所有的活动引用都失效。
# ChannelHandler和ChannelPipeline
## ChannelHandler家族
### Channel的生命周期
registered->active->inactive->unregistered
状态改变时会生成对应的事件，发给ChannelPipeline中的ChannelHandler。
### ChannelHandler的生命周期
- added
- removed
- exceptionCaught
### ChannelInboundHandler接口
```
public class DiscardHandler extends ChannelInboundHandlerAdapter {

    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) {
        ReferenceCountUtil.release(msg); // 显示地释放与池化的ByteBuf实例相关的内存 Netty提供的实用方法
    }

}
```

```
// 使用WARN级别的日志消息记录未释放的资源
public class SimpleDiscardHandler
    extends SimpleChannelInboundHandler<Object> {
    @Override
    public void channelRead0(ChannelHandlerContext ctx,
        Object msg) {
        // No need to do anything special // 由于SimpleChannelInboundHandler会自动释放资源，不应该存储指向任何消息的引用供将来使用，这些引用都将会失效。
    }
}
```
### ChannelOutboundHandler接口
一个强大功能是可以按需推迟操作或者事件。
#### ChannelPromise与ChannelFuture
ChannelOutboundHandler需要一个ChannelPromise参数，以便在操作完成时得到通知。
ChannelPromise是ChannelFuture的一个子类，定义了一些可写的方法，如setSuccess() 和 setFailure()，从而使ChannelFuture不可变。
### ChannelHandler适配器
可以使用ChannelInboundHandlerAdapter和ChannelOutboundHandlerAdapter类作为自己ChannelHandler的起始点。
ChannelHandlerAdapter还提供了isSharable()，如果其对应的实现被标注未Sharable，表示它可以被添加到多个ChannelPipeline中。
### 资源管理
每当使用ChannelInboundHandler.channelRead()或者ChannelOutboundHandler.write()处理数据时，都需要确保没有任何资源泄漏。
如完全使用完某个ByteBuf后，调整其引用计数。
Netty提供了ResourceLeakDetector，对缓冲区分配大约1%的采样。
如果有泄漏会有日志消息：LEAK：ByteBuf.release() was not called before it's garbage-collected.
java -Dio.netty.leakDetectionLevel=ADVANCED // 设置检测级别
```
public class DiscardOutboundHandler
    extends ChannelOutboundHandlerAdapter {
    @Override
    public void write(ChannelHandlerContext ctx,
        Object msg, ChannelPromise promise) {
        ReferenceCountUtil.release(msg);
        promise.setSuccess();
    }
}
```
如果消息达到了传输层，那么它被写入时或者Channel关闭时，被自动释放。
## ChannelPipeline接口
每一个新创建的Channel都被分配一个新的ChannelPipeline。这项关联是永久性的。
ChannelHandlerContext使得ChannelHandler能够和它的ChannelPipeline以及其他的ChannelHandler交互。
在ChannelPipeline传播事件时，会测试ChannelHandler的类型是否和事件的运动方向相匹配。如果不匹配，将跳过该ChannelHandler。
### 修改ChannelPipeline
通过添加、删除或者替换其他ChannelHandler来实时修改ChannelPipeline的布局。
#### ChannelHandler的执行和阻塞
ChannelPipeline中的每个ChannelHandler通过EventLoop（IO线程）来处理传递给它的事件的。不要堵塞线程。
但有时与阻塞API交互，Channel Pipeline有一些接受一个EventExecutorGroup的add()方法。如果一个事件被传递给一个自定义的EventExecutorGroup，它将被包含在这个EventExecutorGroup中某个EventExecutor处理，从而被Channel本身的Event
Loop中移除，Netty提供了DefaultEventExecutorGroup的默认实现。
### 触发事件
ChannelPipeline保存了与Channel相关联的ChannelHandler，根据需要添加或删除ChannelHandler来动态地修改。
## ChannelHandlerContext接口
ChannelHandlerContext代表了ChannelHandler和ChannelPipeline之间的关联，每当有ChannleHandler添加到ChannelPipeline中时，都会创建ChannelHandlerContext。
其主要功能是管理它所关联的ChannelHandler和在同一个ChannelPipeline中的其他ChannelHandler之间的交互。
ChannelHandlerContext和ChannelHandler之间的关联绑定时永远不会改变的，所有缓存对它的引用是安全的。
对于其他类的同名方法，ChannelHandlerContext的方法将产生更短的事件流，获得更大的性能。
### 使用ChannelHandlerContext
虽然被调用的Channel或ChannelPipeline上的write()方法将一直传播事件通过整个ChannelPipeline，但是在ChannelHandler的级别上，事件从一个ChannelHandler到下一个ChannelHandler的移动是由ChannelHandlerContext上的调用完成的。
想调用从某个特定的ChannelHandler开始的处理过程，必须获取在(ChannelPipeline)该ChannelHandler之前的ChannelHandler所关联的ChannelHandlerContext。
### ChannelHandler和ChannelHandlerContext的高级用法
通过将ChannelHandler添加到ChannelPipeline中来实现动态的协议切换。
另一种高级的用法是缓存到ChannelHandlerContext的引用以供稍后的使用，这可能会发生在任何的ChannelHandler方法之外，甚至来自不同的线程。
```
public class WriteHandler extends ChannelHandlerAdapter {
    private ChannelHandlerContext ctx;
    @Override
    public void handlerAdded(ChannelHandlerContext ctx) {
        this.ctx = ctx;
    }
    public void send(String msg) {
        ctx.writeAndFlush(msg);
    }
}
```
因为一个ChannelHandler可以属于多个ChannelPipeline,所以它也可以绑定到多个ChannelHandlerContext实例。用于这种用法的ChannelHandler必须要使用@Sharable注解，并且保证ChannelHandler必须是线程安全的。
```
@Sharable
public class SharableHandler extends ChannelInboundHandlerAdapter {
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) {
        System.out.println("channel read message " + msg);
        ctx.fireChannelRead(msg);
    }
}
```
为了在多个ChannelPipeline中安装同一个ChannelHandler的一个常见的原因是用于收集跨越多个Channel的统计信息，才共享同一个ChannelHandler。
## 异常处理
### 处理入站异常
在ChannelInboundHandler中重写exceptionCaught()
ChannelInboundHandler通常位于ChannelPipeline的最后，这确保了所有的入站异常都会被处理。
### 处理出站异常
第一种方法，添加ChannelFutureListener只需要调用ChannelFuture实例上的addListener(ChannelFutureListener)方法。
这种异常处理更加细致。
```
public class ChannelFutures {
    private static final Channel CHANNEL_FROM_SOMEWHERE = new NioSocketChannel();
    private static final ByteBuf SOME_MSG_FROM_SOMEWHERE = Unpooled.buffer(1024);

    public static void addingChannelFutureListener(){
        Channel channel = CHANNEL_FROM_SOMEWHERE; // get reference to pipeline;
        ByteBuf someMessage = SOME_MSG_FROM_SOMEWHERE; // get reference to pipeline;
        //...
        io.netty.channel.ChannelFuture future = channel.write(someMessage);
        future.addListener(new ChannelFutureListener() {
            @Override
            public void operationComplete(io.netty.channel.ChannelFuture f) {
                if (!f.isSuccess()) {
                    f.cause().printStackTrace();
                    f.channel().close();
                }
            }
        });
    }
}
```
第二种方法，将ChannelFutureListener添加到即将作为参数传递给ChannelOutboundHandler的方法的ChannelPromise。
这种方式更加的简单。
```
public class OutboundExceptionHandler extends ChannelOutboundHandlerAdapter {
    @Override
    public void write(ChannelHandlerContext ctx, Object msg,
        ChannelPromise promise) {
        promise.addListener(new ChannelFutureListener() {
            @Override
            public void operationComplete(ChannelFuture f) {
                if (!f.isSuccess()) {
                    f.cause().printStackTrace();
                    f.channel().close();
                }
            }
        });
    }
}
```
如果ChannelOutboundHandler本身抛出了异常，Netty本身会通知任何已经注册到对应ChannelPromise的监听器。
# EventLoop和线程模型
## 线程模型概述
虽然池化和重用线程相对于简单地为每个任务都创建和销毁线程是一种进步，但是它并没有消除由上下文切换带来的开销，随着线程数量的增加越发明显。
## EventLoop接口
一个EventLoop将由一个永远都不会改变的Thread驱动，同时任务(Runnable或者Callable)可以直接提交给EventLoop实现，立即执行或者调度执行。
更具配置，会创建多个EventLoop并且单个EventLoop被指派用于服务多个Channel。
事件/任务执行的顺序是 FIFO
### Netty 4 中的IO和事件处理
所有IO操作和事件都由已经被分配给了EventLoop的那个Thread来处理。
### Netty 3中的IO操作
只保证了入站事件会在IO线程(Netty4 中的EventLoop)中执行。所有出站事件都由调用线程处理，可能是IO线程也可能是别的线程。
这就需要在ChannelHandler中对出站事件进行仔细同步。不可能保证多个线程不会在同一时刻尝试访问出站事件。例如，通过不同线程中调用Channel.write()，针对同一个Channel同时触发出站的事件，就会发生这种情况。
当出站事件触发了入站事件时，Netty3 需要交给IO线程执行，带来额外的上下文切换。
Netty4 采用的线程模型，通过在同一个线程中处理某个给定的EventLoop产生的所有事件，解决了这个问题。消除了多个ChannelHandler中需要同步的需要(除了Channel中共享的)
## 任务调度
### JDK的任务调度API
```
    public static void schedule() {
        ScheduledExecutorService executor =
                Executors.newScheduledThreadPool(10);

        ScheduledFuture<?> future = executor.schedule(
            new Runnable() {
            @Override
            public void run() {
                System.out.println("Now it is 60 seconds later");
            }
        }, 60, TimeUnit.SECONDS);
        //...
        executor.shutdown();
    }
```
### 使用EventLoop调度任务
```
    public static void scheduleViaEventLoop() {
        Channel ch = CHANNEL_FROM_SOMEWHERE; // get reference from somewhere
        ScheduledFuture<?> future = ch.eventLoop().schedule(
            new Runnable() {
            @Override
            public void run() {
                System.out.println("60 seconds later");
            }
        }, 60, TimeUnit.SECONDS);
    }
```
EventLoop扩展了ScheduledExecutorService，提供了使用JDK实现可用的所有方法。
想要取消或者检查执行状态，可以使用返回的ScheduledFuture。
```
    public static void cancelingTaskUsingScheduledFuture(){
        Channel ch = CHANNEL_FROM_SOMEWHERE; // get reference from somewhere
        ScheduledFuture<?> future = ch.eventLoop().scheduleAtFixedRate(
                new Runnable() {
                    @Override
                    public void run() {
                        System.out.println("Run every 60 seconds");
                    }
                }, 60, 60, TimeUnit.SECONDS);
        // Some other code that runs...
        boolean mayInterruptIfRunning = false;
        future.cancel(mayInterruptIfRunning);
    }
```
## 实现细节
### 线程管理
Netty线程模型性能取决于Thread的身份确认，即确实它是否是分配给当前Channel以及它的EventLoop的那个线程。
如果调用线程正是支撑EventLoop的线程，那么所提交的代码块将被直接执行，否则放入内部队列，稍后执行。这也解释了任何的Thread是如何与Channel直接交互而无需在ChannelHandler中进行额外同步的。
每个EventLoop都有自己的任务队列。
如果必须要进行阻塞或者执行长时间运行的任务，建议使用EventExecutor。
### EventLoop线程的分配
#### 异步传输
使用少量的EventLoop(相关联的Thread)，还可能被多个Channel共享。通过尽可能少量的Thread来支撑大量的Channel，而不是每个Channel分配一个Thread。
EventLoop通常支撑多个Channel，他们的ThreadLocal都是相同的，一些无状态的上下文中，它依然可被用于共享对象，甚至事件。
#### 阻塞传输
每个Channle都被分配一个EventLoop(以及它的Thrad)。
# 引导
引导一个应用程序是指对它进行配置，使它运行起来的过程。
## Bootstrap类
服务器使用一个父Channel来接受来自客户端的链接，并创建子Channel用于通信；而客户端将最可能只需要一个单独的、没有父Channel的Channel用于所有网络交互。
两种应用程序类型之间通用的引导步骤由AbstractBootstrap处理，特定于客户端或者服务器的引导步骤分别由Bootstrap或Server
Bootstrap处理。
有时可能需要创建多个具有配置类似甚至相同的Channel。为了支持这个情况，不需要为每个Channel都创建并配置一个新的引导类实例，AbstractBootstrap被标记为了Cloneable。
这种方式只会创建一个浅拷贝，Channel实例之间共享。
## 引导客户端和无连接协议
### 引导客户端
```
public class BootstrapClient {
    public static void main(String args[]) {
        BootstrapClient client = new BootstrapClient();
        client.bootstrap();
    }

    public void bootstrap() {
        EventLoopGroup group = new NioEventLoopGroup();
        Bootstrap bootstrap = new Bootstrap();
        bootstrap.group(group)
            .channel(NioSocketChannel.class)
            .handler(new SimpleChannelInboundHandler<ByteBuf>() {
                @Override
                protected void channelRead0(
                    ChannelHandlerContext channelHandlerContext,
                    ByteBuf byteBuf) throws Exception {
                    System.out.println("Received data");
                }
                });
        ChannelFuture future =
            bootstrap.connect(
                    new InetSocketAddress("www.manning.com", 80));
        future.addListener(new ChannelFutureListener() {
            @Override
            public void operationComplete(ChannelFuture channelFuture)
                throws Exception {
                if (channelFuture.isSuccess()) {
                    System.out.println("Connection established");
                } else {
                    System.err.println("Connection attempt failed");
                    channelFuture.cause().printStackTrace();
                }
            }
        });
    }
}
```
### Channel和EventLoopGroup的兼容性
不能混用具有不同前缀的组件，如NioEventLoopGroup和OioSocketChannel。
在bind()或者connect()方法之前，必须设置所需组件：
- group()
- channel(), channelFactory()
- handler() 该方法还需要配置好ChannelPipeline

## 引导服务器
### ServerBootstrap类
### 引导服务器
childHandler(),childAttr(),childoption()都是用于服务器应用程序的操作。
ServerChannel的实现负责创建子Channel，这些Channel代表已经接受的连接。负责引导ServerChannel的ServerBootstrap提供了这些方法，以简化将设置应用到已被接受的子Channel的ChannelConfig的任务。
```
public class BootstrapServer {

    public void bootstrap() {
        NioEventLoopGroup group = new NioEventLoopGroup();
        ServerBootstrap bootstrap = new ServerBootstrap();
        bootstrap.group(group)
            .channel(NioServerSocketChannel.class)
            .childHandler(new SimpleChannelInboundHandler<ByteBuf>() {
                @Override
                protected void channelRead0(ChannelHandlerContext channelHandlerContext,
                    ByteBuf byteBuf) throws Exception {
                    System.out.println("Received data");
                }
            });
        ChannelFuture future = bootstrap.bind(new InetSocketAddress(8080));
        future.addListener(new ChannelFutureListener() {
            @Override
            public void operationComplete(ChannelFuture channelFuture)
                throws Exception {
                if (channelFuture.isSuccess()) {
                    System.out.println("Server bound");
                } else {
                    System.err.println("Bind attempt failed");
                    channelFuture.cause().printStackTrace();
                }
            }
        });
    }
}
```
## 从Channel引导客户端
通过将已被接受的子Channel的EventLoop传递给Bootstrap的group()方法来共享该EventLoop。因为分配给EventLoop的所有Channel都使用同一个线程，这样避免了额外的线程创建和上下文切换。
```
public class BootstrapSharingEventLoopGroup {

    public void bootstrap() {
        ServerBootstrap bootstrap = new ServerBootstrap();
        bootstrap.group(new NioEventLoopGroup(), new NioEventLoopGroup())
            .channel(NioServerSocketChannel.class)
            .childHandler(
                new SimpleChannelInboundHandler<ByteBuf>() {
                    ChannelFuture connectFuture;
                    @Override
                    public void channelActive(ChannelHandlerContext ctx)
                        throws Exception {
                        Bootstrap bootstrap = new Bootstrap();
                        bootstrap.channel(NioSocketChannel.class).handler(
                            new SimpleChannelInboundHandler<ByteBuf>() {
                                @Override
                                protected void channelRead0(
                                    ChannelHandlerContext ctx, ByteBuf in)
                                    throws Exception {
                                    System.out.println("Received data");
                                }
                            });
                        bootstrap.group(ctx.channel().eventLoop()); // 使用与分配给已接受的子Channel相同的EventLoop
                        connectFuture = bootstrap.connect(
                            new InetSocketAddress("www.manning.com", 80));
                    }

                    @Override
                    protected void channelRead0(
                        ChannelHandlerContext channelHandlerContext,
                            ByteBuf byteBuf) throws Exception {
                        if (connectFuture.isDone()) {
                            // do something with the data 
                        }
                    }
                });
        ChannelFuture future = bootstrap.bind(new InetSocketAddress(8080));
        future.addListener(new ChannelFutureListener() {
            @Override
            public void operationComplete(ChannelFuture channelFuture)
                throws Exception {
                if (channelFuture.isSuccess()) {
                    System.out.println("Server bound");
                } else {
                    System.err.println("Bind attempt failed");
                    channelFuture.cause().printStackTrace();
                }
            }
        });
    }
}
```
尽量重用EventLoop，减少线程创建所带来的开销。
## 在引导过程中添加多个ChannelHandler
当引导的过程中，你只能设置一个ChannelHandler,但需求又很复杂。
Netty提供了一个特殊的ChannelInboundHandlerAdapter子类。
其中initChannel()方法提供了一种将多个ChannelHandler添加到一个ChannelPipeline中的简便方法。
```
public class BootstrapWithInitializer {

    public void bootstrap() throws InterruptedException {
        ServerBootstrap bootstrap = new ServerBootstrap();
        bootstrap.group(new NioEventLoopGroup(), new NioEventLoopGroup())
            .channel(NioServerSocketChannel.class)
            .childHandler(new ChannelInitializerImpl());
        ChannelFuture future = bootstrap.bind(new InetSocketAddress(8080));
        future.sync();
    }

    final class ChannelInitializerImpl extends ChannelInitializer<Channel> {
        @Override
        protected void initChannel(Channel ch) throws Exception {
            ChannelPipeline pipeline = ch.pipeline();
            pipeline.addLast(new HttpClientCodec());
            pipeline.addLast(new HttpObjectAggregator(Integer.MAX_VALUE));

        }
    }
}
```
## 使用Netty的ChannelOption和属性
Netty提供了AttributeMap抽象以及AttributeKey<T>。使用这些工具，便可以安全地将任何类型的数据项与客户端和服务器Channel相关联了。
```
public class BootstrapClientWithOptionsAndAttrs {

    public void bootstrap() {
        final AttributeKey<Integer> id = AttributeKey.newInstance("ID");
        Bootstrap bootstrap = new Bootstrap();
        bootstrap.group(new NioEventLoopGroup())
            .channel(NioSocketChannel.class)
            .handler(
                new SimpleChannelInboundHandler<ByteBuf>() {
                    @Override
                    public void channelRegistered(ChannelHandlerContext ctx)
                        throws Exception {
                        Integer idValue = ctx.channel().attr(id).get();
                        // do something with the idValue
                    }

                    @Override
                    protected void channelRead0(
                        ChannelHandlerContext channelHandlerContext,
                        ByteBuf byteBuf) throws Exception {
                        System.out.println("Received data");
                    }
                }
            );
        bootstrap.option(ChannelOption.SO_KEEPALIVE, true)
            .option(ChannelOption.CONNECT_TIMEOUT_MILLIS, 5000);
        bootstrap.attr(id, 123456);
        ChannelFuture future = bootstrap.connect(
            new InetSocketAddress("www.manning.com", 80));
        future.syncUninterruptibly();
    }
}
```
## 引导DatagramChannel
用于无连接的协议，与上述代码基于TCP协议的SocketChannel不同在于，不再调用connect()方法，只调用bind()方法
```
public class BootstrapDatagramChannel {

    public void bootstrap() {
        Bootstrap bootstrap = new Bootstrap();
        bootstrap.group(new OioEventLoopGroup()).channel(
            OioDatagramChannel.class).handler(
            new SimpleChannelInboundHandler<DatagramPacket>() {
                @Override
                public void channelRead0(ChannelHandlerContext ctx,
                    DatagramPacket msg) throws Exception {
                    // Do something with the packet
                }
            }
        );
        ChannelFuture future = bootstrap.bind(new InetSocketAddress(0));
        future.addListener(new ChannelFutureListener() {
            @Override
            public void operationComplete(ChannelFuture channelFuture)
               throws Exception {
               if (channelFuture.isSuccess()) {
                   System.out.println("Channel bound");
               } else {
                   System.err.println("Bind attempt failed");
                   channelFuture.cause().printStackTrace();
               }
            }
        });
    }
}
```
## 关闭
需要关闭EventLoopGroup，它将处理任何挂起的事件和任务，释放所有活动的线程。EventLoopGroup.shutdownGracefully()的作用。该方法将返回一个Future，这个Future将在关闭完成时接受通知。该方法是异步方法，需要阻塞等待它完成。
```
public class GracefulShutdown {
    public static void main(String args[]) {
        GracefulShutdown client = new GracefulShutdown();
        client.bootstrap();
    }

    public void bootstrap() {
        EventLoopGroup group = new NioEventLoopGroup();
        Bootstrap bootstrap = new Bootstrap();
        bootstrap.group(group)
             .channel(NioSocketChannel.class)
        //...
             .handler(
                new SimpleChannelInboundHandler<ByteBuf>() {
                    @Override
                    protected void channelRead0(
                            ChannelHandlerContext channelHandlerContext,
                            ByteBuf byteBuf) throws Exception {
                        System.out.println("Received data");
                    }
                }
             );
        bootstrap.connect(new InetSocketAddress("www.manning.com", 80)).syncUninterruptibly();
        //,,,
        Future<?> future = group.shutdownGracefully();
        // block until the group has shutdown
        future.syncUninterruptibly();
    }
}
```
也可以在调用EventLoopGroup.shutdownGracefully()方法之前，显示地在所有活动的Channel上调用Channel.close()。但在任何情况下，都要记得关闭EventLoopGroup本身。
# 单元测试
## 使用EmbeddedChannel测试ChannelHandler
### 测试入站消息
#### 代测试代码 解码器将产生固定为3字节大小的帧
```
public class FixedLengthFrameDecoder extends ByteToMessageDecoder {
    private final int frameLength;

    public FixedLengthFrameDecoder(int frameLength) {
        if (frameLength <= 0) {
            throw new IllegalArgumentException(
                "frameLength must be a positive integer: " + frameLength);
        }
        this.frameLength = frameLength;
    }

    @Override
    protected void decode(ChannelHandlerContext ctx, ByteBuf in,
        List<Object> out) throws Exception {
        while (in.readableBytes() >= frameLength) {
            ByteBuf buf = in.readBytes(frameLength);
            out.add(buf);
        }
    }
}
```
#### 单元测试
```
public class FixedLengthFrameDecoderTest {
    @Test
    public void testFramesDecoded() {
        ByteBuf buf = Unpooled.buffer();
        for (int i = 0; i < 9; i++) {
            buf.writeByte(i);
        }
        ByteBuf input = buf.duplicate();
        EmbeddedChannel channel = new EmbeddedChannel(
            new FixedLengthFrameDecoder(3)); // 以3字节的帧长度被测试
        // write bytes
        assertTrue(channel.writeInbound(input.retain()));
        assertTrue(channel.finish());

        // read messages
        ByteBuf read = (ByteBuf) channel.readInbound();
        assertEquals(buf.readSlice(3), read); // 验证是否有3帧（切片），其中每帧都为3字节
        read.release();

        read = (ByteBuf) channel.readInbound();
        assertEquals(buf.readSlice(3), read);
        read.release();

        read = (ByteBuf) channel.readInbound();
        assertEquals(buf.readSlice(3), read);
        read.release();

        assertNull(channel.readInbound());
        buf.release();
    }

    @Test
    public void testFramesDecoded2() {
        ByteBuf buf = Unpooled.buffer();
        for (int i = 0; i < 9; i++) {
            buf.writeByte(i);
        }
        ByteBuf input = buf.duplicate();

        EmbeddedChannel channel = new EmbeddedChannel(
            new FixedLengthFrameDecoder(3));
        assertFalse(channel.writeInbound(input.readBytes(2))); // 返回 false，因为没有一个完整的可供读取的帧
        assertTrue(channel.writeInbound(input.readBytes(7)));
        assertTrue(channel.finish());
        ByteBuf read = (ByteBuf) channel.readInbound();
        assertEquals(buf.readSlice(3), read);
        read.release();

        read = (ByteBuf) channel.readInbound();
        assertEquals(buf.readSlice(3), read);
        read.release();

        read = (ByteBuf) channel.readInbound();
        assertEquals(buf.readSlice(3), read);
        read.release();

        assertNull(channel.readInbound());
        buf.release();
    }
}
```













































