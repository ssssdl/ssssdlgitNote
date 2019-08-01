--- 
为毕设学一门框架，顺序写的有点乱，但是主要是一些配置，顺序也不是也别重要，能跑起来就行，建议多联系一下
---

> 记录一个小知识java中单引号引用的是char类型，智能有一个字符，双引号的String类型(感觉我就想没学过java一样)  

# 创建工程
- File->new->module,使用maven创建工程
- 选好JDK，勾选Create from archetype，选择org.apache.maven.archetypes:maven-archetype-webapp,next
- 然后是一个项目名称，会出现在pom.xml里面，GroupID类似包名，ArtifactID是项目名
- 接下来是选择maven的一些属性，添加archetypeCatalog:internal这一对键值对会加快项目构建，好像不加也没有慢多少，然后next->Finish

# 构建目录
- main下添加源代码目录和资源目录：先创建两个目录，分别右键Mark Directory as->然后一个选成Sources Root、另一个选Resources Root，

# 导入包
- 修改pom.xml文件，在`<dependencies>`标签下的`<dependency>`用于导入包，`<priperties>`标签用于定制一些全局的变量,使用`${标签名}`进行引用，可以用这这个标签设置一些全局的版本之类的。然后这两个标签改动如下：  
```
 <properties>
    <!-- 前三个标签不用动 -->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>

    <!-- 添加统一控制springframework标签 -->
    <spring.version>5.0.2.RELEASE</spring.version>
  </properties>

<!-- 导入如下5个jar包 这样添加包之后就不用单独导入jar包 -->
  <dependencies>
<!--    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>-->

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.3</version>
      <scope>provided</scope>
    </dependency>

    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.0</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>
```

# 编辑spring的配置文件
- 在刚刚创建的resource目录下创建一个xml文件New->XML Configuration File->Spring Config，这个配置文件要在web.xml中指定，所以这里什么名字无所谓，然后修改如下
```
<?xml version="1.0" encoding="UTF-8"?>
<!-- 首先命名空间一定要正确，尤其是xsi:schemaLoaction属性，命名空间不正确可能直接导致项目启动失败 -->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="
                http://www.springframework.org/schema/beans
                http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/mvc
                http://www.springframework.org/schema/mvc/spring-mvc.xsd
                http://www.springframework.org/schema/context
                http://www.springframework.org/schema/context/spring-context.xsd">b

    <!-- 配置Spring创建容器时要扫描的包，是刚刚指定代码目录下的包可以先创建好了，就是会把这个包下指定的类创建成一个对象，以供直接调用 -->
    <context:component-scan base-package="xyz.ssssdl.controller"></context:component-scan>

    <!-- 配置视图解析器，prefix属性中value后面路径末尾要有斜线，不然就会被解析成/WEB-INF/pagesxxx.jsp,然后pages是存放jsp的路径 -->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>

    <!-- 配置Spring开启注释支持 -->
    <mvc:annotation-driven></mvc:annotation-driven>
</beans>
```

# 编辑web.xml文件
- 主要是指定一个核心的dispatcherServlet来接管所有的请求，这个dispatcherServlet已经由org.springframework.web.servlet.DispatcherServlet创建好，直接调用即可。具体如下：
```
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <!-- 指定dispatcherServlet -->
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

    <!-- 指定springmvc的配置文件为上面创建的spring -->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <!-- 指定dispatcherServlet接管所有请求 -->
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>

```

# 创建包和controller类
- 根据刚刚创建的springmvc配置文件里面指定的包（也可以先创建好），代码如下：
```
package xyz.ssssdl.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

//@Controller指定这是一个控制器
@Controller
public class helloController {
    
    //@RequestMapping(path = "/hello")，指定这个方法被访问的路径
    @RequestMapping(path = "/hello")
    public String hello(){

        //控制台输出
        System.out.println("Hello Spring");
        
        //代表会返回/WEB-INF/pages/hello.jsp界面
        return "hello";
    }
}

```

# 创建jsp界面
然后在WEB-INF/pages/下创建hello.jsp就可以了


# 总结一下
SpringMVC的调用过程如下:
- 首先用户发起请求，如果请求的是webapp目录下jsp的就直接返回，如果不是交给web.xml
- web.xml里面因为配置了/下的请求全部交给dispatcherServlet，
- dispatcherServlet就会到spring创建容器时扫描创建的对象中（springmvc配置文件）找到请求的路径，并调用对应的方法，
- 对应的方法return一个字符串，这个字符串时返回的jsp视图的文件名，
- dispatcherServlet就会到视图解析器org.springframework.web.servlet.view.InternalResourceViewResolver中取寻找对应的对应的jsp视图文件，并返回给用户