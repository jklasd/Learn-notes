#undertow 学习
##一、io.undertow.Undertow.java
   - 构建容器所需要的属性。

	private Undertow(Builder builder) {
        this.byteBufferPool = builder.byteBufferPool;
        this.bufferSize = byteBufferPool != null ? byteBufferPool.getBufferSize() : builder.bufferSize;
        this.directBuffers = byteBufferPool != null ? byteBufferPool.isDirect() : builder.directBuffers;
        this.ioThreads = builder.ioThreads;
        this.workerThreads = builder.workerThreads;
        this.listeners.addAll(builder.listeners);
        this.rootHandler = builder.handler;
        this.worker = builder.worker;
        this.internalWorker = builder.worker == null;
        this.workerOptions = builder.workerOptions.getMap();
        this.socketOptions = builder.socketOptions.getMap();
        this.serverOptions = builder.serverOptions.getMap();
    }
    
  
  - 构建参数默认值
    - 构建ioThreads处理线程
    - 构建后台workerThreads处理线程
    - 构建directBuffers
    - 构建bufferSize
  
	private Builder() {
        ioThreads = Math.max(Runtime.getRuntime().availableProcessors(), 2);
        workerThreads = ioThreads * 8;
        long maxMemory = Runtime.getRuntime().maxMemory();
        //smaller than 64mb of ram we use 512b buffers
        if (maxMemory < 64 * 1024 * 1024) {
            //use 512b buffers
            directBuffers = false;
            bufferSize = 512;
        } else if (maxMemory < 128 * 1024 * 1024) {
            //use 1k buffers
            directBuffers = true;
            bufferSize = 1024;
        } else {
            //use 16k buffers for best performance
            //as 16k is generally the max amount of data that can be sent in a single write() call
            directBuffers = true;
            bufferSize = 1024 * 16 - 20; //the 20 is to allow some space for protocol headers, see UNDERTOW-1209
        }
    }
    1、Runtime.getRuntime().availableProcessors() 返回Java虚拟机可用的处理器数。
    2、Runtime.getRuntime().maxMemory() 返回Java虚拟机将尝试使用的最大内存量。
  

##二、ListenerConfig、ChannelListener、AbstractServerConnection、HttpHandler 

io.undertow.Undertow.ListenerConfig

org.xnio.ChannelListener

io.undertow.server.AbstractServerConnection

io.undertow.server.HttpHandler