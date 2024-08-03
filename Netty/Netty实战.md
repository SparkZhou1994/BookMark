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
### 测试出站信息
#### 待测试代码，将负数转为绝对值
```
public class AbsIntegerEncoder extends
    MessageToMessageEncoder<ByteBuf> {
    @Override
    protected void encode(ChannelHandlerContext channelHandlerContext,
        ByteBuf in, List<Object> out) throws Exception {
        while (in.readableBytes() >= 4) {
            int value = Math.abs(in.readInt());
            out.add(value);
        }
    }
}
```
#### 单元测试
```
public class AbsIntegerEncoderTest {
    @Test
    public void testEncoded() {
        ByteBuf buf = Unpooled.buffer();
        for (int i = 1; i < 10; i++) {
            buf.writeInt(i * -1);
        }

        EmbeddedChannel channel = new EmbeddedChannel(
            new AbsIntegerEncoder());
        assertTrue(channel.writeOutbound(buf));
        assertTrue(channel.finish());

        // read bytes
        for (int i = 1; i < 10; i++) {
            assertEquals(i, channel.readOutbound());
        }
        assertNull(channel.readOutbound());
    }
}
```
## 测试异常处理
### 待测试代码，最大帧为3字节，超出则抛异常
```
public class FrameChunkDecoder extends ByteToMessageDecoder {
    private final int maxFrameSize;

    public FrameChunkDecoder(int maxFrameSize) {
        this.maxFrameSize = maxFrameSize;
    }

    @Override
    protected void decode(ChannelHandlerContext ctx, ByteBuf in,
        List<Object> out)
        throws Exception {
        int readableBytes = in.readableBytes();
        if (readableBytes > maxFrameSize) {
            // discard the bytes
            in.clear();
            throw new TooLongFrameException();
        }
        ByteBuf buf = in.readBytes(readableBytes);
        out.add(buf);
    }
}
```
### 单元测试
```
public class FrameChunkDecoderTest {
    @Test
    public void testFramesDecoded() {
        ByteBuf buf = Unpooled.buffer();
        for (int i = 0; i < 9; i++) {
            buf.writeByte(i);
        }
        ByteBuf input = buf.duplicate();

        EmbeddedChannel channel = new EmbeddedChannel(
            new FrameChunkDecoder(3));

        assertTrue(channel.writeInbound(input.readBytes(2)));
        try {
            channel.writeInbound(input.readBytes(4));
            Assert.fail();
        } catch (TooLongFrameException e) {
            // expected exception
        }
        assertTrue(channel.writeInbound(input.readBytes(3)));
        assertTrue(channel.finish());

        // Read frames
        ByteBuf read = (ByteBuf) channel.readInbound();
        assertEquals(buf.readSlice(2), read);
        read.release();

        read = (ByteBuf) channel.readInbound();
        assertEquals(buf.skipBytes(4).readSlice(3), read);
        read.release();
        buf.release();
    }
}
```
# 编解码器框架
## 解码器
- 将字节解码为消息 -- ByteToMessageDecoder 和 ReplayingDecoder
- 将一种消息类型解码为另一种 -- MessageToMessageDecoder

解码器实现了ChannelInboundHandler
### 抽象类ByteToMessageDecoder
由于不知道远程节点是否会一次性发送完整的小i下，这个类会对入站数据进行缓冲。
- decode(ChannelHandlerContext ctx, ByteBuf in, List<Object> out) 该方法必须实现
- decode(ChannelHandlerContext ctx, ByteBuf in, List<Object> out) 当Channel的状态变为非活动是，该方法将调用一次，用于特殊的处理。

每次读取int数据时都要判断是否有4字节可读，比较繁琐，ReplayingDecoder可以消除这个步骤
#### 编解码器中的引用计数
一旦消息被编码或者解码，它就会被ReferenceCountUtil.release(message)调用自动释放，如果需要保留引用，可以调用ReferenceCountUtil.retain(message)
### 抽象类ReplayingDecoder
```
public class ToIntegerDecoder2 extends ReplayingDecoder<Void> {

    @Override
    public void decode(ChannelHandlerContext ctx, ByteBuf in,
        List<Object> out) throws Exception {
        out.add(in.readInt()); // 如果不够读，将抛出异常
    }
}
```
ReplayingDecoder慢于ByteToMessageDecoder，如果ByteToMessageDecoder不会引入太多复杂性，使用它。
### 抽象类MessageToMessageDecoder
```
public class IntegerToStringDecoder extends
    MessageToMessageDecoder<Integer> {
    @Override
    public void decode(ChannelHandlerContext ctx, Integer msg,
        List<Object> out) throws Exception {
        out.add(String.valueOf(msg));
    }
}
```
更复杂的例子，可以参考HttpObjectAggregator类
### TooLongFraeException类
当解码器缓冲大量的数据(帧超出指定的大小限制)以至于耗尽内存，将抛出异常。
异常处理取决于解码器的用户，如HTTP可能返回一个特殊的响应，而其他情况下，唯一的选择可能是关闭对应的连接。
如果在使用可变帧大小的协议，这个保护措施尤为重要。
```
public class SafeByteToMessageDecoder extends ByteToMessageDecoder {
    private static final int MAX_FRAME_SIZE = 1024;
    @Override
    public void decode(ChannelHandlerContext ctx, ByteBuf in,
        List<Object> out) throws Exception {
            int readable = in.readableBytes();
            if (readable > MAX_FRAME_SIZE) {
                in.skipBytes(readable);
                throw new TooLongFrameException("Frame too big!");
        }
        // do something
        // ...
    }
}
```
## 编码器
和解码器的功能正好相反
### 抽象类MessageToByteEncoder
该类只有一个方法，而不是像解码器一样有两个。原因是解码器通常需要在Channel关闭后产生最后一个消息(所以有了decod eLast()方法)，但这个场景对编码器无意义。
下面代码将Short编码成ByteBuf的2字节
```
public class ShortToByteEncoder extends MessageToByteEncoder<Short> {
    @Override
    public void encode(ChannelHandlerContext ctx, Short msg, ByteBuf out)
        throws Exception {
        out.writeShort(msg);
    }
}
```
更复杂的例子，可以参考WebSocket08FrameEncoder类
### 抽象类MessageToMessageEncoder
关于有趣的MessageToMessageEncoder用法，可以参考ProtobufEncoder
## 抽象的编解码器类
同时实现ChannelInboundHandler和ChannelOutboundHandler接口
尽可能将这两种功能分开，最大化代码的可用性
### 抽象类ByteToMessageCodec
它结合了ByteToMessageDecoder和MessageToByteEncoder
任何协议都可以使用ByteToMessageCodec，如果SMTP编码器成字节，解码器成一个自定义的消息类型如SmtpRequest。在接收端，当响应被创建时，产生一个SmtpReponse，并编码字节返回。
### 抽象类MessageToMessageCodec
```
public class WebSocketConvertHandler extends
     MessageToMessageCodec<WebSocketFrame,
     WebSocketConvertHandler.MyWebSocketFrame> {
     @Override
     protected void encode(ChannelHandlerContext ctx,
         WebSocketConvertHandler.MyWebSocketFrame msg,
         List<Object> out) throws Exception {
         ByteBuf payload = msg.getData().duplicate().retain();
         switch (msg.getType()) {
             case BINARY:
                 out.add(new BinaryWebSocketFrame(payload));
                 break;
             case TEXT:
                 out.add(new TextWebSocketFrame(payload));
                 break;
             case CLOSE:
                 out.add(new CloseWebSocketFrame(true, 0, payload));
                 break;
             case CONTINUATION:
                 out.add(new ContinuationWebSocketFrame(payload));
                 break;
             case PONG:
                 out.add(new PongWebSocketFrame(payload));
                 break;
             case PING:
                 out.add(new PingWebSocketFrame(payload));
                 break;
             default:
                 throw new IllegalStateException(
                     "Unsupported websocket msg " + msg);}
    }

    @Override
    protected void decode(ChannelHandlerContext ctx, WebSocketFrame msg,
        List<Object> out) throws Exception {
        ByteBuf payload = msg.content().duplicate().retain();
        if (msg instanceof BinaryWebSocketFrame) {
            out.add(new MyWebSocketFrame(
                    MyWebSocketFrame.FrameType.BINARY, payload));
        } else
        if (msg instanceof CloseWebSocketFrame) {
            out.add(new MyWebSocketFrame (
                    MyWebSocketFrame.FrameType.CLOSE, payload));
        } else
        if (msg instanceof PingWebSocketFrame) {
            out.add(new MyWebSocketFrame (
                    MyWebSocketFrame.FrameType.PING, payload));
        } else
        if (msg instanceof PongWebSocketFrame) {
            out.add(new MyWebSocketFrame (
                    MyWebSocketFrame.FrameType.PONG, payload));
        } else
        if (msg instanceof TextWebSocketFrame) {
            out.add(new MyWebSocketFrame (
                    MyWebSocketFrame.FrameType.TEXT, payload));
        } else
        if (msg instanceof ContinuationWebSocketFrame) {
            out.add(new MyWebSocketFrame (
                    MyWebSocketFrame.FrameType.CONTINUATION, payload));
        } else
        {
            throw new IllegalStateException(
                    "Unsupported websocket msg " + msg);
        }
    }

    public static final class MyWebSocketFrame {
        public enum FrameType {
            BINARY,
            CLOSE,
            PING,
            PONG,
            TEXT,
            CONTINUATION
        }
        private final FrameType type;
        private final ByteBuf data;

        public MyWebSocketFrame(FrameType type, ByteBuf data) {
            this.type = type;
            this.data = data;
        }
        public FrameType getType() {
            return type;
        }
        public ByteBuf getData() {
            return data;
        }
    }
}
```
### CombinedChannelDuplexHandler类
结合一个解码器和编码器会对可重用性造成影响，但是CombinedChannelDuplexHandler提供了这个解决方案
```
public class CombinedByteCharCodec extends
    CombinedChannelDuplexHandler<ByteToCharDecoder /* extends ByteToMessageDecoder*/, CharToByteEncoder /*MessageToByteEncoder*/> {
    public CombinedByteCharCodec() {
        super(new ByteToCharDecoder(), new CharToByteEncoder());
    }
}
```
# 预置的ChannelHandler和编解码器
## 通过SSL/TLS保护Netty应用程序
为了支持SSL/TLS，Java提供了javax.net.ssl包，它的SSLContext和SSLEngine类使得实现解密和加密相当简单直接。Netty通过一个名为SslHandler的ChannelHandler实现利用了这个API，其中SslHandler在内部使用SSLEngine来完成实际的工作。
Netty还提供了使用OpenSSL工具包的SSLEngine实现。这个OpenSslEngine提供了比JDK更好的性能。
如果OpenSSL库可用，Netty默认使用OpenSslEngine，无论使用哪个实现，其API和数据流都是一致的。
```
public class SslChannelInitializer extends ChannelInitializer<Channel> { //ChannelInitializer用于在Channel注册好时设置ChannelPipeline
    private final SslContext context;
    private final boolean startTls;

    public SslChannelInitializer(SslContext context,
        boolean startTls) {
        this.context = context;
        this.startTls = startTls;
    }
    @Override
    protected void initChannel(Channel ch) throws Exception {
        SSLEngine engine = context.newEngine(ch.alloc());
        ch.pipeline().addFirst("ssl",
            new SslHandler(engine, startTls)); //作为第一个ChannelHandler
    }
}
```
大多数情况下，SslHandler是ChannelPipeline第一个ChannelHandler，这确保了其他Handler将数据处理后，才进行加密。
SslHandler具有一些方法，如在握手阶段，两个节点将相互验证并且商定一直加密方式。可通过配置SslHandler来修改它的行为，或者在SSL/TLS握手一旦完成之后提供通知，握手阶段完成之后，所有数据都将被加密。SSL/TLS握手将会自动执行。
## 构建基于Netty的HTTP/HTTPS应用程序
### HTTP解码器、编码器和编解码器
一个HTTP请求/响应可能由多个数据部分组成，它总是以一个LastHttpContent作为结束。FullHttpRequest和FullHttpResponse是特色的子类，代表完整的请求和响应。
```
public class HttpPipelineInitializer extends ChannelInitializer<Channel> {
    private final boolean client;

    public HttpPipelineInitializer(boolean client) {
        this.client = client;
    }

    @Override
    protected void initChannel(Channel ch) throws Exception {
        ChannelPipeline pipeline = ch.pipeline();
        if (client) {
            pipeline.addLast("decoder", new HttpResponseDecoder());
            pipeline.addLast("encoder", new HttpRequestEncoder());
        } else {
            pipeline.addLast("decoder", new HttpRequestDecoder());
            pipeline.addLast("encoder", new HttpResponseEncoder());
        }
    }
}
```
### 聚合HTTP信息
由于HTTP的请求和响应可能由许多部分组成，需要聚合它们以形成完整的消息。Netty提供了一个聚合器，将多个消息部分合并成FullHttpRequest或者FullHttpResponse消息。
由于消息分段需要被缓冲，直到可以转发一个完整的消息给下一个Handler,所以会有一些开销，但好处是你不必关心消息碎片了。
引入这种自动聚合机制只是添加另一个ChannelHandler罢了。
```
public class HttpAggregatorInitializer extends ChannelInitializer<Channel> {
    private final boolean isClient;

    public HttpAggregatorInitializer(boolean isClient) {
        this.isClient = isClient;
    }

    @Override
    protected void initChannel(Channel ch) throws Exception {
        ChannelPipeline pipeline = ch.pipeline();
        if (isClient) {
            pipeline.addLast("codec", new HttpClientCodec());
        } else {
            pipeline.addLast("codec", new HttpServerCodec());
        }
        pipeline.addLast("aggregator",
                new HttpObjectAggregator(512 * 1024));
    }
}
```
### HTTP压缩
对于文本数据通常建议进行压缩
Netty支持gzip和deflate编码
HTTP请求头提供所支持的压缩格式
```
Accept-Encoding: gzip,deflate
```
服务器没有义务压缩它所发送的数据
```
public class HttpCompressionInitializer extends ChannelInitializer<Channel> {
    private final boolean isClient;

    public HttpCompressionInitializer(boolean isClient) {
        this.isClient = isClient;
    }

    @Override
    protected void initChannel(Channel ch) throws Exception {
        ChannelPipeline pipeline = ch.pipeline();
        if (isClient) {
            pipeline.addLast("codec", new HttpClientCodec());
            pipeline.addLast("decompressor",
            new HttpContentDecompressor());
        } else {
            pipeline.addLast("codec", new HttpServerCodec());
            pipeline.addLast("compressor",
            new HttpContentCompressor());
        }
    }
}
```
如果使用的是JDK6或者更早的版本，需要将JZlib放入CLASSPATH中以支持压缩功能。
```
<dependency>
	<groupId>com.jcraft</groupId>
	<artifactId>jzlib</artifactId>
	<version>1.1.3</version>
</dependency>
```
### 使用HTTPS
```
public class HttpsCodecInitializer extends ChannelInitializer<Channel> {
    private final SslContext context;
    private final boolean isClient;

    public HttpsCodecInitializer(SslContext context, boolean isClient) {
        this.context = context;
        this.isClient = isClient;
    }

    @Override
    protected void initChannel(Channel ch) throws Exception {
        ChannelPipeline pipeline = ch.pipeline();
        SSLEngine engine = context.newEngine(ch.alloc());
        pipeline.addFirst("ssl", new SslHandler(engine));

        if (isClient) {
            pipeline.addLast("codec", new HttpClientCodec());
        } else {
            pipeline.addLast("codec", new HttpServerCodec());
        }
    }
}
```
### WebSocket
WebSocket提供了“在一个单个的TCP连接上提供双向的通信……结合WebSocket API……它为网页和远程服务器之间的双向通信提供了一种替代HTTP轮询的方案”
WebSocket可以用于传输任意类型的数据，很像普通的套接字。
示例代码中的类处理协议升级握手，以及3种控制帧——Close、Ping和Pong。Text和Binary数据帧会被传递给下一个ChannelHandler进行处理。
```
public class WebSocketServerInitializer extends ChannelInitializer<Channel> {
    @Override
    protected void initChannel(Channel ch) throws Exception {
        ch.pipeline().addLast(
            new HttpServerCodec(),
            new HttpObjectAggregator(65536),
            new WebSocketServerProtocolHandler("/websocket"),
            new TextFrameHandler(),
            new BinaryFrameHandler(),
            new ContinuationFrameHandler());
    }

    public static final class TextFrameHandler extends
        SimpleChannelInboundHandler<TextWebSocketFrame> {
        @Override
        public void channelRead0(ChannelHandlerContext ctx,
            TextWebSocketFrame msg) throws Exception {
            // Handle text frame
        }
    }

    public static final class BinaryFrameHandler extends
        SimpleChannelInboundHandler<BinaryWebSocketFrame> {
        @Override
        public void channelRead0(ChannelHandlerContext ctx,
            BinaryWebSocketFrame msg) throws Exception {
            // Handle binary frame
        }
    }

    public static final class ContinuationFrameHandler extends
        SimpleChannelInboundHandler<ContinuationWebSocketFrame> {
        @Override
        public void channelRead0(ChannelHandlerContext ctx,
            ContinuationWebSocketFrame msg) throws Exception {
            // Handle continuation frame
        }
    }
}
```
如果想为WebSocket添加安全性，需要将SslHandler作为第一个ChannelHandler添加到ChannelPipeline中。
## 空闲的连接和超时
|名称|描述|
|:----:|:----:|
|IdleStateHandler|当连接空闲时间太长时，触发一个IdleStateEvent事件。通过ChannelInboundHandler重写userEvent-Triggered()处理该事件|
|ReadTimeoutHandler|如果在指定的时间间隔内没有收到入站数据，抛出一个ReadTimeoutException，并关闭Channel，通过ChannelHandler中的exceptionCaught()来检测该异常|
|WriteTimeoutHandler|没有数据写入|

发送心跳信息到远程节点，如果在60秒子内没有接受或者发送任何的数据，我们将得到通知；如果没有响应，我们将连接关闭。
```
public class IdleStateHandlerInitializer extends ChannelInitializer<Channel>
    {
    @Override
    protected void initChannel(Channel ch) throws Exception {
        ChannelPipeline pipeline = ch.pipeline();
        pipeline.addLast(
                new IdleStateHandler(0, 0, 60, TimeUnit.SECONDS)); // 如果连接超过60秒没有接受或者发送任何数据，使用一个IdelStateEvent事件
        pipeline.addLast(new HeartbeatHandler()); // 检测IdleStateEvent事件
    }

    public static final class HeartbeatHandler
        extends ChannelInboundHandlerAdapter {
        private static final ByteBuf HEARTBEAT_SEQUENCE =
                Unpooled.unreleasableBuffer(Unpooled.copiedBuffer(
                "HEARTBEAT", CharsetUtil.ISO_8859_1));
        @Override
        public void userEventTriggered(ChannelHandlerContext ctx,
            Object evt) throws Exception {
            if (evt instanceof IdleStateEvent) {
                ctx.writeAndFlush(HEARTBEAT_SEQUENCE.duplicate()) // 发送心跳信息，并在发送失败时关闭该连接
                     .addListener(
                         ChannelFutureListener.CLOSE_ON_FAILURE);
            } else {
                super.userEventTriggered(ctx, evt);
            }
        }
    }
}
```
## 解码基于分隔符的协议和基于长度的协议
### 基于分隔符的协议
RFC文档正式定义的许多协议SMTP、POP3、IMAP以及Telnet都是这样的
```
public class LineBasedHandlerInitializer extends ChannelInitializer<Channel>
    {
    @Override
    protected void initChannel(Channel ch) throws Exception {
        ChannelPipeline pipeline = ch.pipeline();
        pipeline.addLast(new LineBasedFrameDecoder(64 * 1024)); // 提取帧
        pipeline.addLast(new FrameHandler()); // 接受帧
    }

    public static final class FrameHandler
        extends SimpleChannelInboundHandler<ByteBuf> {
        @Override
        public void channelRead0(ChannelHandlerContext ctx,
            ByteBuf msg) throws Exception { // 传入单个帧的内容
            // Do something with the data extracted from the frame
        }
    }
}
```
如果正在使用除了行尾符以外的分隔符分割的帧，以类似的方法使用DelimiterBasedFrameDecoder，只需要将特定的分隔符序列指定到其构造函数即可。
作为示例，定义一下协议规范：
- 每个帧由换行符分割
- 每个帧由一系列的元素组成，没有元素由空格分隔
- 一个帧的内容代表一个命令，定义为一个命令名称后跟着数目可变的参数

由此我们自定义解码器类
- Cmd 将帧(命令)的内容存储在ByteBuf中，一个ByteBuf用于名称，另一个用于参数
- CmdDecoder 从被重写了的decode()方法中获取一行字符串，从它的内容构建一个Cmd的实例
- CmdHandler 从CmdDecoder获取解码的Cmd对象，并对它进行一些处理
- CmdHandlerInitializer 定义为专门的ChannelInitializer的嵌套类，并安装到ChannelPipeline中

```
public class CmdHandlerInitializer extends ChannelInitializer<Channel> {
    private static final byte SPACE = (byte)' ';
    @Override
    protected void initChannel(Channel ch) throws Exception {
        ChannelPipeline pipeline = ch.pipeline();
        pipeline.addLast(new CmdDecoder(64 * 1024)); // 提取Cmd对象并转发
        pipeline.addLast(new CmdHandler()); // 处理Cmd对象
    }

    public static final class Cmd {
        private final ByteBuf name;
        private final ByteBuf args;

        public Cmd(ByteBuf name, ByteBuf args) {
            this.name = name;
            this.args = args;
        }

        public ByteBuf name() {
            return name;
        }

        public ByteBuf args() {
            return args;
        }
    }

    public static final class CmdDecoder extends LineBasedFrameDecoder {
        public CmdDecoder(int maxLength) {
            super(maxLength);
        }

        @Override
        protected Object decode(ChannelHandlerContext ctx, ByteBuf buffer)
            throws Exception {
            ByteBuf frame = (ByteBuf) super.decode(ctx, buffer); // 根据行尾符分割帧
            if (frame == null) {
                return null;
            }
            int index = frame.indexOf(frame.readerIndex(),
                    frame.writerIndex(), SPACE); // 查找第一个空格字符的索引 前面时命令名称
            return new Cmd(frame.slice(frame.readerIndex(), index),
                    frame.slice(index + 1, frame.writerIndex()));
        }
    }

    public static final class CmdHandler
        extends SimpleChannelInboundHandler<Cmd> {
        @Override
        public void channelRead0(ChannelHandlerContext ctx, Cmd msg)
            throws Exception {
            // Do something with the command
        }
    }
}
```
### 基于长度的协议
FixedLengthFrameDecoder 固定长度帧
LengthFieldBasedFrameDecoder 根据头部长度值提取帧
```
public class LengthBasedInitializer extends ChannelInitializer<Channel> {
    @Override
    protected void initChannel(Channel ch) throws Exception {
        ChannelPipeline pipeline = ch.pipeline();
        pipeline.addLast(
                new LengthFieldBasedFrameDecoder(64 * 1024, 0, 8)); // 解码将帧长度编码到帧起始的前8个字节
        pipeline.addLast(new FrameHandler());
    }

    public static final class FrameHandler
        extends SimpleChannelInboundHandler<ByteBuf> {
        @Override
        public void channelRead0(ChannelHandlerContext ctx,
             ByteBuf msg) throws Exception {
            // Do something with the frame
        }
    }
}
```
## 写大型数据
由于写操作是非阻塞的，所以即使没有写出所有的数据，写操作也会在完成时返回并通知ChannelFuture。当这种情况发生时，如果仍然不停地写入，就有内存耗尽的风险。所以在写大型数据时，需要准备处理到远程节点的连接是慢速连接的情况，这种情况会导致内存释放的延迟。
在Netty应用程序中，需要做的就是使用一个FileRegion接口的实现
```
public class FileRegionWriteHandler extends ChannelInboundHandlerAdapter {
    private static final Channel CHANNEL_FROM_SOMEWHERE = new NioSocketChannel();
    private static final File FILE_FROM_SOMEWHERE = new File("");

    @Override
    public void channelActive(final ChannelHandlerContext ctx) throws Exception {
        File file = FILE_FROM_SOMEWHERE; //get reference from somewhere
        Channel channel = CHANNEL_FROM_SOMEWHERE; //get reference from somewhere
        //...
        FileInputStream in = new FileInputStream(file);
        FileRegion region = new DefaultFileRegion(
                in.getChannel(), 0, file.length());
        channel.writeAndFlush(region).addListener(
            new ChannelFutureListener() {
            @Override
            public void operationComplete(ChannelFuture future)
               throws Exception {
               if (!future.isSuccess()) {
                   Throwable cause = future.cause();
                   // Do something
               }
            }
        });
    }
}
```
该示例只适用于文件内容的直接传输，不包括应用程序对数据的任何处理。在需要将数据从文件系统复制到用户内存中时，可以使用ChunkedWriteHandler，它支持异步写大型数据流，而又不会导致大量的内存消耗。
下列所示代码使用一个File以及一个SslContext进行实例化。当initChannel()方法被调用时，它将使用所示的ChannelHandler链初始化该Channel。
当Channel的状态变为活动时，WriteStreamHandler将会逐块地把来自文件中的数据作为ChunkedStream写入。数据在传输之前将会由SslHandler加密。
```
public class ChunkedWriteHandlerInitializer
    extends ChannelInitializer<Channel> {
    private final File file;
    private final SslContext sslCtx;
    public ChunkedWriteHandlerInitializer(File file, SslContext sslCtx) {
        this.file = file;
        this.sslCtx = sslCtx;
    }

    @Override
    protected void initChannel(Channel ch) throws Exception {
        ChannelPipeline pipeline = ch.pipeline();
        pipeline.addLast(new SslHandler(sslCtx.newEngine(ch.alloc())));
        pipeline.addLast(new ChunkedWriteHandler()); // 要使用自己的ChunkedInput实现，需要在ChannelPipeline中安装一个ChunkedWriteHandler
        pipeline.addLast(new WriteStreamHandler()); // 一旦连接建立，开始写文件
    }

    public final class WriteStreamHandler
        extends ChannelInboundHandlerAdapter {

        @Override
        public void channelActive(ChannelHandlerContext ctx)
            throws Exception {
            super.channelActive(ctx);
            ctx.writeAndFlush(
            new ChunkedStream(new FileInputStream(file)));
        }
    }
}
```
## 序列化数据
### JDK序列化
Netty提供了用于和JDK进行操作的序列化类
### 使用JBoss Marshalling进行序列化
比JDK快3倍，而且更紧凑。
```
public class MarshallingInitializer extends ChannelInitializer<Channel> {
    private final MarshallerProvider marshallerProvider;
    private final UnmarshallerProvider unmarshallerProvider;

    public MarshallingInitializer(
            UnmarshallerProvider unmarshallerProvider,
            MarshallerProvider marshallerProvider) {
        this.marshallerProvider = marshallerProvider;
        this.unmarshallerProvider = unmarshallerProvider;
    }

    @Override
    protected void initChannel(Channel channel) throws Exception {
        ChannelPipeline pipeline = channel.pipeline();
        pipeline.addLast(new MarshallingDecoder(unmarshallerProvider));
        pipeline.addLast(new MarshallingEncoder(marshallerProvider));
        pipeline.addLast(new ObjectHandler());
    }

    public static final class ObjectHandler
        extends SimpleChannelInboundHandler<Serializable> {
        @Override
        public void channelRead0(
            ChannelHandlerContext channelHandlerContext,
            Serializable serializable) throws Exception {
            // Do something
        }
    }
}
```
### 通过Protocol Buffers序列化
```
public class ProtoBufInitializer extends ChannelInitializer<Channel> {
    private final MessageLite lite;

    public ProtoBufInitializer(MessageLite lite) {
        this.lite = lite;
    }

    @Override
    protected void initChannel(Channel ch) throws Exception {
        ChannelPipeline pipeline = ch.pipeline();
        pipeline.addLast(new ProtobufVarint32FrameDecoder());
        pipeline.addLast(new ProtobufEncoder());
        pipeline.addLast(new ProtobufDecoder(lite));
        pipeline.addLast(new ObjectHandler());
    }

    public static final class ObjectHandler
        extends SimpleChannelInboundHandler<Object> {
        @Override
        public void channelRead0(ChannelHandlerContext ctx, Object msg)
            throws Exception {
            // Do something with the object
        }
    }
}
```
# WebSocket
实时Web并不是所谓的硬实时服务质量(QoS)，Qos是保证计算结果将在指定的时间间隔内被提交。仅HTTP的请求响应模式设计就使其难以被支持。
实时Web使用户在信息的作者发布信息之后能够立即收到信息，而不需要他们周期性地检查信息源以获得更新。
## WebSocket示例应用程序
- 客户端发送一个消息
- 该消息被广播到所有其他连接的客户端

## 添加WebSocket支持
### 处理HTTP请求
```
public class HttpRequestHandler extends SimpleChannelInboundHandler<FullHttpRequest> {
    private final String wsUri;
    private static final File INDEX;

    static {
        URL location = HttpRequestHandler.class
             .getProtectionDomain()
             .getCodeSource().getLocation();
        try {
            String path = location.toURI() + "index.html";
            path = !path.contains("file:") ? path : path.substring(5);
            INDEX = new File(path);
        } catch (URISyntaxException e) {
            throw new IllegalStateException(
                 "Unable to locate index.html", e);
        }
    }

    public HttpRequestHandler(String wsUri) {
        this.wsUri = wsUri;
    }

    @Override
    public void channelRead0(ChannelHandlerContext ctx,
        FullHttpRequest request) throws Exception {
        if (wsUri.equalsIgnoreCase(request.getUri())) {
            ctx.fireChannelRead(request.retain()); // WebSocket协议升级
        } else {
            if (HttpHeaders.is100ContinueExpected(request)) {
                send100Continue(ctx); // 处理 100 continue 请求 以符合HTTP1.1 规则
            }
            RandomAccessFile file = new RandomAccessFile(INDEX, "r"); // 读取index.html
            HttpResponse response = new DefaultHttpResponse(
                request.getProtocolVersion(), HttpResponseStatus.OK);
            response.headers().set(
                HttpHeaders.Names.CONTENT_TYPE,
                "text/html; charset=UTF-8");
            boolean keepAlive = HttpHeaders.isKeepAlive(request);
            if (keepAlive) {
                response.headers().set(
                    HttpHeaders.Names.CONTENT_LENGTH, file.length());
                response.headers().set( HttpHeaders.Names.CONNECTION,
                    HttpHeaders.Values.KEEP_ALIVE);
            }
            ctx.write(response);
            if (ctx.pipeline().get(SslHandler.class) == null) {
                ctx.write(new DefaultFileRegion( // 将index.html写入客户端
                    file.getChannel(), 0, file.length()));
            } else {
                ctx.write(new ChunkedNioFile(file.getChannel()));
            }
            ChannelFuture future = ctx.writeAndFlush(
                LastHttpContent.EMPTY_LAST_CONTENT); // 写LastHttpContent并冲刷至客户端
            if (!keepAlive) {
                future.addListener(ChannelFutureListener.CLOSE);
            }
        }
    }

    private static void send100Continue(ChannelHandlerContext ctx) {
        FullHttpResponse response = new DefaultFullHttpResponse(
            HttpVersion.HTTP_1_1, HttpResponseStatus.CONTINUE);
        ctx.writeAndFlush(response);
    }
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause)
        throws Exception {
        cause.printStackTrace();
        ctx.close();
    }
}
```
需要调用FullHttpRequest的retain()方法，是因为调用channelRead()方法完成后，它将调用FullHttpRequest对象上的release()方法以释放它的资源。
如果客户端发送HTTP 1.1的头信息，那么HttpRequestHandler将会发送一个100 Continue响应。在该HTTP头信息设置之后，HttpRequestHandler回写一个HttpResponse给客户端。这不是一个FullHttpResponse，因为它只是响应的第一个部分。这里就不会调用writeAndFlush()方法，在结束的时候才会调用。
如果不需要加密和压缩，那么可以通过将index.html的内容存储到DefaultFileRegion中来达到最佳效果。它会利用零拷贝的特性来进行内容的传输。
HttpRequestHandler将写一个LastHttpContent来标记响应的结束。如果没有请求keep-alive，将添加ChannelFutureListener到最后一次写出动作的ChannelFuture，并关闭该连接。你将调用writeAndFlush()方法以冲刷所有之前写入的消息。
### 处理WebSocket帧
一个完整的消息可能包含许多帧
Netty提供了WebSocketServerProtocolHandler来处理其他类型的帧
```
public class TextWebSocketFrameHandler
    extends SimpleChannelInboundHandler<TextWebSocketFrame> {
    private final ChannelGroup group;

    public TextWebSocketFrameHandler(ChannelGroup group) {
        this.group = group;
    }

    @Override
    public void userEventTriggered(ChannelHandlerContext ctx,
        Object evt) throws Exception {
        if (evt == WebSocketServerProtocolHandler
             .ServerHandshakeStateEvent.HANDSHAKE_COMPLETE) {
            ctx.pipeline().remove(HttpRequestHandler.class); // 如果该事件表示握手成功，则从该ChannelPiepeline移除
            group.writeAndFlush(new TextWebSocketFrame(
                    "Client " + ctx.channel() + " joined")); // 通知所有已连接的WebSocket客户端新的客户端
            group.add(ctx.channel()); // 将新的WebSocket Channel添加到ChannelGroup
        } else {
            super.userEventTriggered(ctx, evt);
        }
    }

    @Override
    public void channelRead0(ChannelHandlerContext ctx,
        TextWebSocketFrame msg) throws Exception {
        group.writeAndFlush(msg.retain()); // 增加消息的引用计数
    }
}
```
对于retain()方法的调用是必需的，因为当channelRead0()方法返回时，TextWebSocketFrame的引用计数将会被减少。由于所有的操作都是异步的，因此，writeAndFlush()方法可能会在CchannelRead0()方法返回之后完成，而且它绝对不能访问一个已经失效的引用。
### 初始化ChannelPipeline
```
public class ChatServerInitializer extends ChannelInitializer<Channel> {
    private final ChannelGroup group;

    public ChatServerInitializer(ChannelGroup group) {
        this.group = group;
    }

    @Override
    protected void initChannel(Channel ch) throws Exception {
        ChannelPipeline pipeline = ch.pipeline();
        pipeline.addLast(new HttpServerCodec());
        pipeline.addLast(new ChunkedWriteHandler());
        pipeline.addLast(new HttpObjectAggregator(64 * 1024));
        pipeline.addLast(new HttpRequestHandler("/ws"));
        pipeline.addLast(new WebSocketServerProtocolHandler("/ws"));
        pipeline.addLast(new TextWebSocketFrameHandler(group));
    }
}
```
WebSocketServerProtocolHandler处理了所有委托管理的WebSocket帧类型以及升级握手本身。如果握手成功，那么所需的ChannelHandler将会被添加到ChannelPipeline中，而那些不再需要的ChannelHandler将被移除。
当WebSocket协议升级完成之后，WebSocketServerProtocolHandler将会把HttpRequestDecoder替换成WebSocketFrameDecoder。为了性能最大化，它将移除任何不再被WebSocket连接所需要的ChannelHandler。包括了HttpObjectAggregator和HttpRequest-Handler。
### 引导
```
public class ChatServer {
    private final ChannelGroup channelGroup =
        new DefaultChannelGroup(ImmediateEventExecutor.INSTANCE);
    private final EventLoopGroup group = new NioEventLoopGroup();
    private Channel channel;

    public ChannelFuture start(InetSocketAddress address) {
        ServerBootstrap bootstrap = new ServerBootstrap();
        bootstrap.group(group)
             .channel(NioServerSocketChannel.class)
             .childHandler(createInitializer(channelGroup));
        ChannelFuture future = bootstrap.bind(address);
        future.syncUninterruptibly();
        channel = future.channel();
        return future;
    }

    protected ChannelInitializer<Channel> createInitializer(
        ChannelGroup group) {
        return new ChatServerInitializer(group);
    }

    public void destroy() {
        if (channel != null) {
            channel.close();
        }
        channelGroup.close();
        group.shutdownGracefully();
    }

    public static void main(String[] args) throws Exception {
        if (args.length != 1) {
            System.err.println("Please give port as argument");
            System.exit(1);
        }
        int port = Integer.parseInt(args[0]);
        final ChatServer endpoint = new ChatServer();
        ChannelFuture future = endpoint.start(
                new InetSocketAddress(port));
        Runtime.getRuntime().addShutdownHook(new Thread() {
            @Override
            public void run() {
                endpoint.destroy();
            }
        });
        future.channel().closeFuture().syncUninterruptibly();
    }
}
```
## 如何进行加密
通过扩展ChantServerInitializer来创建一个SecureChatServerInitializer来完成这个需求
```
public class SecureChatServerInitializer extends ChatServerInitializer {
    private final SslContext context;

    public SecureChatServerInitializer(ChannelGroup group,
        SslContext context) {
        super(group);
        this.context = context;
    }

    @Override
    protected void initChannel(Channel ch) throws Exception {
        super.initChannel(ch);
        SSLEngine engine = context.newEngine(ch.alloc());
        engine.setUseClientMode(false);
        ch.pipeline().addFirst(new SslHandler(engine));
    }
}

public class SecureChatServer extends ChatServer {
    private final SslContext context;

    public SecureChatServer(SslContext context) {
        this.context = context;
    }

    @Override
    protected ChannelInitializer<Channel> createInitializer(
        ChannelGroup group) {
        return new SecureChatServerInitializer(group, context);
    }

    public static void main(String[] args) throws Exception {
        if (args.length != 1) {
            System.err.println("Please give port as argument");
            System.exit(1);
        }
        int port = Integer.parseInt(args[0]);
        SelfSignedCertificate cert = new SelfSignedCertificate();
        SslContext context = SslContext.newServerContext(
                cert.certificate(), cert.privateKey());
        final SecureChatServer endpoint = new SecureChatServer(context);
        ChannelFuture future = endpoint.start(new InetSocketAddress(port));
        Runtime.getRuntime().addShutdownHook(new Thread() {
            @Override
            public void run() {
                endpoint.destroy();
            }
        });
        future.channel().closeFuture().syncUninterruptibly();
    }
}
```
# 使用UDP广播事件
## UDP示例应用程序
在不安全的环境中并不倾向于使用UDP广播，路由器通常也会阻止广播消息。
## 消息POJO: LogEvent
```
public final class LogEvent {
    public static final byte SEPARATOR = (byte) ':';
    private final InetSocketAddress source;
    private final String logfile;
    private final String msg;
    private final long received;
}
```
## 编写广播者
DatagramPacket是一个简单的消息容器，DatagramChannel实现用它来和远程节点通信。
要将LogEvent消息转换成DatagramPacket，我们将需要一个编码器。只需拓展MessageToMessageEncoder。
```
public class LogEventEncoder extends MessageToMessageEncoder<LogEvent> {
    private final InetSocketAddress remoteAddress;

    public LogEventEncoder(InetSocketAddress remoteAddress) {
        this.remoteAddress = remoteAddress;
    }

    @Override
    protected void encode(ChannelHandlerContext channelHandlerContext,
        LogEvent logEvent, List<Object> out) throws Exception {
        byte[] file = logEvent.getLogfile().getBytes(CharsetUtil.UTF_8);
        byte[] msg = logEvent.getMsg().getBytes(CharsetUtil.UTF_8);
        ByteBuf buf = channelHandlerContext.alloc()
            .buffer(file.length + msg.length + 1);
        buf.writeBytes(file); // 将文件名写入ByteBuf中
        buf.writeByte(LogEvent.SEPARATOR); // 添加一个SEPARATOR
        buf.writeBytes(msg); // 将日志消息写入ByteBuf中
        out.add(new DatagramPacket(buf, remoteAddress));
    }
}

public class LogEventBroadcaster {
    private final EventLoopGroup group;
    private final Bootstrap bootstrap;
    private final File file;

    public LogEventBroadcaster(InetSocketAddress address, File file) {
        group = new NioEventLoopGroup();
        bootstrap = new Bootstrap();
        bootstrap.group(group).channel(NioDatagramChannel.class) // 引导该NioDatagramChannelcc 无连接
             .option(ChannelOption.SO_BROADCAST, true) // 设置SO_BROADCAST套接字选项
             .handler(new LogEventEncoder(address));
        this.file = file;
    }

    public void run() throws Exception {
        Channel ch = bootstrap.bind(0).sync().channel();
        long pointer = 0;
        for (;;) { // 启动主动处理循环
            long len = file.length();
            if (len < pointer) {
                // file was reset
                pointer = len; // 如果有必要，将文件指针设置到该文件的最后一个字节
            } else if (len > pointer) {
                // Content was added
                RandomAccessFile raf = new RandomAccessFile(file, "r");
                raf.seek(pointer); // 设置当前的文件指针，以确保没有任何的旧日志被发送
                String line;
                while ((line = raf.readLine()) != null) {
                    ch.writeAndFlush(new LogEvent(null, -1,
                    file.getAbsolutePath(), line)); // 对于每个日志条目，写入一个LogEvent到Channel
                }
                pointer = raf.getFilePointer(); // 存储其在文件中的当前位置
                raf.close();
            }
            try {
                Thread.sleep(1000); // 休眠1秒，如果被中断，则退出循环，否则重新处理它
            } catch (InterruptedException e) {
                Thread.interrupted();
                break;
            }
        }
    }

    public void stop() {
        group.shutdownGracefully();
    }

    public static void main(String[] args) throws Exception {
        if (args.length != 2) {
            throw new IllegalArgumentException();
        }
        LogEventBroadcaster broadcaster = new LogEventBroadcaster(
                new InetSocketAddress("255.255.255.255",
                    Integer.parseInt(args[0])), new File(args[1]));
        try {
            broadcaster.run();
        }
        finally {
            broadcaster.stop();
        }
    }
}
```
对于初始测试，可以使用netcat程序，用来监听某个指定的端口，并打印到标准输出.
```
# 监听UDP 9999端口数据
nc -l -u -p 9999
```
## 编写监视器
```
public class LogEventDecoder extends MessageToMessageDecoder<DatagramPacket> {

    @Override
    protected void decode(ChannelHandlerContext ctx,
        DatagramPacket datagramPacket, List<Object> out)
        throws Exception {
        ByteBuf data = datagramPacket.content();
        int idx = data.indexOf(0, data.readableBytes(),
            LogEvent.SEPARATOR);
        String filename = data.slice(0, idx)
            .toString(CharsetUtil.UTF_8);
        String logMsg = data.slice(idx + 1,
            data.readableBytes()).toString(CharsetUtil.UTF_8);
        LogEvent event = new LogEvent(datagramPacket.sender(),
            System.currentTimeMillis(), filename, logMsg);
        out.add(event);
    }
}

public class LogEventHandler
    extends SimpleChannelInboundHandler<LogEvent> {

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx,
        Throwable cause) throws Exception {
        cause.printStackTrace();
        ctx.close();
    }

    @Override
    public void channelRead0(ChannelHandlerContext ctx,
        LogEvent event) throws Exception {
        StringBuilder builder = new StringBuilder();
        builder.append(event.getReceivedTimestamp());
        builder.append(" [");
        builder.append(event.getSource().toString());
        builder.append("] [");
        builder.append(event.getLogfile());
        builder.append("] : ");
        builder.append(event.getMsg());
        System.out.println(builder.toString());
    }
}

public class LogEventMonitor {
    private final EventLoopGroup group;
    private final Bootstrap bootstrap;

    public LogEventMonitor(InetSocketAddress address) {
        group = new NioEventLoopGroup();
        bootstrap = new Bootstrap();
        bootstrap.group(group)
            .channel(NioDatagramChannel.class)
            .option(ChannelOption.SO_BROADCAST, true)
            .handler( new ChannelInitializer<Channel>() {
                @Override
                protected void initChannel(Channel channel)
                    throws Exception {
                    ChannelPipeline pipeline = channel.pipeline();
                    pipeline.addLast(new LogEventDecoder());
                    pipeline.addLast(new LogEventHandler());
                }
            } )
            .localAddress(address);
    }

    public Channel bind() {
        return bootstrap.bind().syncUninterruptibly().channel();
    }

    public void stop() {
        group.shutdownGracefully();
    }

    public static void main(String[] args) throws Exception {
        if (args.length != 1) {
            throw new IllegalArgumentException(
            "Usage: LogEventMonitor <port>");
        }
        LogEventMonitor monitor = new LogEventMonitor(
            new InetSocketAddress(Integer.parseInt(args[0])));
        try {
            Channel channel = monitor.bind();
            System.out.println("LogEventMonitor running");
            channel.closeFuture().sync();
        } finally {
            monitor.stop();
        }
    }
}
```
























































