# 个人信息
姓名：罗兴惠

性别：男

联系电话： 13671457693

电子邮箱： luoxh8@qq.com

Github： https://github.com/luoxh8

个人博客：https://luoxh8.github.io/

期望薪资：16 - 18

 

# 技能描述

2年工作经验

目前使用的技术栈：

spring、git、nginx、docker、mongo、redis、mysql



擅长应用服务化

擅长多线程、多进程、协程、异步

擅长 restful、rpc 接口开发，websocket推送服务

擅长 rabbitmq 消息服务

擅长 mongodb 、redis、mysql 数据库使用

擅长 vue.js、uni-app等前端技术

擅长 postman、Charles 等接口测试、抓包工具



# 工作经历

| 广州一花互动科技有限公司 | 2020.03 - 今 |
| ------------------------ | ------------ |
|                          |              |


职位：Java开发

产品：蜂鸟支付、流盟数字营销云



**蜂鸟支付**

角色：Java开发、运维

ka/dealer地址：https://ka.fengniaopays.com/

admin地址：http://admin.fengniaopays.com/

该项目用到的镜像文件：https://hub.docker.com/r/readrou/npos8

​	

项目说明

该项目是自主研发的项目，主要是对外提供一下在支付上面的自动化的支持，同时通过收取手续费进行盈利。

由三大系统组成，admin，ka系统，dealer系统。



系统模型原型

由用户模块、任务模块、设备模块、消息推送组成

通过docker内置的nginx转发请求，到对应的web模块进行处理，同时集成了定时任务（Quartz）用于定期清除无用数据



个人工作

1. 通过评估对该项目进行分布式模块划分
2. 实现设备管理接口
3. 实现mqtt下发任务到对应的设备
4. 实现websocket实时推送信息到和客户端，进行信息的状态监控
5. 实现log日志回传模块
6. 自制docker镜像，并使用Jenkins自动发布项目



主要使用到的技术：spring boot，docker，nginx，rabbitmq，Quartz、mqtt、websocket



**流盟数字营销云**

角色：Java开发、运维

项目地址：https://ad.cqlm-tech.net/



项目说明

该项目是一个聚合的广告平台项目，主要对接了腾讯广告、朋友圈广告、以及头条广告三大模块

通过广告平台的佣金实现盈利



系统模型原型

由用户模块、任务模块、设备模块、收集模块、订单模块组成

该项目为单体应用



个人工作

1. 实现任务模块、用户模块的接口
2. 实现异步email发送service
3. 实现线程池 bean 对象
4. 实现CacheInterceptor，在标注了 @Cache 注解的接口，自动缓存其结果
5. 实现log日志回传模块
6. 编写nginx配置，添加https协议，以及子域名的分配
7. 使用wagon自动发布应用



主要使用到的技术：spring boot、wagon、rabbit、mysql、jedis、mongodb、mybatis、hibernate



| 广州海视传媒有限公司 | 2019.03 - 2020.03 |
| -------------------- | ----------------- |
|                      |                   |

职位：Java开发

产品：肇庆行政服务



**肇庆行政服务**

预览方式：微信搜索 “肇庆行政服务”，进入大厅指引，进入大厅布局，点击虚拟场景。



项目说明

该项目使用了ibeacon蓝牙设备，制作的室内定位服务

主要是让客户能够通过这个小程序模块，浏览到办事大厅的布局



系统模型原型

由全景图、坐标定位、路线计算模块组成

该项目为单体应用



个人工作

1. 实现后端api、以及管理页面
2. 编写nginx配置，添加https协议
3. 使用wagon自动发布应用



主要使用到的技术：spring boot、wagon、mysql、hibernate



# 教育经历

| 广州科技职业技术学院  大专 / Java Web开发 |
| ----------------------------------------- |
|                                           |

