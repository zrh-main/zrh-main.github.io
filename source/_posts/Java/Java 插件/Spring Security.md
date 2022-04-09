---
title: Spring Security
tags:
  - Java 插件
categories:
  - Java
---

## 概述
Spring Security 的前身是 Acegi Security ，是 Spring 项目组中用来提供安全认证服务的框架。
(https://projects.spring.io/spring-security/) Spring Security 为基于J2EE企业应用软件提供了全面安全服务

安全包括两个主要操作。
“认证”，是为用户建立一个他所声明的主体。主题一般式指用户，设备或可以在你系 统中执行动作的其他系
统。
“授权”指的是一个用户能否在你的应用中执行某个操作，在到达授权判断之前，身份的主题已经由 身份验证
过程建立了。

## 使用
Maven依赖
``` xml
<dependencies>
  <dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-web</artifactId>
    <version>5.0.1.RELEASE</version>
  </dependency>
  <dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-config</artifactId>
    <version>5.0.1.RELEASE</version>
  </dependency>
</dependencies>
```
web.xml
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <display-name>Archetype Created Web Application</display-name>
    <!-- 配置加载类路径的配置文件 -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath*:applicationContext.xml,classpath*:spring-security.xml</param-value>
    </context-param>

    <!-- 配置监听器 -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!-- 配置监听器，监听request域对象的创建和销毁的 -->
    <listener>
        <listener-class>org.springframework.web.context.request.RequestContextListener</listener-class>
    </listener>

    <!-- 委派过滤器 -->
    <filter>
        <filter-name>springSecurityFilterChain</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>springSecurityFilterChain</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!-- 前端控制器（加载classpath:springmvc.xml 服务器启动创建servlet） -->
    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 配置初始化参数，创建完DispatcherServlet对象，加载springmvc.xml配置文件 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <!-- 服务器启动的时候，让DispatcherServlet对象创建 -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <welcome-file-list>
        <welcome-file>login.html</welcome-file>
        <welcome-file>login.htm</welcome-file>
        <welcome-file>login.jsp</welcome-file>
<!--        <welcome-file>index.html</welcome-file>-->
<!--        <welcome-file>index.htm</welcome-file>-->
<!--        <welcome-file>index.jsp</welcome-file>-->
<!--        <welcome-file>default.html</welcome-file>-->
<!--        <welcome-file>default.htm</welcome-file>-->
<!--        <welcome-file>default.jsp</welcome-file>-->
    </welcome-file-list>
</web-app>

```
spring security配置
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:security="http://www.springframework.org/schema/security"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/security
http://www.springframework.org/schema/security/spring-security.xsd">


    <!-- 配置不过滤的资源（静态资源及登录相关） -->
    <security:http security="none" pattern="/login.jsp"/>
    <security:http security="none" pattern="/failure.jsp"/>
    <security:http pattern="/css/**" security="none"/>
    <security:http pattern="/img/**" security="none"/>
    <security:http pattern="/plugins/**" security="none"/>
    <security:http pattern="/js/**" security="none"/>
    <security:http pattern="/WEB-INF/**" security="none"/>


    <security:http auto-config="true" use-expressions="false">

        <!-- 配置资源权限，表示任意资源路径都需要ROLE_USER权限 -->
        <security:intercept-url pattern="/**" access="ROLE_USER,ROLE_ADMIN"/>

        <!-- 自定义登陆页面，
            login-page 自定义登陆页面
            authentication-failure-url 用户权限校验失败之后才会跳转到这个页面，如果数据库中没有这个用户则不会跳转到这个页面。
            default-target-url 登陆成功后跳转的页面。
            注：登陆页面用户名固定 username，密码password，action:login
         -->
        <security:form-login
                login-page="/login.jsp"
                login-processing-url="/login" username-parameter="username"
                password-parameter="password" authentication-failure-url="/failure.jsp"
                default-target-url="/pages/main.jsp"
        ></security:form-login>

        <!-- 注销，
            invalidate-session 是否删除session
            logout-url：注销处理链接
            logout-success-url：注销成功页面
            注：注销操作 只需要链接到 logout即可注销当前用户
        -->
        <security:logout invalidate-session="true" logout-url="/logout"
                         logout-success-url="/login.jsp"/>

        <!-- 关闭CSRF,默认是开启的 关闭跨域请求-->
        <security:csrf disabled="true"/>
    </security:http>
    <security:authentication-manager>
        <security:authentication-provider user-service-ref="userService">
            <!-- 配置加密的方式 使用该方式必须对用户密码进行加密处理 -->
<!--            <security:password-encoder ref="passwordEncoder"/>-->

        </security:authentication-provider>
    </security:authentication-manager>
    <!-- 配置加密类 -->
<!--    <bean id="passwordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"/>-->
</beans>

```


## Spring Security使用数据库认证
使用UserDetails、UserDetailsService来完成操作
`UserDetails`是一个接口，作用是于封装当前进行认证的用户信息，
可以手动对其进行实现，也可以使用Spring Security提供的实现类`User`来完成操作
自定义的UserService继承UserDetailsService接口
``` Java
public interface UsersService extends UserDetailsService {
}
```
实现类
``` Java
@Service("userService")
@Transactional
public class UsersServiceImpl implements UsersService {
    @Autowired
    private UsersDao usersDao;

    //  该方法为UserDetailsService的方法,需实现该方法
    @Override
    public UserDetails loadUserByUsername(String username)  {
        System.out.println(username);
        Users user = usersDao.findByUsername(username);
        //  这里不对密码加密时需要拼接 {noop}
        //  这是security的规定
        User securityUser = new User(user.getUsername(), "{noop}" + user.getPassword(),
                user.getStatus() == 0 ? false : true, true, true, true, getAuthority(user.getRoles()));
        return securityUser;
    }

    //  该用户的角色列表
    private List<SimpleGrantedAuthority> getAuthority(List<Role> roles) {
        List<SimpleGrantedAuthority> authorities = new ArrayList();
        for (Role role : roles) {
            authorities.add(new SimpleGrantedAuthority(role.getRoleName()));
        }
        return authorities;
    }
}
```

## 用户密码加密处理
spring-security.xml
``` xml
<security:authentication-manager>
    <security:authentication-provider user-service-ref="usersService">
        <!-- 配置加密的方式 使用该方式必须对用户密码进行加密处理 -->
        <security:password-encoder ref="passwordEncoder"/>

    </security:authentication-provider>
</security:authentication-manager>
<!-- 配置加密类 -->
<bean id="passwordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"/>

```
UserServiceImpl.java
``` Java

@Service("usersService")
@Transactional
public class UsersServiceImpl implements UsersService {
    @Autowired
    private UsersDao usersDao;
    @Autowired
    private BCryptPasswordEncoder bCryptPasswordEncoder;


    @Override
    public UserDetails loadUserByUsername(String username)  {
        System.out.println(username);
        Users user = usersDao.findByUsername(username);
        //  使用加密;这里不再需要拼接 {noop}
        User securityUser = new User(user.getUsername(),  user.getPassword(),
                user.getStatus() == 0 ? false : true, true, true, true, getAuthority(user.getRoles()));
        return securityUser;
    }

    @Override
    public List<Users> findAll(Integer page, Integer pageSize) {
        PageHelper.startPage(page, pageSize);
        return usersDao.findAll();
    }

    //  保存时必须使用xml中配置的加密方式
    @Override
    public int save(Users users) {
        //  生成ID
        users.setId(UUID.randomUUID().toString().replace("-","" ).substring(0,10));
        //  bCryptPasswordEncoder.encode 密码加密
        String encode = bCryptPasswordEncoder.encode(users.getPassword());
        users.setPassword(bCryptPasswordEncoder.encode(users.getPassword()));
        return usersDao.save(users);
    }
}

```


## 注解对方法级进行权限控制
Spring Security在方法的权限控制上
支持三种类型的注解，JSR-250注解、@Secured注解和支持表达式的注解，这三种注解默认都是没有启用的，需要
单独通过global-method-security元素的对应属性进行启用
注解开启
@EnableGlobalMethodSecurity ：Spring Security默认是禁用注解的，要想开启注解，需要在继承
WebSecurityConfigurerAdapter的类上加@EnableGlobalMethodSecurity注解，并在该类中将
AuthenticationManager定义为Bean。
spring-security.xml配置文件
``` xml
<security:global-method-security jsr250-annotations="enabled"/> 
<security:global-method-security secured-annotations="enabled"/> 
<security:global-method-security pre-post-annotations="disabled"/>
<!-- 使用注解需要开启表达式  -->
<!-- use-expressions="true"开启使用表达式  -->
    <security:http auto-config="true" use-expressions="true">
        <!-- 配置具体的拦截的规则 pattern="请求路径的规则" access="访问系统的人，必须有ROLE_USER的角色" -->
        <security:intercept-url pattern="/**" access="hasAnyRole('ROLE_USER','ROLE_ADMIN')"/>

        <!-- 定义跳转的具体的页面 -->
        <security:form-login
                login-page="/login.jsp"
                login-processing-url="/login.do"
                default-target-url="/pages/main.jsp"
                authentication-failure-url="/failer.jsp"
        />

        <!-- 关闭跨域请求 -->
        <security:csrf disabled="true"/>

        <!-- 退出 -->
        <security:logout invalidate-session="true" logout-url="/logout.do" logout-success-url="/login.jsp" />

    </security:http>
```
### JSR-250注解
@RolesAllowed表示访问对应方法时所应该具有的角色
示例：
`@RolesAllowed({"USER", "ADMIN"}) 该方法只要具有"USER", "ADMIN"任意一种权限就可以访问。这里可以省略前缀ROLE_，实际的权限可能是ROLE_ADMIN`

@PermitAll表示允许所有的角色进行访问，也就是说不进行权限控制
@DenyAll是和PermitAll相反的，表示无论什么角色都不能访问

### 支持表达式的注解
@PreAuthorize 在方法调用之前,基于表达式的计算结果来限制对方法的访问
@PostAuthorize 允许方法调用,但是如果表达式计算结果为false,将抛出一个安全性异常
@PostFilter 允许方法调用,但必须按照表达式来过滤方法的结果
@PreFilter 允许方法调用,但必须在进入方法之前过滤输入值

### @Secured注解
@Secured注解标注的方法进行权限控制的支持，其值默认为disabled。
示例：
``` java

@Secured("IS_AUTHENTICATED_ANONYMOUSLY") 
public Account readAccount(Long id); 
@Secured("ROLE_TELLER")
 ```

## 页面端标签控制权限
在jsp页面中使用spring security提供的权限标签来进行权限控制
### maven导入
``` xml
<dependency> 
 <groupId>org.springframework.security</groupId> 
 <artifactId>spring-security-taglibs</artifactId> 
 <version>version</version> 
</dependency>
```
### jsp导入
``` jsp
<%@taglib uri="http://www.springframework.org/security/tags" prefix="security"%>
```
### 常用标签
**authentication**
authentication代表的是当前认证对象，可以获取当前认证对象信息
`<security:authentication property="" htmlEscape="" scope="" var=""/> `
property： 只允许指定Authentication所拥有的属性，可以进行属性的级联获取，如“principle.username”，
不允许直接通过方法进行调用
htmlEscape：表示是否需要将html进行转义。默认为true。
scope：与var属性一起使用，用于指定存放获取的结果的属性名的作用范围，默认我pageContext。Jsp中拥
有的作用范围都进行进行指定
var： 用于指定一个属性名，这样当获取到了authentication的相关信息后会将其以var指定的属性名进行存
放，默认是存放在pageConext中

**authorize**
authorize是用来判断普通权限的，通过判断用户是否具有对应的权限而控制其所包含内容的显示
`<security:authorize access="" method="" url="" var=""></security:authorize> `
access： 需要使用表达式来判断权限，当表达式的返回结果为true时表示拥有对应的权限
method：method属性是配合url属性一起使用的，表示用户应当具有指定url指定method访问的权限，
method的默认值为GET，可选值为http请求的7种方法
url：url表示如果用户拥有访问指定url的权限即表示可以显示authorize标签包含的内容
var：用于指定将权限鉴定的结果存放在pageContext的哪个属性中

**accesscontrollist**
accesscontrollist标签是用于鉴定ACL权限的。其一共定义了三个属性：hasPermission、domainObject和var，
其中前两个是必须指定的
`<security:accesscontrollist hasPermission="" domainObject="" var=""></security:accesscontrollist> `
hasPermission：hasPermission属性用于指定以逗号分隔的权限列表
domainObject：domainObject用于指定对应的域对象
var：var则是用以将鉴定的结果以指定的属性名存入pageContext中，以供同一页面的其它地方使用