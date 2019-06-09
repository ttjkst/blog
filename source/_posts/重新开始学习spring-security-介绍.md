---
title: 重新开始学习spring-security-介绍
date: 2019-05-28 07:27:介绍
tags: spring，spring-Security,basic,learn
---
# 历史
spring Secuity 始于2013年的Acegi 项目，当时，在Spring Developers的邮件列表中提出了一个问题，是否对spring自身框架考虑一种安全框架实现？当时Spring社区相对较小（特别是与今天相比！），而且Spring本身从2003年初才开始作为SourceForge项目存在。对这个问题的回答是，尽管目前缺乏相应的时间，它仍是一个值得考虑的领域。
根据这一想法，开发者最初构建一个简单的安全框架的实现（但没有发布）。几周之后，spring 社区一名成员需求一个安全框架的实现，当时作者将这个简单的实现提供给他。提供这个实现之后，落干的其他的社区成员也使用这个实现。到了2004年1月份，大概有20人在使用这个实现的。这些使用这个实现的社区成员强烈建议开发者将这个实现作为spring的子项目，于是到了2004年3月，该实现成为spring 的一个子项目。
随着Acegi的项目开始专注身份验证领域，spring才开有了自己的身份验证模块。
刚开始Acegi项目还算适合spring项目的安全需求，随着越来越多的特性被添加到Acegi项目中，Acegi项目的基础架构才慢慢变得清晰.这也早期用户对配置文件混淆的原因，这个还被添加到JARs中。随后Acegit特殊身份鉴权服务被开发出来。大约1年后，Acegi项目正式成为Spring FrameWork的一个子项目。1.0.0的最终正式版于2006年5月发布。这个版本此前经历过2年半的社区积极使用。
现在，Spring Security成为了一个强大而又活跃的开源社区。 在Spring Security支持论坛上有成千上万的信息。 有一个积极的核心开发团队专职开发，一个积极的社区定期共享补丁并支持他们的同伴。
# 一些概念
在基本上所有的安全框架中，都会涉及的到三个概念---------用户、角色和资源。在spring security 当然也会有这三个概念，但是spring security 又将这三个概念抽象为2个过程，一是authenticate(鉴权)，二是authorize(授权),
 * authenticate 是对用户进入系统之前的身份认证。
 * authorize 是对用身份进行权限的限制（给与用户某些角色以及限制用户某些权限）
# spring security 适用的范围
spring security 主要适用于由spring 框架所支撑的应用，但也能在非spring 环境的进行适用。spring security 对web 类应用支撑最好，也能在非web类的桌面应用进行适用（java的桌面。虽然已经凉凉），学习spring security 主要是使用spring security 在web应用中的配置。
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
# 这次学习spring security 学习什么？
在这次spring security 我主要重新学习spring security的配置，oauth2的配置方式，oauth2登入的方式以及如何自定义配置一个自己的spring security 认证和授权。此次学习过程，基本上不会出现xml配置方式（我是坚定的java config 配置的信仰者），本次学习的所有项目构建工具用gradle
# 最简单，从最简单开始
我在这里构建一个spring security 最简单的模型，这个模型中只涉及的authenticate过程，不涉及到authorize过程，可以理解为一个web用户登入到系统过程。
第一个编写的是身份证明认证处理提供器。
:    /**
 * 身份证明认证处理提供器。
 * 如用用户名与密码相等，则给予用户 ROLE_USER 用户的角色
 * */
public class SampleAuthenticationManager implements AuthenticationManager {
    static final List<GrantedAuthority> AUTHORITIES = Arrays.
            asList(new SimpleGrantedAuthority("ROLE_USER"));

    /**
     * @param auth 需要认证的身份证明（Authentication 在英文中为名词的认证，这里方便理解将其解释为身份证明）
     * @return 鉴权过的身份证明
     * @throws AuthenticationException 如果的用户名不等于密码 则抛出@see org.springframework.security.authentication.BadCredentialsException
     * */
    @Override
    public Authentication authenticate(Authentication auth) throws AuthenticationException {

        //当帐户用户名等于密码的时候 给与用户 ROLE_USER 角色,
        // 其他判断 用户鉴权(Authentication)失败
        if (auth.getName().equals(auth.getCredentials())) {
            return new UsernamePasswordAuthenticationToken(auth.getName(),
                    auth.getCredentials(), AUTHORITIES);
        }
        throw new BadCredentialsException("Bad Credentials");
    }
}
然后编写如果使用SampleAuthenticationManager 这个的处理器的例子。
:    public class Sample {
    private static AuthenticationManager am = new SampleAuthenticationManager();

    public static void  main(String ...args){
        theSameWillSuccess();
        System.out.println("=====================以下是鉴权失败的情景=========================");
        theDiffWillFail();
    }

    private static void theSameWillSuccess() {
        String name     ="ttjkst";
        String password ="ttjkst";
        authenicationUsernameAndPassword(name, password);
    }


    private static void theDiffWillFail() {
        String name     ="ttjkst";
        String password ="123456";
        authenicationUsernameAndPassword(name, password);
    }

    private static void authenicationUsernameAndPassword(String name, String password) {
        try {
            //构建待认证的 的身份认证
            Authentication request = new UsernamePasswordAuthenticationToken(name, password);
            //对待认证 的身份认证进行认证
            Authentication result  = am.authenticate(request);
            //将认证的结果放入 holder 中，方便后续使用
            SecurityContextHolder.getContext().setAuthentication(result);
        } catch (AuthenticationException e) {
            System.out.println("Authentication failed: " + e.getMessage());
            return;
        }
        System.out.println("Successfully authenticated. Security context contains: " +
                SecurityContextHolder.getContext().getAuthentication());
    }
}
从这个例子的我们能窥探出，基本的spring security 的authenticate的过程，这个过程的基本的模式是
1.首先的构建一个需要认证的身份证明。
2.其次将上一步构建的身份证明作为参数，调用身份证明处理器对身份证明进行authenticate。
3.如果身份证明无误，则将结果放入holder 中，方便其他组件使用。如果身份证明有问题，则抛出异常。
