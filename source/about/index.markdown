---
layout: page
title: "关于我"
date: 2013-09-02 17:36
comments: false
sharing: true
footer: true
---
* 姓名: 冯继敏
* Email: feng_jimin@foxmail.com

## 技能

* 新加坡国大计算机科学硕士，毕业论文在 Computer Vision 和 GPU 方向
* 7年以上互联网以及企业级应用开发经验
* 3年半第三方支付业务开发与管理经验
* 熟练掌握OOP, Java, Spring, ESB, .NET 以及各种基于JVM的开源框架
* 通晓 Ruby, 对Scala, Clojure 等高级编程语言和 Functional Programming 兴趣浓厚
* 通晓 Oracle/MySQL, MyBatis ORM框架
* 通晓 HTML、CSS、Javascript, jQuery, AJAX
* 通晓 Ruby on Rails 开发
* 熟悉 Linux, Mac 环境管理
* 熟悉 Git, SVN 版本控制, Maven, GEM等包管理工具

## 经历

* 2000年冬，由南京大学获新加坡教育部本科奖学金，转入新加坡国大，经历了大半年强化英语课程。
* 2001-2005年，在国大计算机学院学习计算机科学，以荣誉本科学位毕业。
* 2005-2008年， 由于得到在学校做研究工作和读研的机会，转入 postgraduate 研究，主要领域在 Computer Vision, GPU, 和 Embedded Systems，期间发表过 GPU 方向论文一篇。
* 2008-2009年，加入 NCS Singapore, 开发了一个帮助企业本地用户、邮件服务转向云服务(Google Apps) 的网络应用平台。
  * 负责了用户目录与Google Apps 的同步功能；集成了SMS群发功能，使用了JSP, JMS, Google Apps API 等技术
  * 通过这一项目得到了重要的经验教训，深刻理解了学校与业界的区别。印象比较深的是曾访问过 Google MTV 总部几天。
* 2009年10月到2010年底，加入了一个邮政信息领域的小公司[Postea](http://www.postea.com/index.shtml)，期间负责了2个小项目和2个大项目。
  * 第一个小项目是用PHP写一个SVN用户权限管理工具。
  * 另一个则是在视频监控里检测和跟踪人影，得以运用研究生所学的 Vision 知识和 OpenCV 框架。
  * 第一个由我主要负责的大项目[QubeVu](http://www.postea.com/qubevu/)，一个使用专门硬件来检测邮包大小重量，以及表单的设备。通过与设备设计者的合作，使用 .NET 和 COM+ 成功完成设备驱动, 实现了设计接口。
  * 第二个大项目是[PostMarque](http://www.postea.com/solutions/postmarque.shtml)，主要是使用 ESB 来集成邮政公司的各子系统比如在线挂号邮件系统到一个全新邮件跟踪系统，使用了 ServiceMix, Camel, ActiveMQ, Cxf 等开源框架。在短时间内得以上线。
  * 值得追忆的是做QubeVu期间到Boston出差的一个月。
* 2011－2013年初，加入 PayPal Singapore, 在 Compliance 项目部经历了多个需求至维护的完整周期，使用了PayPal前端后端的各项技术。
  * **US 报税** -- 负责的第一个部分是批处理PayPal用户报税状态查询，使用了内部的通用 batch 框架；第二部分负责更新用户 compliance 状态的 orchestration service operation；第三部分是负责前端的用户条件缺失需要做数据收集的 web flow。以上工作让我大体理解PayPal的技术框架。
  * **POS 限额** -- 实现 POS 相关的 limits service operation; 使用了一些静态和动态的 profiling 技术。
  * **KYC (Japan, RU, CA) ** -- 用户手动审核的 admin功能；后台服务非实时由用户 financial instrument status 来推导 compliance status 的逻辑；发送和接收限额的需求；
  * **Rules Engine** -- 使用iLOG规则引擎来实施用户资料更新规则处理 (BOM, XOM etc)
  * 有些项目使用了敏捷开发的实践, 如user story, 每天站立会议，当面交流，早交流等理念，很大地提高了效率。
  * 得到十分有益的经验教训，体会到在复杂系统，复杂需求的环境下，有效沟通的重大意义。
* 2013年，做为 [Greenlife 公司](http://www.greenlifeonefamily.com)的 Contractor，为其开发定制 Accounting 系统。使用 Ruby on Rails 框架。
  * 直接面对客户，积极交流理解其实际需求，设计合理的网站架构。
  * 快速得到用户反馈，促进实践敏捷开发。
  * 对网站开发有了更清晰整体的理解把握。
* 2013年11月——现在，加入国内第三方支付的初创公司，平安付，首先作为C门户和收银台前端JAVA LEAD，带领一个6人小组完成复杂紧迫的业务需求。接着作为资深开发，负责了几个主要风控项目的开发实施。
  * 从无到有实现主要的前端收银台功能：快捷支付、网银、无协议网关；支持十多种支付方式、及复杂的组合支付逻辑；实现模块的数据驱动
  * 解决了一系列问题如 XSS, CSRF with freemarker and Spring MVC; 动静分离; 使用Memcache缓存提升性能等 
  * 参与风控用户业务权限管理、交易实时信任规则的数据累计与统计等需求的设计与开发
  * 完成风控根据业务规则，生成银行卡、预付卡报表的需求
  * 在风控引入HBASE作为大数据管理的新平台，初步实现事件反查，风险用户关联等需求
  * 实践了项目管理、团队管理；参加过内部架构培训

* 通过对以下技术理念的学习，回顾以往经验，对软件开发有了更深入理解。
  * OOP, Design Patterns, IOC, AOP;
  * 高级语言如 Ruby, Scala, 函数式编程;
  * NoSQL技术如 HBase, MongoDB;
  * jQuery, JS modular框架；
  * DRY, Orthogonal design;
  * Domain Driven Design;
  * DSL;
  * SCRUM 敏捷开发。
* 实践加关联才能发现上帝的珍宝
* _沉淀是为了更好的出发，坚持才能到达目标。_
 

