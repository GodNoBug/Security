预备：

# 使用Spring Security开发基于表单的认证

	主要在`imooc-security-brower`写安全模块，其他地方引用
	
	之前的springboot启动，处于无保护状态。任何人只要知道服务的地址，都可以访问你的服务，这一点是绝不允许存在的。我们需要对用户做身份的验证和授权，保证合法的用户和授权的用户才能访问。Spring Security就是用来做这个事的

Spring Security 核心功能

1. 	认证（告诉系统：你是谁、通过登录过程，我们就知道是谁在使用这个系统）
	. 	授权（指定用户能干什么）
	. 	攻击防护（防止伪造身份）

## Spring Security 基本原理

```yaml
security.basic.enabled=false #是否开启基本的鉴权，默认为true,也就是说，只加入security什么都不做，那么springboot默认开启基本的鉴权，所有的服务都保护起来，需要以http方式身份验证
```

	配置类需要继承WebSecurityConfigurerAdapter，这是Spring Security提供的一个做web安全应用的适配器类，Spring Security 最核心的是一组过滤器链，访问的`请求`和`响应`都会经过Spring Security，这些过滤器在系统启动前，SpringBoot自动配置进去，这个无需开发人员关心。以下是最主要的过滤器：

BasicAuthenticationFilter：用来处理HTTPBasic登录的

UsernamePasswordAuthenticationFilter：用来处理表单登录的

ExceptionTranslationFilter

FilterSecurityInterceptor ：根据规则，过，不过，抛出异常

这些过滤器会检查请求包含的信息，比如`UsernamePasswordAuthenticationFilter`检查当前请求是否是登录请求，在登录请求里面带没带登录名和密码，如果带了，尝试用户登录，如果没带，放行到下一个过滤器。其他过滤器来解析请求。

## 第二节、自定义用户认证逻辑

密码自动生成、用户名默认是user，这不能满足需求，应该是在数据库里读取的

### 1、处理用户信息获取逻辑（怎么在数据库里读出来）

```java
	处理用户信息获取逻辑在Spring Security中是被封装在UserDetailsService：
	UserDetails	loadUserByUsername(String username) 
	根据用户传入username读取用户信息，封装到UserDetails类中，返回后Spring Security拿着这个消息去处理和校验，通过了就放入session；找不到的话，抛出 UsernameNotFoundException异常

public interface UserDetails extends Serializable {

	Collection<? extends GrantedAuthority> getAuthorities();//获取用户权限信息

	String getPassword();//获取密码

	String getUsername();//获取用户名
	//执行校验逻辑
	boolean isAccountNonExpired();//账户是否过期，如果true，没有过期
	boolean isAccountNonLocked();//账户是否被锁定、冻结
	boolean isCredentialsNonExpired();//密码是否过期
	boolean isEnabled();//账户是否可用，是否删了
}

```

### 2、处理用户校验逻辑（验证密码，是否冻结等）

	见上

### 3、处理密码的加密和界面

## 4、个性化用哪个华认证流程

	自定义登录页面 `http.formLogin().loginPage("imooc-signIn.html")`

	自定义登录成功处理  `authenticationSuccessHandler`

	自定义失败处理 `AuthenticationFailureHandler`

	异步信息而不是跳转到指定页面









实现用户名 + 密码认证

实现手机号+短信认证





	