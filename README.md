# 在 Spring 工程中使用 SOFATracer

在官方文档 [使用 SOFATracer 远程汇报数据到 Zipkin](https://github.com/alipay/sofa-tracer/tree/master/tracer-samples/tracer-sample-with-zipkin) 中演示了如何在SOFABoot/SpringBoot 工程成使用zipkin来上报数据。实例上 SOFATracer 的插件除了在 SOFABoot/SpringBoot 使用之外，也可以通过引入单独的插件来使用。本文中以下面三个插件使用为例，在 Spring 工程中将数据上报到zipkin。

- sofa-tracer-httpclient-plugin
- sofa-tracer-springmvc-plugin
- sofa-tracer-zipkin-plugin

## 前提条件

* Zipkin Server 的搭建可以 [参考此文档](https://zipkin.io/) 进行配置和服务端的搭建。
* 新建Spring工程  [tracer-zipkin-plugin-demo](https://github.com/glmapper/tracer-zipkin-plugin-demo/tree/master)。

* SOFATracer Github [ sofa-tracer ](https://github.com/alipay/sofa-tracer)


##  引入插件

由于 zipkin 插件是在 v2.3.0 版本才提供，在引入tracer的插件时，版本不能低于 v2.3.0 。

```xml
	<!-- springmvc 插件-->
	<dependency>
      <groupId>com.alipay.sofa</groupId>
      <artifactId>sofa-tracer-springmvc-plugin</artifactId>
      <version>${sofa.tracer.version}</version>
    </dependency>
    <!-- zipkin 插件-->
    <dependency>
      <groupId>com.alipay.sofa</groupId>
      <artifactId>sofa-tracer-zipkin-plugin</artifactId>
      <version>${sofa.tracer.version}</version>
    </dependency>
    <!-- httpclient 插件-->
    <dependency>
      <groupId>com.alipay.sofa</groupId>
      <artifactId>sofa-tracer-httpclient-plugin</artifactId>
      <version>${sofa.tracer.version}</version>
    </dependency>
    <!-- HttpClient Dependency -->
    <dependency>
      <groupId>org.apache.httpcomponents</groupId>
      <artifactId>httpclient</artifactId>
      <version>4.5.3</version>
    </dependency>
    <dependency>
      <groupId>org.apache.httpcomponents</groupId>
      <artifactId>httpasyncclient</artifactId>
      <version>4.1.3</version>
    </dependency>
```

> sofa-tracer-springmvc-plugin 是基于 标准 Servlet 实现的，因此即使是非 SpringMvc 工程，只要是标准的Servlet 工程，均可以使用该插件。


## 配置

这部分包括 filter 配置、配置文件配置、zipkin bean配置等。

### Filter 配置

在 web.xml 中配置 sofa-tracer-springmvc-plugin 插件的 Filter。 

```xml
<filter>
  <filter-name>zipkinFilter</filter-name>
  <filter-class>
    com.alipay.sofa.tracer.plugins.springmvc.SpringMvcSofaTracerFilter
  </filter-class>
</filter>
<filter-mapping>
  <filter-name>zipkinFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

### 配置文件

在 resources 目录下新建 sofa.tracer.properties 文件，并且增加 zipkin 上报所需的配置：

```properties
com.alipay.sofa.tracer.zipkin.enabled=true
com.alipay.sofa.tracer.zipkin.baseUrl=http://localhost:9411
```

### zipkin bean 配置

在 Spring 工程中，需要配置一个 bean，用于初始化 zipkin 上报所需的信息。

```xml
<bean id="zipkinReportRegisterBean" class="com.alipay.sofa.tracer.plugins.zipkin.initialize.ZipkinReportRegisterBean"/>
```



## 启动 zipkin server

分别执行下面两个命令，启动 zipkin server，默认端口是 9411

```
curl -sSL https://zipkin.io/quickstart.sh | bash -s
java -jar zipkin.jar
```

启动成功之后的界面：

![image-20181121155843918](https://github.com/glmapper/tracer-zipkin-plugin-demo/blob/master/src/main/webapp/image/index.png)

## 配置&启动tomcat

1、配置 Server

![image-20181121160008779](https://github.com/glmapper/tracer-zipkin-plugin-demo/blob/master/src/main/webapp/image/image-20181121160008779.png)

2、配置 Deployment

![image-20181121160026061](https://github.com/glmapper/tracer-zipkin-plugin-demo/blob/master/src/main/webapp/image/image-20181121160026061.png)

3、启动tomcat

![image-20181121160713218](https://github.com/glmapper/tracer-zipkin-plugin-demo/blob/master/src/main/webapp/image/image-20181121160713218.png)


## 访问资源&上报展示

1、访问资源

![image-20181121160916361](https://github.com/glmapper/tracer-zipkin-plugin-demo/blob/master/src/main/webapp/image/image-20181121160916361.png)

2、zipkin 展示

![image-20181121160851771](https://github.com/glmapper/tracer-zipkin-plugin-demo/blob/master/src/main/webapp/image/image-20181121162300360.png)
