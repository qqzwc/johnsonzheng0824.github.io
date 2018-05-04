---
layout: post
title: "SSH 框架学习之初识 Java 中的 Action、Dao、Service、Model."
date: 2017-06-25 
tag: ssh 
--- 

## **1.Action、Dao、Service、Model**

_首先这是现在最基本的分层方式，结合了 SSH 架构。_

**Action 层**：引用对应的 Service 层，在这里结合 Struts 的配置文件，跳转到指定的页面，当然也能接受页面传递的请求数据，也可以做些计算处理。

**Dao 层**是使用了 Hibernate 连接数据库、操作数据库（增删改查）。

**Service 层**：引用对应的 Dao 数据库操作，在这里可以编写自己需要的代码（比如简单的判断）。

**Model 层**就是对应的数据库表的实体类。

以上的 Hibernate，Struts 都需要注入到 Spring 的配置文件中，Spring 把这些联系起来，成为一个整体。

action 是 Struts 的控制层 service 是 mvc 中的服务层，model 是 java 实体 bean，dao 是与数据库进行交互持久层，ssh 中由 Hibernate 实现。

jsp 传参数给 action，action 调用 service，service 调用 dao，他们相互调用的时候传递的参数就是 model

Struts 负责前台与后台之间数据的传递、后台完成逻辑操作之后页面的跳转，后台每层之间的调用通过 Spring 进行类注入，在 service 中对 model 做出逻辑操作，然后传递给 dao，在 dao 层中用 Hibernate 对数据库进行持久化操作，根据需求，是否应有返回值。最后到 action 中进行页面跳转。

DAO 是底层与数据库直接交互的部分，service 是又对 DAO 进行了一次封装。而 service 是暴露给 action 的部分。action 里面调用 service，service 调用 DAO。

## **2\. 三大框架 Struts/Hibernate/Spring**

简单地说：

**Struts——控制用的；**

**Hibernate——操作数据库的；**

**Spring——解耦用的** （解耦就是将两种框架分离开来处理问题）

详细地说：

Struts 在 SSH 框架中起控制的作用，其核心是 Controller，即 ActionServlet，而 ActionServlet 的核心就是 Struts-config.xml，主要控制逻辑关系的处理。

Hibernate 是数据持久化层，是一种新的对象、关系的映射工具，提供了从 Java 类到数据表的映射，也提供了数据查询和恢复等机制，大大减少数据访问的复杂度。把对数据库的直接操作，转换为对持久对象的操作。

Spring 是一个轻量级的控制反转 (IoC) 和面向切面 (AOP) 的容器框架。面向接口的编程，由容器控制程序之间的依赖关系，而非传统实现中，由程序代码直接操控。这就是所谓 “**控制反转**” 的概念所在：（依赖）控制权由应用代码中转到了外部容器，控制权的转移，是所谓反转。依赖注入，即组件之间的依赖关系由容器在运行期决定，形象地说，即由容器动态地将某种依赖关系注入到组件之中，起到的主要作用是解耦。

Struts、Spring、Hibernate 在各层的作用：

（1）Struts 负责 Web 层：ActionFormBean 接收网页中表单提交的数据，然后通过 Action 进行处理，再 Forward 到对应的网页。在 Struts-config.xml 中定义 <action-mapping>，ActionServlet 会加载。

（2）Spring 负责业务层管理，即 Service（或 Manager）。

*   Service 为 action 提供统计的调用接口，封装持久层的 DAO；
*   可以写一些自己的业务方法；
*   统一的 Javabean 管理方法；
*   声明式事务管理；
*   集成 Hibernate。

（3）Hibernate，负责持久化层，完成对数据库的 crud 操作。提供 OR/Mapping。它由一组. hbm.xml 文件和 POJO，是跟数据库中的表相对应的。然后定义 DAO，这些是跟数据库打交道的类，它们会使用 PO。

## **3\. 框架业务逻辑分析**

在 Struts + Spring + Hibernate 的系统中，

对象的调用流程是：**<u>JSP—Action—Service—DAO—Hibernate</u>**。

数据的流向是：ActionFormBean 接受用户的数据，Action 将数据从 ActionFormBean 中取出，封装成 VO 或 PO，再调用业务层的 Bean 类，完成各种业务处理后再 Forward。而业务层 Bean 收到这个 PO 对象之后，会调用 DAO 接口方法，进行持久化操作。

SSH 框架的优点：

Hibernate 的最大好处就是根据数据库的表，反向生成实体类，并且还有关系在里面，还有就是它对数据的操作也很方便；

Spring，省去了在类里面 new 对象的过程，把这个调用与被调用的关系直接展示到了配置文件里，做任何操作都变得简单了。