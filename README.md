# �� Spring ������ʹ�� SOFATracer

�ڹٷ��ĵ� [ʹ�� SOFATracer Զ�̻㱨���ݵ� Zipkin](https://github.com/alipay/sofa-tracer/tree/master/tracer-samples/tracer-sample-with-zipkin) ����ʾ�������SOFABoot/SpringBoot ���̳�ʹ��zipkin���ϱ����ݡ�ʵ���� SOFATracer �Ĳ�������� SOFABoot/SpringBoot ʹ��֮�⣬Ҳ����ͨ�����뵥���Ĳ����ʹ�á��������������������ʹ��Ϊ������ Spring �����н������ϱ���zipkin��

- sofa-tracer-httpclient-plugin
- sofa-tracer-springmvc-plugin
- sofa-tracer-zipkin-plugin

## ǰ������

* Zipkin Server �Ĵ���� [�ο����ĵ�](https://zipkin.io/) �������úͷ���˵Ĵ��
* �½�Spring����  [tracer-zipkin-plugin-demo](https://github.com/glmapper/tracer-zipkin-plugin-demo/tree/master)��

* SOFATracer Github [ sofa-tracer ](https://github.com/alipay/sofa-tracer)


##  ������

���� zipkin ������� v2.3.0 �汾���ṩ��������tracer�Ĳ��ʱ���汾���ܵ��� v2.3.0 ��

```xml
	<!-- springmvc ���-->
	<dependency>
      <groupId>com.alipay.sofa</groupId>
      <artifactId>sofa-tracer-springmvc-plugin</artifactId>
      <version>${sofa.tracer.version}</version>
    </dependency>
    <!-- zipkin ���-->
    <dependency>
      <groupId>com.alipay.sofa</groupId>
      <artifactId>sofa-tracer-zipkin-plugin</artifactId>
      <version>${sofa.tracer.version}</version>
    </dependency>
    <!-- httpclient ���-->
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

> sofa-tracer-springmvc-plugin �ǻ��� ��׼ Servlet ʵ�ֵģ���˼�ʹ�Ƿ� SpringMvc ���̣�ֻҪ�Ǳ�׼��Servlet ���̣�������ʹ�øò����


## ����

�ⲿ�ְ��� filter ���á������ļ����á�zipkin bean���õȡ�

### Filter ����

�� web.xml ������ sofa-tracer-springmvc-plugin ����� Filter�� 

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

### �����ļ�

�� resources Ŀ¼���½� sofa.tracer.properties �ļ����������� zipkin �ϱ���������ã�

```properties
com.alipay.sofa.tracer.zipkin.enabled=true
com.alipay.sofa.tracer.zipkin.baseUrl=http://localhost:9411
```

### zipkin bean ����

�� Spring �����У���Ҫ����һ�� bean�����ڳ�ʼ�� zipkin �ϱ��������Ϣ��

```xml
<bean id="zipkinReportRegisterBean" class="com.alipay.sofa.tracer.spring.zipkin.initialize.ZipkinReportRegisterBean"/>
```



## ���� zipkin server

�ֱ�ִ����������������� zipkin server��Ĭ�϶˿��� 9411

```
curl -sSL https://zipkin.io/quickstart.sh | bash -s
java -jar zipkin.jar
```

�����ɹ�֮��Ľ��棺

![image-20181121155843918](/Users/guolei.sgl/Library/Application Support/typora-user-images/image-20181121155843918.png)

## ����&����tomcat

1������ Server

![image-20181121160008779](/Users/guolei.sgl/Library/Application Support/typora-user-images/image-20181121160008779.png)

2������ Deployment

![image-20181121160026061](/Users/guolei.sgl/Library/Application Support/typora-user-images/image-20181121160026061.png)

3������tomcat

![image-20181121160713218](/Users/guolei.sgl/Library/Application Support/typora-user-images/image-20181121160713218.png)


## ������Դ&�ϱ�չʾ

1��������Դ

![image-20181121160916361](/Users/guolei.sgl/Library/Application Support/typora-user-images/image-20181121160916361.png)

2��zipkin չʾ

![image-20181121160851771](/Users/guolei.sgl/Library/Application Support/typora-user-images/image-20181121160851771.png)

