# Springboot框架

#### 三层架构

- controller:控制层,接受前端发送的请求,对请求进行处理,并响应数据。
- service:业务逻辑层,处理具体的业务逻辑。
- dao:数据访问层(Data Access Object)[持久层],负责数据访问操作,包括数据的增、删、改、查。

<img src="D:\桌面\三层架构.png" alt="三层架构"  />

#### 分层解耦

- 控制反转: Inversion Of Control,简称IOC。对象的创建控制权有程序自身转移到外部(容器),这种思想称为控制反转。
- 依赖注入: Dependency Injection,简称DI。容器为应用程序提供运行时所依赖的资源,称之为依赖注入。
- Bean对象:IOC容器中创建、管理的对象,称之为Bean。

###### IOC&DI入门

1. 将Dao及Service层的实现类,交给IOC容器管理。
2. 为Controller及Service注入运行时所依赖的对象。

实现方法:

@Component(加在实现类上,而非接口上)

@Autowired(自动注入)



JWT依赖

```xml
        <dependency>
            <!--JWT依赖-->
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.1</version>
        </dependency>
```

