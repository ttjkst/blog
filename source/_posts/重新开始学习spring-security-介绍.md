---
title: 重新开始学习spring-security-介绍
date: 2019-05-28 07:27:介绍
tags: spring，spring-Security,basic,learn
---
# 历史
spring Secuity 始于2013年的Acegi 项目，当时，在Spring Developers的邮件列表中提出了一个问题，是否对spring自身框架考虑一种安全框架实现？当时Spring社区相对较小（特别是与今天相比！），而且Spring本身从2003年初才开始作为SourceForge项目存在。对这个问题的回答是，尽管目前缺乏相应的时间，它仍是一个值得考虑的领域。
根据这一想法，开发者最初构建一个简单的安全框架的实现（但没有发布）。几周之后，spring 社区一名成员需求一个安全框架的实现，当时作者将这个简单的实现提供给他。提供这个实现之后，落干的其他的社区成员也使用这个实现。到了2004年1月份，大概有20人在使用这个实现的。这些使用这个实现的社区成员强烈建议开发者将这个实现作为spring的子项目，于是到了2004年3月，该实现成为spring 的一个子项目。
随着Acegi的项目开始专注身份验证领域，spring才开有了自己的身份验证模块。
刚开始Acegi项目还算适合spring项目的安全需求，随着越来越多的特性被添加到Acegi项目中，Acegi项目的基础架构才慢慢变得清晰.这也早期一个用户对配置文件混淆的原因，这个还被添加到JARs中。随后Acegit特殊身份鉴权服务被开发出来。大约1年后，Acegi项目正式成为Spring FrameWork的一个子项目。1.0.0的最终正式版于2006年5月发布。这个版本此前经历过2年半的社区积极使用。
现在，Spring Security成为了一个强大而又活跃的开源社区。 在Spring Security支持论坛上有成千上万的信息。 有一个积极的核心开发团队专职开发，一个积极的社区定期共享补丁并支持他们的同伴。
# 一些概念
在基本上所有的安全框架中，都会涉及的到三个概念---------用户、角色和资源。在spring security 当然也会有这三个概念，但是spring security 又将这三个概念抽象为2个过程，一是authentication(鉴权)，二是authorization(授权),
    * authentication 是对用户进入系统之前的身份认证。
    * authorization 是对用身份进行权限的限制（给与用户某些角色以及限制用户某些权限）
# spring security 适用的范围
spring security 主要适用于由spring 框架所支撑的应用，但也能在非spring 环境的进行适用。spring security 对web 类应用支撑最好，也能在非web类的桌面应用进行适用（java的。虽然已经凉凉），学习spring security 主要是使用spring security 在web应用中的配置。
# spring security 在web中能够提供什么？
使用spring security框架，能够为您的应用带来以下特性、
 * 对应用中的每一个URL进行鉴权（authentication）。 
 * 允许用户使用 用户名/密码模式 登入应用。
 * 允许用户进行登出应用。
 * 防御CSRF 攻击。
 * 防御会话固定攻击。
 * Security Headers 特性，主要包括什么，我在列一个子列表
    + HSTS（HTTP Strict Transport Security）特性
    + X-Content-Type-Options 特性
    + 静态资源缓存管理
    + X-XSS-Protection 特性
    + X-Frame-Options 特性
 * 支持一些Servlet的API 特性
    + HttpServletRequest#getRemoteUser()
    + HttpServletRequest#getUserPrincipal()
    + HttpServletRequest#isUserInRole(java.lang.String)
    + HttpServletRequest#login(java.lang.String, java.lang.String)
    + HttpServletRequest#logout()
