# ruoyi

## packaging标签的作用

> 项目打包的类型: pom jar war

- pom: 父类型都为pom类型

```xml
<packaging>pom</packaging>
```

- jar: 内部调用或者是做服务使用

```xml
<packaging>jar</packaging>
```

- war: 打包项目,用于在容器(Tomcat,Jetty等) 上部署

```xml
<packaging>war</packaging>
```

> pom的意思是项目里没有java代码，也不执行任何代码，只是为了聚合工程或传递依赖用的。所以并不会寻找配置文件，若想配置文件生效，改为jar
> 
> 如果没有将packing 指定为pom ，那么子模块之间将无法正常的进行依赖传递。我们执行的maven命令的时候将首先对父项目执行，而后当 父项目 的packing 类型为 pom 时，将对所有的子模块执行同样的命令，否则将无法执行同样的命令，那么依赖的传递将无法由maven 编译或者打包命令 得以执行。
> 
> 总结：Maven-多模块项目的聚合，父项目必须将packing 指定 为 pom !!!

```xml
<!-- maven 默认的打包类型为 jar，在项目聚合的时候，需要显式的将 父项目的 packing 指定为 pom，然后再指定所属的子模块 -->
 <modules>
    <module>cats-admin</module>
    <module>cats-common</module>
    <module>cats-framework</module>
    <module>cats-generator</module>
    <module>cats-quartz</module>
    <module>cats-system</module>
 </modules>
 <packaging>pom</packaging>
```
