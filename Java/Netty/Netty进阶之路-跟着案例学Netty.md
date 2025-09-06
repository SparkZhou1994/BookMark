# 服务端意外退出案例
## Netty服务端意外退出案例
### 如何防止Netty服务端意外退出
```
ChannelFuture f = b.bind(18080).sync();
// //main函数处于阻塞状态，后续的shutdownGracefully方法就不会被执行
// f.channel().closeFuture().sync() 
f.channel().closeFuture().addListener(new ChannelFutureListener() {
	@Override
	public void operationComplete(ChannelFuture future) {
		bossGroup.shutdownGracefully();
		workerGroup.shutdownGracefully();
	}
});
```
在实际项目中，很少用main函数直接调用Netty服务器，而是通过Tomcat或者SpringBoot
当系统退出时，建议通过调用EventLoopGroup的shutdownGracefully来完成内存队列中积压消息的处理、链路的关闭和EventLoop线程的退出，以实现停机不中断业务(单靠Netty框架实际上无法保证)
## Netty优雅退出机制
### Java优雅退出机制
通过注册JDK的ShutdownHook来实现。当系统收到退出指令，首先标记系统处于退出状态，不再接受新的消息，然后将积压的消息处理完，最后调用资源回收接口将资源销毁，各线程退出执行。
```
Runtime.getRuntime().addShutdownHook(new Thread(() -> {
	System.exit(0);
}, ""));
```
除了注册ShutdownHook，还可以通过监听信号量并注册SignalHandler的方式实现优雅退出
```
Signal sig = new Signal("INT");
Signal.handle(sig, (s)->{

});
```
### Java优雅退出的注意点
不能在ShutdownHook中调用System.exit()，它会卡住JVM，导致进程无法退出。
handle接口中一定要避免阻塞操作，如果SignalHandler执行的操作比较耗时，建议异步放到ShutdowHook中执行。
### Netty优雅退出原理和源码分析
#### NioEventLoop
- 修改线程状态为正在关闭状态(Netty5 通过加锁实现， Netty4 通过自旋锁实现)
- 把注册在selector上所有Channel都关闭
- 如果当前链路正在发送，将SeelectionKey注销操作封装成Task并放入eventLoop中执行










