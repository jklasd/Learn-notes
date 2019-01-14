# rabbitMQ 注册流程

## RabbitListenerAnnotationBeanPostProcessor#postProcessAfterInitialization

#### 1、findListenerAnnotations获取所有关于类的@RabbitListener ==> Collection\<RabbitListener>

        @RabbitListener
        public class MqConsumerListener {
            ...
        }

#### 2、(spring 反射工具类)ReflectionUtils#doWithMethods，遍历findListenerAnnotations获取所有关于方法的@RabbitListener
        
        @RabbitListener(queues="queue")
        public void refundCallBack(String msg) {
        }

#### 3、遍历中处理队列 #processAmqpListener    
   - 构建MethodRabbitListenerEndpoint绑定对应的队列方法
   - 通过RabbitListenerEndpointRegistrar注册 MethodRabbitListenerEndpoint 和 RabbitListenerContainerFactory 绑定
            
            RabbitListenerEndpointRegistrar#registerEndpoint(endpoint, factory)
   - 绑定 RabbitListenerEndpoint 和 MessageListenerContainer
        
            RabbitListenerEndpointRegistrar#registerListenerContainer(endpoint, factory,boolean startImmediately)
            
   - RabbitListenerContainerFactory创建ListenerContainer，
            
            RabbitListenerEndpointRegistry#createListenerContainer(endpoint, factory)
            ...
            MessageListenerContainer listenerContainer = factory.createListenerContainer(endpoint)
            ...
            C instance = createContainerInstance();// C extends AbstractMessageListenerContainer => MessageListenerContainer
            endpoint.setupListenerContainer(instance);
   - 通过endpoint.setupListenerContainer(container) 构建 MessagingMessageListenerAdapter
            
            MessageListener messageListener = createMessageListener(container);
            //createMessageListener是通过MethodRabbitListenerEndpoint构建MessagingMessageListenerAdapter
            
            container.setupMessageListener(messageListener);
            
            
   - 最终通过适配器MessagingMessageListenerAdapter绑定对应的Bean 和 Method
   
#### 4、所有的消息处理
    
    org.springframework.amqp.rabbit.listener.adapter.MessagingMessageListenerAdapter#onMessage