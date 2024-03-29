﻿# 学生管理系统

## 开发环境

* 操作系统: Windows 11

* 开发工具: Intellij IDEA 2019.1.4(UItimate Edition)

* Java版本: 11.0.2

* 服务器: Tomcat 9.0

* 数据库: Server version: 8.0.11 MySQL Community Server - GPL

* 采用技术: JSP+Spring+Spring MVC+MyBatis

使用前，请运行`./ssm_sms.sql`，并修改`./src/main/resources/database_conf/db.properties`。

## 项目介绍

### 登录页面

![image-20211230124808643](project_pictures/登录页面.png)

### 系统首页

![image-20211230125033345](project_pictures/系统首页.png)

### 项目功能

* 用户登录

  * 可以以管理员、教师、学生身份登录

  * 不同身份登录所拥有权限不同

* 年级管理
  * 可对年级进行添加、更新、修改、删除
  * 可使用年级名称模糊查询年级
* 班级管理
  * 可对班级进行添加、更新、修改、删除
  * 可使用班级名称查询，学生姓名模糊查询，或二者联合查询班级
* 教师管理
  * 可对教师进行添加、更新、修改、删除
  * 可使用班级名称查询，教师姓名模糊查询，或二者联合查询教师
* 学生管理
  * 可对学生进行添加、更新、修改、删除
  * 使用班级名称查询，学生姓名模糊查询，或二者联合查询学生

### 用户权限

|              |   **管理员权限**    |    **教师权限**     | **学生权限** |
| :----------: | :-----------------: | :-----------------: | :----------: |
| 个人信息管理 |      修改密码       |      修改密码       |   修改密码   |
| 学生信息管理 | 添加/更新/修改/删除 | 添加/更新/修改/删除 |     添加     |
| 教师信息管理 | 添加/更新/修改/删除 |        添加         |      -       |
| 班级信息管理 | 添加/更新/修改/删除 |          -          |      -       |
| 年级信息管理 | 添加/更新/修改/删除 |          -          |      -       |
| 系统用户管理 | 添加/更新/修改/删除 |          -          |      -       |

## 项目实现

### 数据库

![image-20211229164212110](project_pictures/数据库.png)

### 实体类

编写实体类，包括`Admin`、`Clazz`、`Grade`、`Student`、`Teacher`、`LoginForm`。

![image-20211230125917154](project_pictures/实体类.png)

其中，`LoginForm`并不对应数据库中的表项，仅用于封装登录信息。

### 持久层

持久层使用Mybatis框架。 编写Mybatis的配置文件， 编写实体类对应的Mapper接口（增删改查方法）， 编写对应的映射配置文件。

![image-20211230203234432](project_pictures/持久层.png)

### 业务层

编写Sping的配置文件，业务层调用持久层方法，配置Sping的事务管理器，并使用`@Transactional`实现方法的事务支持。

![image-20211230203428034](project_pictures/业务层.png)

### 表现层

表现层使用SpringMVC框架。编写SpingMVC的配置文件，表现层调用业务层方法，编写`SystemController`用于登录页面，编写`CommonController`用于修改个人信息页面

![image-20211230203611974](project_pictures/表现层.png)

### 视图层

视图层使用JSP+CSS+JS，编写JSP页面。

![image-20211230203758154](project_pictures/视图层.png)

## 特色功能

### 登录验证码

 功能：登录时显示登录验证码，点击验证码可切换验证码图片，验证码输入正确时方可成功登录。

 实现：创建工具类`CreateVerificationCodeImage`，用于生成验证码图片。

![image-20211228210119076](project_pictures/登录验证码.png)

### 头像上传

 功能：添加管理员/教师/学生时可以对其上传头像，头像格式仅限于`.png`、`.jpg`、`.jepg`、`.gif`、`.bmp`，且图片大小不得超过20MB。

 实现：创建工具类`UploadFile`，用于上传文件头像，并在SpingMVC配置文件种配置`multipartResolver` 。头像上传后会自动重命名以保证文件名唯一。

![image-20211230203952397](project_pictures/头像上传.png)

### 分页功能

 功能：查询项目条目数有限制（默认当前页只显示10条），其余条目会显示在下一页。

 实现：使用Github上分页插件[Mybatis-PageHelper](https://github.com/pagehelper/Mybatis-PageHelper)，用于分页功能。在Spring的配置文件中，配置Mybatis与Spring整合时引入分页插件。

![image-20220104124950260](project_pictures/分页功能.png)

### 表单验证

 功能：会实时验证表单信息是否正确，包括手机号、密码、邮箱等填写是否正确。

 实现：使用Javascript里编写函数进行判断，并使用AJAX实时更新提示消息。

![image-20211230204224523](project_pictures/表单验证.png)

修改密码时要求密码为8~16位，且必须包含大写字母、小写字母、数字以及特殊字符，否则密码修改不通过。当输入密码满足要求时，黄色提示信息消失。

