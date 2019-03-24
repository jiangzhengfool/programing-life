### 核心api
- [NioEventLoopGroup](http://www.cnblogs.com/wangkaick/p/5097189.html)
    - shutdownGracefully()
- ServerBootstrap
    - group 指定或设置EventLoopGroup
    - channel(Class<? extends C> channelClass)   指定 Channel instances from
    - option(ChannelOption<T> option,T value)
    - handler(ChannelHandler handler)
    - childhandler(ChannelHandler childHandler)
    - bind
- ServerSocketChannel
- NioServerSocketChannel
- SocketChannel
    - pipeline()
- NioSocketChannel
- ChannelOption 配置Channel
    - SO_BACKLOG
- ChannelHandler
    - 常用实现类:
    - LoggingHandler
    - ChannelInitializer
        - initChannel(C ch)
- ChannelPipeline
    - addLast(ChannelHandler... handlers)
    - addLast(String name, ChannelHandler handler)
- ChannelFuture
    - sync()
    - channel()
codec相关的ChannelHandler
- LengthFieldBasedFrameDecoder
- LengthFieldPrepender

- ByteBuf
    - AbstractByteBuf
        - AbstractReferenceCountedByteBuf
            - PooledByteBuf
                - PooledHeadByteBuf
                - PooledDirectByteBuf
                - PooledUnsafeDirectByteBuf
            - UnpooledHeadByteBuf
            - UnpooledDirectByteBuf
            - UnpooledUnsafeDirectByteBuf
- ByteBufHolder
- ByteBufAllocator
- CompositeByteBuf
- ByteBufUtil:encodeString,decodeString,hexDump
Channel
Channel.Unsafe
ChannelPipeline
Futrue
ChannelHandler

## 开源项目
NettyRpc-master.zip
face2face-master.zip
netty-chat-master.zip
netty-demo-master.zip
netty-example-4.1.17.Final-20171007.120448-15-sources.jar

