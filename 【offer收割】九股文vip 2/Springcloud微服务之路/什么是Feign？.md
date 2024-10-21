# 👌什么是Feign？

<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">Feign是一个声明式的Web服务客户端，旨在使编写HTTP客户端变得更加简单和优雅。以下是关于Feign的详细解释：</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">一、定义与特点</font>
+ **<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">定义</font>**<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">：Feign的英文表意为“假装，伪装，变形”，它是一个http请求调用的轻量级框架，允许开发者以Java接口注解的方式调用Http请求，而无需像传统方式那样通过封装HTTP请求报文来直接调用。</font>
+ **<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">特点</font>**<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">：</font>
    - **<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">声明式接口</font>**<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">：Feign的接口是声明式的，开发者可以像调用本地方法一样调用远程接口，这大大简化了HTTP客户端的编写，降低了开发难度。</font>
    - **<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">注解支持</font>**<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">：Feign提供了丰富的注解支持，包括Feign注解和JAX-RS注解，使得开发者可以根据项目需求选择合适的注解，提高了开发效率和灵活性。</font>
    - **<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">负载均衡</font>**<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">：Feign与Spring Cloud集成后，可以利用Ribbon和Eureka实现负载均衡的HTTP客户端，这有助于在微服务架构中提高系统的可用性和稳定性。</font>
    - **<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">可插拔的编解码器</font>**<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">：Feign支持可插拔的编码器和解码器，如Jackson、Gson、JAXB等，可以根据项目的需求进行定制，以将请求和响应转换成对象。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">二、工作原理</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">当应用程序调用Feign客户端接口时，Feign会在运行时动态地生成一个代理对象。</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">代理对象通过注解来获取远程服务的信息，然后将远程调用转化为HTTP请求，发送给远程服务。</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">远程服务处理请求后，将结果返回给Feign客户端，Feign客户端再将结果转换成Java对象返回给调用者。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">三、应用场景</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">Feign主要用于微服务架构中，通过HTTP协议调用其他服务的RESTful API。</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">它简化了对RESTful API的调用，使得开发者可以更加专注于业务逻辑的实现，而不是HTTP请求的发送和接收。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">四、优势与不足</font>
+ **<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">优势</font>**<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">：</font>
    - <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">简化了HTTP客户端的编写，降低了开发难度。</font>
    - <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">提供了丰富的注解支持和可插拔的编解码器，提高了开发效率和灵活性。</font>
    - <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">与Spring Cloud集成后，可以利用Ribbon和Eureka实现负载均衡，提高了系统的可用性和稳定性。</font>
+ **<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">不足</font>**<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">：</font>
    - <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">Feign的性能相对较差，因为它是基于HTTP协议实现的，每次远程调用都需要建立TCP连接，开销比较大。相比之下，基于RPC协议的远程调用框架（如Dubbo）性能更好。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">五、使用方式</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">在项目的pom.xml文件中添加spring-cloud-starter-feign依赖。</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">在启动类上添加@SpringBootApplication和@EnableFeignClients注解，以开启Feign的功能。</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">创建一个接口，并使用Feign的注解来定义远程调用的方法。</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">在需要使用远程服务的地方，注入创建的接口实例，并直接调用其方法。Feign会自动将方法调用转换为HTTP请求，并发送到指定的服务地址。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">六、总结</font>
<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">Feign是一个高效、易用的Web服务客户端工具，它通过声明式接口和丰富的注解支持，简化了HTTP客户端的编写，降低了开发难度。在微服务架构中，Feign是调用RESTful API的优选工具之一。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ezz4sx8nbr9dlo0p>