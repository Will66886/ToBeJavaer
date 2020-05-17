# 狂神SpringMVC

学习自：https://www.bilibili.com/video/BV1aE41167Tu/

## 一、什么是SpringMVC

### 1、回顾MVC

#### 1.1、什么是MVC

- MVC是模型（Model），视图（View），控制器（Controller）的简写，是一种软件设计规范
- 是将业务逻辑、数据、显式分离的方法组织代码
- MVC不是设计模式，MVC是一种架构模式，不同的MVC存在差异

**Model（模型）：**数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或JavaBean组件（包含数据和行为），不过现在一般都分离开来：Value Object（数据Dao） 和 服务层（行为Service）。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。

**View（视图）：**负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。

**Controller（控制器）：**接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给视图，由视图负责展示。也就是说控制器做了个调度员的工作。

**最典型的MVC就是JSP + servlet + javabean的模式。**

![img](狂神SpringMVC.assets/640.png)

#### 1.2、Model1时代

- 在web早期的开发中，通常采用Model1

- Model1中主要分为两层，视图层和模型层

  ![img](狂神SpringMVC.assets/640-1589628646192.png)

  优点：架构简单，比较适合小型项目开发

  缺点：JSP职责不单一，不便于维护

#### 1.3、Model2时代

Model2把一个项目分成三部分，包括视图、控制、模型

![image-20200516195055053](狂神SpringMVC.assets/image-20200516195055053.png)

1. 用户发请求
2. Servlet接收请求数据，并调用对应的业务逻辑方法
3. 业务处理完毕，返回更新后的数据给servlet
4. servlet转向到JSP，由JSP来渲染页面
5. 响应给前端更新后的页面

##### 职责分析：

###### Controller：控制器

1. 取得表单数据
2. 调用业务逻辑

